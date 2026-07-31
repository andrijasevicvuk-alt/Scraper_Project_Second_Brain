---
status: ACTIVE_ADDITIONS
author: Gemini
last_updated: 2026-07-30
---

> **IMPORTANT:** This note contains appended sections intended to supplement the original `17_Master_Inventory.md` file. It does not overwrite, delete, or modify any of the source-neutral infrastructure tracking or component statuses previously documented by Codex.

---

## 1. Updated AI Agent Responsibilities Matrix

| Agent | Core Responsibility | Primary Workspace / Folder | Commit Rights |
| :--- | :--- | :--- | :--- |
| **Vuk** | Product Owner & Merge Gatekeeper | Root Repo & Branch Management | **Yes (Sole Authority)**[cite: 11] |
| **ChatGPT**| Architecture & Contract Verification[cite: 11] | `docs/`, `01_Main_Plan.md`[cite: 11] | No[cite: 11] |
| **Gemini** | Source Research, Blueprints, Second Brain[cite: 11] | `sources/`, `architecture/`, `*.md`[cite: 11] | No[cite: 11] |
| **Jules** | Protected Source Adapter Coding[cite: 11] | `src/acquisition/protected/**`[cite: 11] | No |
| **Codex** | Source-Neutral Platform Infrastructure[cite: 11] | `src/orchestration/`, `src/database/`[cite: 11] | No[cite: 11] |

---

## 2. Complete 6-Source Target Inventory

> **Capacity Planning Target:** 100,000 active source records.

| Source Key | Priority | Volume Est. | Defense Profile | Engine Routing Strategy | Secondary Escape Hatch |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`boat24`** | P1 | 35,850 | High (Cloudflare) | Session Sync Bridge (`nodriver` + `curl_cffi`) | Direct XML Feed / Partner API |
| **`inautia`** | P2 | 31,113 | Extreme (Akamai/WAF) | Scrapling `StealthyFetcher` + Res. Proxy | Band of Boats / TopBarcos |
| **`theyachtmarket`**| P3 | 24,222 | Low (Clean 200) | Fast HTTP (`curl_cffi` + `Selectolax`) | N/A (Wide open target) |
| **`njuskalo_nautika`**| P4 | 4,777 | Extreme (Geo-Block) | `StealthyFetcher` + HR Residential IP | Index Oglasi (Nautika) |
| **`croatian_yachting`**| P5 | 158 | Medium (CSR Render) | Client-Side Renderer (`nodriver`) | Hardcoded Index Crawl |
| **`marine_one`** | P6 | ~150 | Low / Template | Fast HTTP + Deep Text Regex Parser | Unstructured Paragraph Parser |

---

## 3. Infrastructure & Cost Allocations

| Component | Configuration | Budget / Cost | Allocation Role |
| :--- | :--- | :--- | :--- |
| **Worker PC** | Ryzen 5 5600X / 16GB RAM / 1TB SSD | Existing Hardware | Headless Chrome, SQLite, Docker |
| **Proxy Allowance**| DataImpulse Residential Pool | **$30.00 USD (30 GB)** | 100,000 Capacity Target Genesis + Routine Sync |
| **Scraper Runtime**| Docker Container (`scrapling:latest`) | Free Open-Source | Non-root isolated worker environment |