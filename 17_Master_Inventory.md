# Master Inventory

## Purpose

This note records important repositories, agents, tools, machines, datasets and project responsibilities used by `scraper_project`.

## Inventory status vocabulary

- `approved`
- `active`
- `active design`
- `candidate`
- `experimental`
- `future`
- `deprecated`

Design maturity is recorded separately as:

- `hypothesis`
- `prototype_decision`
- `experiment_supported`
- `production_approved`

## Repositories and documentation

| Item | Role | Status | Source of truth |
|---|---|---|---|
| Scraper Project Second Brain | Project memory and operating manual | active | Public GitHub repository |
| `scraper_project` | Source acquisition and source-readiness platform | active | Public GitHub repository |
| YPI repository | Normalization, valuation-ready data, scoring and UI | active | YPI GitHub repository |

## AI and agent tools

| Agent | Responsibility | Status | Boundary |
|---|---|---|---|
| Vuk | Product owner, pilot gate, commit and merge authority | active | Approves architecture, maturity promotion and production |
| ChatGPT | Lead architecture, boundary audit, reviewer prompts and Second Brain consistency | active | Reviews protected code but does not directly replace it |
| Gemini | Source research, acquisition blueprints, experiments and source documentation | active | Does not implement or commit protected production code |
| Jules | Protected source-adapter implementation and protected tests | active | Returns files or patches; does not commit; no source-neutral ownership |
| Codex | Source-neutral platform, offline parsers, tests and exports | active | Does not directly repair Jules protected code |
| Antigravity | Previous protected implementation role | deprecated | Replaced by Jules in the active workflow |

## Machines

| Machine | Role | Status | Known specification |
|---|---|---|---|
| ThinkPad laptop | Control node, development, GitHub, Codex and monitoring | active | ThinkPad L15 Gen 2 |
| Home PC | Docker worker, future acquisition jobs, snapshots and runtime database | worker architecture verified 2026-09-20; current online state unknown | Ryzen 5 1600, 16 GB RAM, ~1 TB SSD, ~3 TB nominal HDD (about 2.7 TiB observed) |

## Core infrastructure

| Tool | Role | Status | Notes |
|---|---|---|---|
| GitHub | Code, documentation, migrations and reviews | approved | No secrets or runtime data |
| Obsidian | Human-readable Second Brain | approved | Not the technical source of truth |
| Docker Compose | Local container orchestration | approved / verified for synthetic worker | Extend existing scaffold; no Kubernetes |
| SQLite | Source-neutral jobs, checkpoints and runtime ledger | approved / verified | WAL mode, one authoritative queue owner |
| Tailscale | Private control-node → worker transport | verified at 2026-09-20 checkpoint | Existing OpenSSH remains the shell boundary; current reachability must be checked when needed |
| OpenSSH | Remote worker administration | verified at 2026-09-20 checkpoint | Key-only; password/root login disabled in accepted configuration |
| Protected session-state store | Cookies, identity bundles and refresh leases | prototype decision | Separate protected runtime state by default; never Git |
| PostgreSQL/Supabase | YPI structured data layer | separate YPI responsibility | Do not store all raw HTML in PostgreSQL |

## Acquisition architecture and tools

| Component | Role | Inventory status | Maturity | Owner / replacement policy |
|---|---|---|---|---|
| Protected live source adapter in current `scraper_project` main | Source-specific acquisition | not implemented | hypothesis / future Step 6 | No Boat24 protected adapter is present on current main; Jules implements only after an approved Gemini blueprint |
| Source-specific routing policy | Select acquisition route per source/page type | active design | prototype_decision | Gemini blueprint, Jules implementation |
| Session Sync Bridge | Transfer a validated browser-created identity to a compatible fast client | active design | prototype_decision | Experiment before production |
| Fast HTTP client (`curl_cffi` candidate) | Efficient list/detail acquisition where compatible | candidate | prototype_decision | Exact client and settings require experiments |
| Stealth-browser runtime (`nodriver` or approved equivalent) | Browser-mediated source session/bootstrap where required | active design | prototype_decision | Preserved as active design; validate per source |
| Protected Session Broker | Coordinate identity generation and bounded refresh | active design | prototype_decision | No queue/retry ownership |
| CDP/network asset controls | Reduce browser bandwidth | experimental | hypothesis | Source-specific allow/block rules only after measurement |
| Scrapling network fetchers | Alternative protected acquisition route | experimental | hypothesis | Must not replace approved engine without evidence and Vuk decision |
| Crawlee | Possible orchestration/acquisition component | candidate | hypothesis | Must not introduce a second retry owner |
| DataImpulse residential proxies | Residential proxy traffic | active | production-approved provider choice | Record bytes per source and run |


## Experimental Tools

These tools and techniques remain part of the project’s research space. Their presence here does not mean they are required, implemented, experiment-supported or production-approved.

Gemini researches whether they are useful for a specific source. Jules implements them only after Vuk approves the source blueprint. ChatGPT and Codex may review and suggest optimizations, but they must not silently remove, reject or replace them.

| Tool or technique | Intended role | Status | Maturity | Research owner | Implementation owner |
|---|---|---|---|---|---|
| Playwright | Browser rendering, interaction, tracing and fixture capture | experimental | hypothesis | Gemini | Jules |
| OxyMouse | Candidate human-like mouse path generation | experimental | hypothesis | Gemini | Jules |
| HumanMoveMouse | Candidate desktop-level mouse movement generation | experimental | hypothesis | Gemini | Jules |
| Jittery scrolling routines | Source-specific nonlinear scrolling and lazy-load interaction | experimental | hypothesis | Gemini | Jules |
| CDP `Input.dispatchKeyEvent` | Source-specific low-level keyboard interaction | experimental | hypothesis | Gemini | Jules |
| Header-locality synchronisation | Align source-visible locale and request profile with approved proxy configuration | experimental | hypothesis | Gemini | Jules |
| Brotli/Zstandard response configuration | Reduce response bytes where the selected client and server support it | experimental | hypothesis | Gemini | Jules |
| Route asset interception | Reduce browser proxy traffic using measured source-specific request rules | experimental | hypothesis | Gemini | Jules |
| Stagehand | AI-assisted browser research, interaction or candidate element-location experiments | experimental | hypothesis | Gemini | Jules |
| AgentQL | Semantic element-location or independent extraction-candidate experiments | experimental | hypothesis | Gemini | Jules |
| Loguru telemetry | Alternative protected acquisition logging experiment | experimental | hypothesis | Gemini | Jules |

Experimental tools are not rejected merely because ChatGPT or Codex cannot implement or recommend a particular tactic. Removal, rejection, replacement or production promotion requires evidence and Vuk’s explicit decision.

## Offline parser and resilience tools

| Component | Role | Status | Maturity | Owner |
|---|---|---|---|---|
| Selectolax or BeautifulSoup | Strict offline HTML parsing | candidate | prototype_decision per parser | Codex |
| Controlled regex fallback | Recover labelled values from unstructured text | active design | prototype_decision | Codex; evidence and warnings required |
| Scrapling adaptive relocation | Generate candidate selector/fingerprint repairs offline | experimental | hypothesis | Codex implementation, Gemini source evidence |
| Automated repair validation | High/medium/low confidence promotion workflow | active design | prototype_decision | Codex; Vuk approves policy |
| AI-assisted repair proposal | Optional selector suggestion after major layout change | future experiment | hypothesis | Never direct production mutation |
| Pydantic v2 | Contract or parser validation target | approved target | prototype_decision until installed | Shared/Codex |
| RapidFuzz | Duplicate-candidate and similarity signals | approved target | prototype_decision until implemented | Codex |
| pytest | Unit, fixture and integration testing | recommended | prototype_decision | Shared |

The current repository implementation still uses standard-library dataclasses and `unittest`; inventory approval does not claim a dependency is already installed.

## Target sources

| Source | Business role | Current state | Maturity |
|---|---|---|---|
| Boat24 | Main marketplace backbone | source specification in progress (`scraper_project Step 4`) | hypothesis until specification/evidence is approved |
| Croatian Yachting | Croatian and Adriatic broker anchor | planned | hypothesis |
| MarineOne / YachtBrokerage | Additional local broker anchor | planned | hypothesis |
| Njuškalo Nautika | Croatian marketplace coverage | planned | hypothesis |
| TheYachtMarket | Broad international and Mediterranean context | planned | hypothesis |
| iNautia | Mediterranean expansion | planned | hypothesis |
| Alternative sources | Fallback coverage | optional | hypothesis |

Exact listing counts, defense profiles and engine routes stay in source notes or experiments rather than becoming inventory facts without evidence.

## Repository visibility checkpoint

At the 2026-09-28 audit, GitHub reported the Second Brain, `scraper_project` and YPI repositories as public. This is an observed current setting, not a recommendation. See [[13_Risk_Register]] R21 and [[12_Second_Brain_Operating_System]] before adding operational details.

## Runtime storage


| Data | Location | Git status |
|---|---|---|
| Source code | GitHub and local clone | committed |
| Raw snapshots | Home-PC worker storage | ignored |
| Runtime SQLite database | Home-PC persistent volume | ignored |
| Protected session state | Home-PC protected runtime volume | ignored / never logged |
| Checkpoints | Home-PC persistent volume | ignored |
| Logs | Home-PC runtime storage | ignored and redacted |
| Dataset exports | Worker storage and approved backup | ignored |
| Parser fixtures | Repository `tests/fixtures/` | committed |
| Secrets | Runtime environment only | never committed |

## Current deliverables

- keep `scraper_project` Steps 1–3 closed unless new evidence reveals a regression;
- finalize and approve the Boat24 source specification;
- prepare the Gemini source-research/prototype-blueprint handoff;
- preserve Home-PC live work for the later evidence/fixture/pilot phases rather than making current planning depend on physical access;
- recover/review/push the local-only bounded-Docker-logging branch before long unattended live workloads;
- complete the YPI transactional publication hardening before scheduled/high-volume scraper-driven YPI publication.

## Inventory maintenance rules

Update this note when:

- a dependency is added or removed;
- a tool changes maturity or inventory status;
- a source changes priority;
- a repository or machine role changes;
- proxy or storage assumptions change;
- an implementation version is promoted, deprecated or replaced.

Every status change must identify evidence and whether Vuk approval is required.
