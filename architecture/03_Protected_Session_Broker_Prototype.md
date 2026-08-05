---
status: prototype_decision
blueprint_owner: Gemini
implementation_owner: Jules
layer: protected_acquisition_runtime
production_status: not_approved
last_updated: 2026-08-05
---

# Protected Session Broker Prototype

## Confirmation

The Session Broker idea is useful. The original problem was placement and ownership, not the core coordination pattern.

A protected Session Broker can prevent multiple workers from refreshing the same source session simultaneously. It cannot become a second job orchestrator.

## Broker responsibilities

The broker may own source-specific protected runtime state such as:

- identity bundle reference;
- cookie/session material;
- user-agent and compatible client-profile labels;
- proxy-affinity label where required;
- session generation number;
- refresh lease owner and expiry;
- session creation, validation and expiry timestamps;
- bounded refresh status and protected internal telemetry.

This state is secret runtime material. It is never committed, included in shared contracts or printed in logs.

## Responsibilities it does not own

The Session Broker does not own:

- crawl runs or partitions;
- `DetailFetchJob` state;
- job leasing;
- job requeue;
- job retry counts;
- checkpoints;
- terminal-state accounting;
- parser runs;
- dataset batches.

Those remain Codex source-neutral responsibilities.

## Correct refresh flow

```text
protected acquisition call detects stale or blocked session
        ↓
worker asks broker for current generation
        ↓
worker obtains refresh lease or waits with bounded jitter
        ↓
lease holder creates and validates a replacement identity bundle
        ↓
new protected session generation is stored
        ↓
waiting workers reload the new generation
        ↓
current acquisition call makes only its approved bounded internal attempt
        ↓
successful RawFetchArtifact / classified FetchTelemetry outcome
        ↓
Codex orchestrator alone decides whether the job is succeeded, retried or exhausted
```

The broker must never directly return a job to the queue.

## Persistence boundary

### Prototype default

Use a protected local runtime store under an ignored worker volume, separate from business and shared-contract data. A dedicated SQLite file is acceptable because it stores session coordination, not a second crawl queue.

### Optional canonical database integration

If Vuk later approves storing broker metadata in the canonical runtime database:

- ChatGPT reviews the interface;
- Codex owns the migration and repository implementation;
- secret values remain protected or referenced indirectly;
- queue and retry tables remain unchanged;
- Jules consumes the approved interface without owning the migration.

## Lock requirements

A refresh lease must include:

- source/session key;
- generation number;
- lease owner;
- acquired and expiry times;
- safe recovery after worker failure;
- atomic compare-and-set or transaction semantics;
- bounded waiting and jitter;
- no credential values in diagnostic output.

Exact timeout values remain experiment-configured rather than hard-coded architecture facts.

## Introduction sequence

- **Step 5:** Gemini defines the source-specific need, identity bundle, compatibility requirements, internal attempt limit and experiment plan.
- **Step 6:** Jules implements the protected broker prototype and tests inside protected paths.
- **Step 8:** Codex integrates only through existing contracts and the authoritative queue.
- **Step 9:** Vuk runs controlled session lifetime, refresh-herd and failure-recovery experiments.
- **Production:** Vuk approves a version only after recorded evidence meets criteria.

## Required experiments

- one refresh under concurrent worker pressure;
- lease expiry after the refresh worker crashes;
- session generation change detection;
- compatible browser-to-client session transfer;
- invalid-session rejection before promotion;
- bounded internal attempts;
- no duplicate job retries or queue mutations;
- secret redaction and runtime-state cleanup.
