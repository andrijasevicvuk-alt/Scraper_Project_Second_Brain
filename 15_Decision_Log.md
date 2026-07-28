# Decision Log

This note records architecture decisions so they are not silently changed later.

## D-001 — YPI and scraper separation

**Decision:** The scraper publishes raw/source-level batches. YPI owns canonical normalization, final cross-source dedupe, valuation eligibility and scoring.

**Status:** canonical

## D-002 — Protected acquisition zone

**Decision:** Gemini and Antigravity own the protected acquisition implementation. Codex and ChatGPT do not replace it.

**Status:** canonical

## D-003 — Source-neutral core

**Decision:** Queues, checkpoints, snapshot manifests, offline parsers, telemetry and exports use shared contracts independent of the acquisition implementation.

**Status:** canonical

## D-004 — Source-by-source delivery

**Decision:** Complete a full vertical slice for one source before launching all sources.

**Status:** canonical

## D-005 — Routine frequency

**Decision:** Begin with one complete light discovery sweep per source per week and conditional detail fetching.

**Status:** current starting policy

## D-006 — Proxy allowance

**Decision:** Test whether 5 GB is sufficient for routine scraping before purchasing a larger routine allowance. Purchase Genesis traffic based on pilot measurements.

**Status:** current budget policy

## D-007 — Second brain timing

**Decision:** Build the Obsidian second brain now as a lightweight control and knowledge system. Delay AI/RAG integration until the scraper foundation and YPI core are stable.

**Status:** canonical

## D-008 — SQLite runtime persistence foundation

**Date:** 2026-07-28

**Decision:** Use SQLite with WAL mode, one source-neutral writer boundary, persistent leases, bounded retries, checkpoints and immutable successful snapshot manifests.

**Reason:** The scraper must recover safely from crashes and preserve crawl state on the local worker PC.

**Effect on architecture:** Step 2 is complete. SQLite runtime persistence uses packaged migrations, WAL mode, one authoritative queue owner, durable lifecycle transitions, bounded retries, checkpoints, immutable successful snapshots, proxy accounting and versioned dataset manifests.

**Evidence:** Commits `4c0debe`, `12f900a` and `cddb3e4`; 29 passing tests; GitHub Actions green.

**Status:** canonical
## New decision template

### D-XXX — Title

**Date:**

**Decision:**

**Reason:**

**Alternatives considered:**

**Effect on architecture:**

**Status:** draft / canonical / superseded
