---
status: ACTIVE_DESIGN
priority: P1
engine: Hybrid Session Sync Bridge
author: Gemini
last_updated: 2026-07-30
---
## 1. Source Metadata & Identity

*   **Official Name:** Boat24 `[VERIFIED]`

*   **Primary Domains:** `https://www.boat24.com` (with localized subpaths like `/en/`, `/de/`, `/fr/`, `/es/`) `[VERIFIED]`

*   **Geographic Focus:** Pan-European market, heavily dominant in DACH (Germany, Austria, Switzerland) and the Mediterranean `[VERIFIED]`

*   **Stable Listing ID Extraction:** Numeric offer ID cleanly extracted from the detail URL path (e.g., `.../detail/123456/` $\rightarrow$ `123456`) `[VERIFIED]`

*   **Fallback Identity Strategy:** SHA-256 hash of the normalized canonical detail URL `[DECISION]`

---

## 2. Discovery & Partitioning Strategy

*   **Primary Entry Points:** `https://www.boat24.com/en/mboats/` (Motorboats) and `https://www.boat24.com/en/sboats/` (Sailboats) `[VERIFIED]`

*   **Pagination Mechanics:** URL query parameters via `?page=N` `[HYPOTHESIS]`

*   **Page Limits:** The platform enforces a hard ceiling on the maximum number of paginated results accessible for a single broad search query (e.g., caps at 1,000 items) `[HYPOTHESIS]`

*   **Dynamic Partitioning:** To bypass the pagination ceiling and discover all ~35,850 listings, search queries must be dynamically split using strict bounds: by `price_from` / `price_to` brackets, and `year_built` boundaries `[DECISION]`

*   **Expected Update Behavior:** Daily delta scans to detect new listings and price drops `[DECISION]`

---

## 3. Acquisition & Runtime Strategy

*   **WAF & Defense Profile:** Cloudflare Bot Management featuring strict TLS fingerprinting and interactive Turnstile JavaScript challenges `[VERIFIED]`

*   **Stealth Cookie Generator:** A `nodriver` instance is launched via a DataImpulse sticky residential proxy strictly to navigate the entry challenge, solve Turnstile, and extract the valid `cf_clearance` cookie and specific User-Agent string. These credentials are saved to the local SQLite Session Store before the browser is killed `[ACTIVE_DESIGN]`

*   **Fast Bulk Fetching:** The primary crawling is executed by `curl_cffi` async workers impersonating `chrome120`. They load the saved `cf_clearance` cookies and matching TLS fingerprints from the SQLite store, allowing high-speed, low-memory extraction at ~4 requests per second `[ACTIVE_DESIGN]`

*   **CDP Asset Blocking:** For any tasks requiring `nodriver`, Chrome DevTools Protocol (CDP) commands are injected to instantly abort requests for `Image`, `Stylesheet`, `Font`, and `Media`. This drops the page payload down to ~40–75KB, preserving the 30 GB proxy budget `[DECISION]`

---

## 4. Data Availability & Extraction Map

| Field Name | Source Page | Data Type | YPI Valuation Weight | Status |
| :--- | :--- | :--- | :--- | :--- |
| `listing_id` | Detail / List | String | Mandatory | `[VERIFIED]` |
| `title` | Detail / List | String | High | `[HYPOTHESIS]` |
| `price` | Detail / List | Numeric | Mandatory | `[HYPOTHESIS]` |
| `currency` | Detail / List | String | Mandatory | `[HYPOTHESIS]` |
| `make` | Detail | String | Mandatory | `[HYPOTHESIS]` |
| `model` | Detail | String | Mandatory | `[HYPOTHESIS]` |
| `year_built` | Detail | Integer | Mandatory | `[HYPOTHESIS]` |
| `length_m` (LOA) | Detail | Float | Mandatory | `[HYPOTHESIS]` |
| `beam_m` | Detail | Float | High | `[HYPOTHESIS]` |
| `draft_m` | Detail | Float | Medium | `[HYPOTHESIS]` |
| `engine_power_hp` | Detail | String/Int | High | `[HYPOTHESIS]` |
| `location_country` | Detail | String | High | `[HYPOTHESIS]` |
| `images` | Detail | List[URL] | High | `[HYPOTHESIS]` |

---

## 5. Protected Code Map

*   **Protected Adapter Path:** `src/acquisition/protected/adapters/boat24_adapter.py` `[DECISION]`

*   **Configuration Path:** `config/protected/boat24.yaml` `[DECISION]`

*   **Test Suite Path:** `tests/protected_acquisition/test_boat24_adapter.py` `[DECISION]`

*   **Fixture Path:** `tests/fixtures/boat24/` `[DECISION]`

*   **Assigned Coding Agent:** Jules `[DECISION]`