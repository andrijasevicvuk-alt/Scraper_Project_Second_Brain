---
status: ACTIVE_DESIGN
author: Gemini
domain: Acquisition Architecture
last_updated: 2026-07-30
---
## 1. The Core Philosophy

Building a master dataset with a capacity target of 100,000+ active listings (currently ~96,000 raw source records) realistically requires processing well over 110,000 individual requests due to index pages, WAF challenges, and API hydration. If we executed this workload using standard headless browsers (like default Playwright), each worker would consume 150MB–300MB of RAM and download 2MB–5MB of tracking scripts and media per page. This approach would instantly choke the worker node's memory and vaporize our proxy budget.

To solve this, we rely on the **Session Sync Bridge**. This architecture treats heavy browsers not as scrapers, but as "icebreakers." We use `nodriver` solely to solve complex edge defenses (Cloudflare/Akamai). Once the security gate is cleared, we immediately extract the clearance credentials and hand the actual scraping workload over to an ultra-fast, memory-efficient HTTP client (`curl_cffi`) capable of forging browser TLS fingerprints.

## 2. Component Workflow

The engine dynamically routes requests based on the target source's defense profile. All session states are centralized to ensure seamless handoffs between the stealth browser and the fast HTTP workers.

```text
┌─────────────────────────────────────────────────────────────────────────┐
│                        INCOMING TARGET URL QUEUE                        │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
             ┌───────────────────────┴───────────────────────┐
             ▼                                               ▼
  [ ROUTE A: Low Defense ]                        [ ROUTE B: High Defense ]
(TheYachtMarket, MarineOne)                      (Boat24, iNautia, Njuškalo)
             │                                               │
             │                                               ▼
             │                                 ┌───────────────────────────┐
             │                                 │ Stealth Cookie Generator  │
             │                                 │ (`nodriver` CDP Session)  │
             │                                 │ 1. Emulate human input    │
             │                                 │ 2. Solve Cloudflare JS    │
             │                                 │ 3. Extract `cf_clearance` │
             │                                 └─────────────┬─────────────┘
             │                                               │
             │                                               ▼
             │                                 ┌───────────────────────────┐
             │                                 │   SQLite Session Store    │
             │                                 │ (Stores Cookies, JA4 TLS, │
             │                                 │  and User-Agent strings)  │
             │                                 └─────────────┬─────────────┘
             │                                               │
             ▼                                               ▼
  ┌───────────────────────────────────────────────────────────────────────┐
  │                           FAST HTTP ENGINE                            │
  │                     (`curl_cffi` Async Workers)                       │
  │     Injects matching TLS profiles and session cookies from SQLite     │
  └──────────────────────────────────┬────────────────────────────────────┘
                                     │
                                     ▼
                      To Asset Blocker & Offline Parser
```

## 3. CDP Network Asset Blocker

To secure our DataImpulse residential proxy budget, we must prevent heavy media files from passing through the proxy network. 

*   **Intercepted and Dropped:** `Image`, `Media` (videos), `Font`, `Stylesheet` (CSS), and `Other` (third-party tracking pixels).

*   **Result:** A page that normally weighs 3.5 MB is choked down to raw HTML DOM text. This drastically reduces our per-page proxy payload to **~40 KB – 75 KB**, ensuring our bandwidth scales safely to accommodate the 100,000 listing capacity target.
## 4. Yacht Quality Score (YQS)

Every raw HTML payload processed by the engine is parsed by `Selectolax` and evaluated by `Pydantic v2`. It is assigned a **Yacht Quality Score (YQS)** from `0.00` to `1.00` based on the completeness of its structured data.

### YQS Weighting Table

|**Category**|**Weight**|**Target Fields**|
|---|---|---|
|**Core Specs**|40%|Make, Model, Price, Currency, Year Built, Length Overall (LOA).|
|**Location Richness**|20%|Country, Region/State, Specific Marina/City.|
|**Secondary Specs**|20%|Engine HP, Engine Hours, Fuel Type, Beam, Draft.|
|**Media & Context**|20%|Raw description length (>200 chars), Image URL count (>= 3).|

### The Regex Fallback Parser

If the primary `Selectolax` DOM parser yields a YQS below **0.50**, it signifies that the source page relies on unstructured text (often seen on smaller broker sites like MarineOne) rather than neat specification tables.

When the `< 0.50` threshold is triggered, the system automatically engages the **Regex Fallback Parser**. This bypasses DOM tags entirely, dumping the page's raw `<body>` text and scanning it with deep regular expressions (e.g., `(?i)\b(?:length|loa)\s*:?\s*(\d{1,2}(?:\.\d{1,2})?)\s*(?:m|meters)\b`) to manually salvage floating data points before committing the record to the immutable snapshot store.