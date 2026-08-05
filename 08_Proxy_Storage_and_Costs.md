# Proxy, Storage and Costs

This note stores planning assumptions. Actual purchase and production decisions must be based on controlled pilot telemetry.

## Current proxy plan

- DataImpulse residential proxies;
- existing working allowance: 5 GB for controlled pilots and routine-operation testing;
- Genesis traffic purchased separately only after source pilots measure real usage.

## Capacity target

The current planning capacity is approximately 100,000 raw source records before cross-source deduplication.

This is a capacity target, not a verified active-record count. Per-source volume estimates remain hypotheses until a complete discovery experiment records the evidence.

## Genesis traffic planning hypothesis

| Scenario | Estimated traffic |
|---|---:|
| efficient | 30–40 GB |
| realistic planning range | 50–80 GB |
| browser-heavy | 120–200 GB |
| repeated reruns / poor optimization | 200 GB+ |

Do not buy the maximum before measuring representative pilots, including at least one browser-heavy source.

## Routine planning hypothesis

| Architecture quality | Estimated monthly traffic |
|---|---:|
| excellent | 2–3 GB |
| expected hybrid delta | 4–5 GB |
| moderate browser overhead | 7–10 GB |
| repeated detail rescans | 15–30+ GB |

## Proxy budget states

- `0–60%`: normal weekly discovery and detail queue;
- `60–75%`: delay low-priority stale refreshes;
- `75–85%`: new, changed, incomplete and high-priority details only;
- `85–95%`: prioritize Croatian/Adriatic sources and production-critical jobs;
- `95–100%`: emergency and critical verification only.

## Required telemetry

Measure per source and acquisition version:

- bytes per list page;
- listings per list page;
- bytes per detail page;
- detail-fetch percentage;
- browser-bootstrap traffic;
- session-refresh traffic;
- retry traffic;
- failed traffic;
- cost per accepted record;
- monthly and Genesis projection.

## Storage planning hypothesis

For the 100,000-record capacity target without downloading all full-resolution images:

- initial primary dataset: approximately 70–120 GB;
- one-year primary storage: approximately 140–270 GB;
- one local backup plus safety margin: approximately 300–600 GB.

These ranges must be revised after snapshot-size and retention experiments.

### SSD

- active containers;
- runtime databases;
- current checkpoints;
- current snapshots and batches;
- protected session state;
- development environment.

### HDD

- compressed raw archive;
- completed Genesis batches;
- old routine snapshots;
- logs and local backups.

## Image rule

Initially store:

- image URLs;
- image count;
- optional primary-image hash later.

Do not download every full-resolution image unless a separate business requirement justifies the storage and bandwidth.
