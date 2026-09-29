# Project Status and Next Actions

> Canonical detailed status checkpoint for the scraper project.  
> Last cross-repository audit: 2026-09-28.  
> Use [[20_Session_Start]] for the short session-entry version.

## Scope warning — two independent step sequences

YPI and `scraper_project` have different historical numbered roadmaps.

This note uses **`scraper_project Step N`** for the scraper subsystem. YPI status is described separately and must not be used to renumber the scraper roadmap.

## Executive status

| Area | Status | Evidence |
|---|---|---|
| scraper_project Step 1 — source-neutral foundation | VERIFIED | current main code/contracts/config/CLI/tests |
| scraper_project Step 2 — persistent runtime/orchestration | VERIFIED | migrations, queue/repositories, persistence/recovery tests and later Step 3 execution |
| scraper_project Step 3A — isolated Docker synthetic worker | VERIFIED | PR #5 merged to main; CI/repository synthetic flow |
| scraper_project Step 3B — physical dual-node validation | VERIFIED at 2026-09-19/20 checkpoint | ThinkPad/Home-PC execution, persistence, reboot, idempotency and abandoned-lease recovery |
| separate remote-reliability gate | VERIFIED at 2026-09-20 checkpoint | off-LAN private access, key-only SSH, reboot recovery, sleep prevention, backup/restore |
| Home PC online right now | UNKNOWN | Vuk is at college and no current live check was performed in this audit |
| scraper_project Step 4 — Boat24 source specification | CURRENT | source note exists, but the source specification is not yet finalized/approved |
| Boat24 protected acquisition adapter in scraper_project | PLANNED | no protected Boat24 implementation exists on current main |
| Boat24 offline parser/fixtures in scraper_project | PLANNED | no Boat24 parser or fixture set exists on current main |
| Boat24 pilot / Genesis / routine scraper | PLANNED | no live source run is authorized |
| scraper→YPI automated production handoff | PLANNED/PARTIAL | contracts define the boundary; high-volume publication remains gated by YPI hardening |

## What scraper_project Steps 1–3 actually achieved

### Step 1 — source-neutral foundation

Implemented and verified:

- typed/versioned shared contracts;
- source-registry configuration model;
- CLI boundary;
- redacted logging foundation;
- Docker development scaffold;
- unit tests and CI;
- explicit separation between protected source acquisition and source-neutral platform code.

The repository intentionally performs no live target-source acquisition at this layer.

### Step 2 — persistent crawl state and recovery

Implemented and verified:

- SQLite with WAL mode;
- versioned migrations `0001` and `0002`;
- crawl-run and partition lifecycle state;
- discovery observations;
- one authoritative detail-job queue owner;
- idempotent job insertion;
- leases and bounded retries;
- abandoned-job recovery;
- checkpoints;
- immutable snapshot manifests;
- parser-run state;
- proxy-usage ledger;
- complete dataset-batch manifests;
- database backup and restore helpers.

This establishes the operational state machine needed before a real source adapter exists.

### Step 3 — isolated Docker and dual-node proof

Step 3A proved the repository/container flow using only synthetic data.

Step 3B physically proved the real ThinkPad/Home-PC workflow:

- remote ThinkPad control of the Home PC;
- native non-root Docker;
- synthetic worker execution;
- persistence through container recreation;
- persistence through a full physical reboot;
- same-run idempotency without duplicate queue work/snapshot/batch creation;
- recovery of an intentionally abandoned leased job;
- persistent runtime state in the named Docker volume;
- Ubuntu automatic default boot.

**Conclusion:** `scraper_project Step 3` is genuinely complete.

The repository README still describes Step 3 as next/pending. That is a **documentation defect**, not an implementation failure.

## Remote worker acceptance after Step 3

A separate operational gate was added because Vuk would be away from the Home PC.

At the 2026-09-20 acceptance checkpoint the following passed:

- private Tailscale transport from an external network;
- existing OpenSSH over that path;
- key-only SSH;
- password and root SSH login disabled;
- remote administration returning after reboot without local GUI login;
- host-wide suspend/hibernate prevention;
- `tailscaled`, `ssh`, `NetworkManager`, `docker` and `containerd` enabled and active after reboot;
- healthy free disk space;
- unchanged runtime-volume identity;
- SQLite quick/integrity checks;
- coherent backup with checksum manifest;
- off-machine ThinkPad copy;
- isolated restore through the project restore helper;
- migration state preserved after restore.

This proves the architecture and acceptance checkpoint. It does **not** mean the Home PC can be described as online today without a current check.

The realistic physical fallback remains a trusted local person powering on or, only when specifically instructed, power-cycling the machine.

## Current scraper architecture

```text
ThinkPad control node
  ├─ GitHub / branches / reviews
  ├─ Second Brain
  ├─ ChatGPT / Codex / planning
  └─ private remote administration
              ↓
       Home PC worker node
          ├─ Docker Compose
          ├─ SQLite runtime queue/state
          ├─ checkpoints
          ├─ raw snapshots
          ├─ parser outputs / exports
          └─ future protected live acquisition
              ↓
      versioned source-level batch
              ↓
          YPI raw ingestion
```

Git synchronizes code and documentation. Runtime SQLite, snapshots, cookies/session state, logs and secrets are **not** synchronized through Git.

There is deliberately no second distributed queue and no automatic bidirectional runtime-state synchronization between the two computers.

## YPI cross-repository checkpoint

The YPI repository has progressed beyond its own historical Step 3.

### VERIFIED

- YPI manual-entry raw-ingestion path;
- narrow YPI Step 4 bootstrap `raw_listings → extraction → normalization → validation → boats + listings` path for controlled manual/bootstrap records;
- valid/invalid handling and raw-lineage/idempotency smoke coverage documented for that narrow path.

### IMPLEMENTED / PARTIAL

- read-only `valuation_ready_comparables` SQL view;
- data-access comparable query using same-builder + same-model as the first safe retrieval tier;
- optional variant/year filters and deterministic ordering;
- scoring package contract boundary.

The scoring package intentionally returns no real score, rank, confidence or valuation range. Cross-builder/spec-similar retrieval, geography weighting, recency weighting and final valuation logic remain future work.

### Known YPI hardening gate

YPI publication currently uses multiple Supabase REST writes and is not one atomic transaction.

That does not block the current Boat24 source-specification/prototype work, but **before scheduled scraper-driven ingestion, large batches or production/non-local publication**, YPI Step 4D transactional publication and partial-publication recovery must be implemented and verified.

### Historical Boat24 collector

The YPI repository contains an older bounded Boat24 draft collector. Its recorded run returned HTTP 403 for the attempted search routes and produced zero rows.

Treat it as historical evidence about the naive direct-request approach, **not** as the active protected `scraper_project` Boat24 implementation.

## Current Step — scraper_project Step 4

### Goal

Create and approve a complete Boat24 source specification before any protected implementation.

The specification must define the business/data contract that Gemini and Jules may implement without inventing YPI requirements.

### Phase 4A — reconcile Boat24 with YPI data needs

Can be done from college.

Define:

- YPI business role;
- in-scope and excluded boat categories;
- geographic scope;
- required source-level fields;
- optional fields;
- fields that may legitimately remain unknown;
- which fields are list-level versus detail-level;
- source identity requirements;
- source-level output required by the YPI raw-ingestion boundary.

### Phase 4B — source-evidence plan

Can be prepared from college; live claims remain hypotheses until evidence exists.

Define the evidence needed for:

- stable listing identity;
- discovery entry points;
- pagination and possible result ceilings;
- category/price/year partition needs;
- list-card field availability;
- detail-page field availability;
- access/defense behaviour.

Do not promote the old 403 result into a universal claim about Boat24. It proves only the tested direct-request route failed under its recorded conditions.

### Phase 4C — prototype acceptance contract

Can be done from college.

Specify:

- allowed protected inputs/outputs;
- snapshot requirements;
- telemetry requirements;
- fixture requirements;
- classified failure outcomes;
- internal-attempt limits;
- proxy-byte measurements;
- disk/runtime stop conditions;
- first pilot limits;
- maturity/promotion criteria.

### Phase 4D — Gemini/Jules handoff preparation

Can be done from college.

Once Vuk approves the source specification:

1. Gemini researches the current source and prepares the source-specific blueprint/experiment plan.
2. Vuk approves the prototype direction.
3. Jules implements only the approved protected paths/tests.
4. Vuk applies/commits accepted protected work.
5. ChatGPT/Codex review contract and source-neutral integration without rewriting protected internals.

### Phase 4E — live evidence and fixture collection

Requires a usable worker environment and explicit approval.

This is where Home-PC access becomes materially useful for browser/proxy/session tests, source telemetry and live fixture capture.

Do not require this phase merely to finish the written Step 4 source specification.

## What can be done while Vuk is at college

Safe and useful now:

- finish/approve the Boat24 source specification;
- review GitHub code, tests, contracts and documentation;
- prepare Gemini/Jules/Codex prompts;
- update source registry documentation/configuration without enabling live acquisition;
- design Boat24 fixture schema and parser test cases;
- implement source-neutral/offline work that uses synthetic or approved saved fixtures;
- review the YPI Step 4D transactional publication design;
- keep the Second Brain and repository boundaries aligned.

Do not make routine progress depend on the Home PC being online.

## What requires the Home PC or equivalent live worker access

Later:

- real Boat24 acquisition;
- browser/session/proxy experiments;
- bandwidth and worker-resource measurement;
- live fixture capture;
- staged acquisition pilots;
- Genesis execution;
- realistic routine scheduling;
- verifying the Home PC's current runtime state after time away;
- recovering/pushing any branch that exists only in the Home-PC local clone.

## Important local-only follow-up

At the 2026-09-20 checkpoint, Docker JSON-log limits were validated and committed on a Home-PC local branch:

`chore/bounded-docker-logging` at commit `85329f6`.

GitHub currently has no branch by that name. Therefore this change is **not present on remote main** and must not be treated as active until the local branch is recovered, reviewed, pushed and merged by Vuk.

This is a small operational follow-up before long unattended live workloads, not a reason to block the current source-specification work.

## Do not work on yet

Do not jump ahead to:

- full Genesis;
- weekly production scheduling;
- every experimental browser/acquisition tool at once;
- Session Broker production promotion;
- adaptive parser production auto-promotion;
- cross-source dedupe/scoring inside the scraper;
- final YPI valuation formula;
- large automated scraper→YPI publication before YPI atomic-publication hardening;
- enterprise orchestration such as Kubernetes.

## Immediate next action

1. Vuk reviews/merges the current Second Brain audit PR when satisfied.
2. Start `scraper_project Step 4` by turning [[sources/01_Boat24]] into the approved source specification.
3. After source-spec approval, hand the specification to Gemini for current source research and the protected prototype blueprint.

The Home PC is not required for items 1–2.
