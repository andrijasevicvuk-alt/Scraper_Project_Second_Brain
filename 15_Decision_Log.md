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

## D-013 — No silent removal or replacement of project ideas

**Date:** 2026-08-06

**Decision:** Existing acquisition ideas, experimental tools and protected tactics remain documented until controlled evidence and Vuk’s explicit decision reject, replace, deprecate or promote them.

ChatGPT and Codex may recommend optimizations, identify risks and prepare repair prompts, but they must not silently remove, downgrade or replace Gemini-designed acquisition ideas or Jules-authored protected implementation.

An agent’s inability to assist with a tactic is not technical evidence that the tactic is unnecessary or invalid.

**Effect on architecture:** Gemini retains source research and blueprint ownership. Jules retains protected implementation ownership. ChatGPT and Codex retain architecture, source-neutral platform, parser and review responsibilities.

**Status:** canonical

## D-014 — Second Brain transparency and selective updates

**Date:** 2026-09-19

**Decision:** No Second Brain modification may be hidden from Vuk. Updates are made only when they preserve durable information useful for project continuation, debugging, maintenance, handoff, major milestones, incidents, decisions, constraints or material project-state changes.

Every Second Brain modification must identify the affected file/section, additions, modifications, removals, reason and effect on roadmap, architecture, project state, assumptions, decisions or next steps. If nothing changed, that must be stated explicitly.

Previous decisions are not silently rewritten. Important superseded history is preserved and the new finding/current decision/reason are recorded.

Git history is the lightweight Second Brain change log; the Decision Log records important decisions. Significant documentation updates should be identifiable in Git history rather than hidden inside unrelated work.

**Reason:** Keep the Second Brain useful as a current project-management and handoff system without turning it into an activity log, while guaranteeing complete visibility into its evolution.

**Alternatives considered:** Update after every small action; maintain a separate detailed change-log document; rely on undocumented agent discretion.

**Effect on architecture:** No scraper technical architecture change. Project-governance and documentation workflow are tightened.

**Status:** canonical

## D-015 — Private remote administration and recovery baseline

**Date:** 2026-09-19; acceptance completed 2026-09-20

**Decision:** Use private Tailscale transport to the existing OpenSSH service on `ypi-worker` as the remote-administration baseline for the university-away period. Do not expose SSH, Supabase, PostgreSQL, Docker APIs or development ports directly to the public internet by default.

The accepted baseline also requires:

- key-only OpenSSH access;
- password and root SSH login disabled;
- host-wide suspend/hibernate prevention;
- networking, Tailscale and OpenSSH returning automatically after an ordinary reboot without local GUI login;
- a coherent off-machine restore-tested runtime backup;
- a simple physical recovery owner for power, boot or home-network failures that cannot be repaired in-band.

Tailscale SSH is not part of the baseline. Existing OpenSSH remains the shell boundary.

**Acceptance evidence:**

- genuine off-LAN access from the ThinkPad over a phone hotspot passed;
- Tailscale DERP relay fallback was observed and accepted when a direct path was unavailable;
- ThinkPad key-only SSH passed in BatchMode;
- password-only SSH was rejected;
- controlled reboot returned `tailscaled`, `ssh`, `NetworkManager`, `docker` and `containerd` automatically;
- host sleep prohibitions remained effective after reboot;
- an off-machine backup was copied to the ThinkPad;
- backup checksums passed;
- backup and isolated restored SQLite databases both passed integrity checks;
- restored migration state preserved `0001` and `0002`.

**Reason:** Vuk will not have physical access to the Home PC for approximately one month. Remote recoverability is therefore part of the operating architecture, not merely convenience.

**Alternatives considered:** public OpenSSH with forwarding/DDNS; direct WireGuard; WireGuard through a VPS; Tailscale plus existing OpenSSH; Tailscale SSH; a separate remote-KVM/power path.

**Residual boundary:** Tailscale is still in-band. A hard power loss, hardware fault, failed boot before networking or home-router/ISP outage may require a trusted local person. No independently tested out-of-band KVM/power system was added.

**Effect on architecture:** Adds a verified operational-access and recovery layer around the existing dual-node worker. It does not change source acquisition, queue, parser or YPI boundaries. With the remote-readiness gate accepted, Step 4 may proceed.

**Status:** canonical

## New decision template

### D-XXX — Title

**Date:**

**Decision:**

**Reason:**

**Alternatives considered:**

**Effect on architecture:**

**Status:** draft / canonical / superseded
