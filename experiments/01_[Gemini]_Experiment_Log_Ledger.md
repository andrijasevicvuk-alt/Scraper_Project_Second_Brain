---
status: ACTIVE_LEDGER
author: Gemini
domain: Experimental Evidence
last_updated: 2026-07-31
---
## 1. Ledger Purpose & Rules

This ledger is the empirical source of truth for the Scraper Project's acquisition layer. **Assumption-driven development is strictly prohibited.** 

**The Golden Rule:** No strategic pipeline change—such as swapping `Selectolax` for `Scrapling`, altering proxy backoff delays, or adjusting cookie Time-To-Live (TTL) timeouts—will be accepted or merged into the production platform without recorded, reproducible evidence logged in this directory. If a WAF bypass works, we prove it here first. If an adaptive parser works, we measure its accuracy here first.

## 2. The 4-Stage Testing Pipeline

Before any target source adapter is authorized for a production Genesis Scrape, it must pass through this rigorous 4-stage experimental validation pipeline:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                      4-STAGE TESTING PIPELINE                          │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│  [STAGE 1: WAF/TLS Probe] ────────► Validates HTTP 200 vs 403 blocks   │
│           │                         using fast `curl_cffi` requests.   │
│           ▼                                                            │
│  [STAGE 2: Cookie TTL] ───────────► Measures the exact lifespan of     │
│           │                         `cf_clearance` via `nodriver`.     │
│           ▼                                                            │
│  [STAGE 3: CDP Bandwidth Audit] ──► Confirms asset interception drops  │
│           │                         page payloads to <75KB per fetch.  │
│           ▼                                                            │
│  [STAGE 4: DOM Resilience] ───────► Proves Scrapling auto-relocation   │
│                                     works accurately on mutated HTML.  │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘