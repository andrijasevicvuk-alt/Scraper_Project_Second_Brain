# Decision Log

This note records architecture decisions so they are not silently changed later.

## D-001 — YPI and scraper separation

**Decision:** The scraper publishes raw/source-level batches. YPI owns canonical normalization, final cross-source dedupe, valuation eligibility and scoring.

**Status:** canonical

## D-002 — Protected acquisition zone

**Decision:** Gemini and Antigravity own the protected acquisition implementation. Codex and ChatGPT do not replace it.

**Status:** superseded by D-009 and D-010

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

## D-007 — Second Brain timing

**Decision:** Use the Second Brain now as a lightweight control and knowledge system. Delay complex AI/RAG integration until the scraper foundation and YPI core are stable.

**Status:** canonical

## D-008 — SQLite runtime persistence foundation

**Date:** 2026-07-28

**Decision:** Use SQLite with WAL mode, one source-neutral writer boundary, persistent leases, bounded retries, checkpoints and immutable successful snapshot manifests.

**Status:** canonical

## D-009 — Gemini-authored acquisition with Vuk-controlled commits

**Date:** 2026-07-29

**Decision:** Gemini owns source-specific research, design and protected code authoring; Vuk controls application and commits.

**Reason:** This removed the Gemini-to-Antigravity handoff.

**Status:** superseded by D-010

## D-010 — Gemini blueprints and Jules protected implementation

**Date:** 2026-08-05

**Decision:** Gemini owns source research, source-specific acquisition blueprints, experiment plans and Second Brain maintenance. Jules owns protected acquisition implementation and protected tests inside the existing protected paths. Vuk applies, commits and merges accepted Jules work. ChatGPT and Codex may inspect and test protected code but must return implementation defects as precise Jules repair prompts rather than editing or replacing the protected implementation directly.

**Reason:** Preserve a strict design-to-implementation boundary while keeping Vuk as the merge gate and preventing reviewers from silently overwriting source-specific work.

**Alternatives considered:** Gemini both designs and implements; Antigravity implementation; direct Codex compatibility edits inside protected code.

**Effect on architecture:** Protected paths and shared contracts remain unchanged. Existing approved protected implementations are preserved. Antigravity remains deprecated. Codex remains the source-neutral platform and offline parser owner.

**Status:** canonical

## D-011 — Prototype and evidence maturity

**Date:** 2026-08-05

**Decision:** Architecture and source behaviour use four maturity levels: `hypothesis`, `prototype_decision`, `experiment_supported`, and `production_approved`.

A prototype decision may be fully specified and implemented for testing, but it is not treated as proven. Experiment support requires reproducible evidence. Production approval requires defined acceptance criteria and Vuk's decision.

**Effect on architecture:** Session Sync, Session Broker, Scrapling and other unproven components remain visible without silently replacing approved production components.

**Status:** canonical

## D-012 — Acquisition, session coordination and parser boundary

**Date:** 2026-08-05

**Decision:** Protected acquisition ends at `DiscoveryObservation`, `RawFetchArtifact` and `FetchTelemetry`. Protected session coordination may manage source-specific session state and bounded refresh attempts, but the Codex-owned source-neutral orchestrator remains the sole owner of job retries, requeue and checkpoints. Offline extraction, regex fallback, adaptive selector/fingerprint promotion and source-readiness logic remain in the Codex-owned parser and quality layers.

**Reason:** Preserve immutable raw evidence, reproducibility and one authoritative queue owner.

**Status:** canonical

## New decision template

### D-XXX — Title

**Date:**

**Decision:**

**Reason:**

**Alternatives considered:**

**Effect on architecture:**

**Status:** draft / canonical / superseded
