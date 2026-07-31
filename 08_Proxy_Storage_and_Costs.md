# Proxy, Storage and Costs

This note stores project assumptions. Actual decisions must be based on telemetry from real pilots.

## Current proxy plan

- DataImpulse residential proxies
- existing working allowance: 5 GB for routine operations
- Genesis traffic purchased separately as required

## Genesis planning range for 100,000 listings (Capacity Target)

*Note: Target is ~96,000 raw source records before cross-source deduplication.*

| Scenario | Estimated traffic |
|---|---:|
| efficient | 30–40 GB |
| realistic target | 50–80 GB |
| browser-heavy | 120–200 GB |
| repeated reruns / poor optimization | 200 GB+ |
> Do not buy the maximum before measuring Boat24 and iNautia pilots.

## Storage planning
For 100,000 capacity target listings without downloading all images:
- initial primary dataset: approximately 70–120 GB
- one-year primary storage: approximately 140–270 GB
- with one local backup and safety margin: approximately 300–600 GB

## Routine planning range

| Architecture quality | Estimated monthly traffic |
|---|---:|
| excellent | 2–3 GB |
| expected hybrid delta | 4–5 GB |
| moderate browser overhead | 7–10 GB |
| repeated detail rescans | 15–30+ GB |

## Proxy budget states

- `0–60%`: normal weekly discovery and detail queue
- `60–75%`: delay low-priority stale refreshes
- `75–85%`: new, changed, incomplete and high-priority details only
- `85–95%`: prioritize Croatian/Adriatic sources and production-critical jobs
- `95–100%`: emergency and critical verification only

## Required telemetry

Measure per source:

- bytes per list page
- listings per list page
- bytes per detail page
- detail-fetch percentage
- retry traffic
- failed traffic
- monthly projection
- cost per accepted record

## Storage planning

For 50,000 listings without downloading all images:

- initial primary dataset: approximately 35–60 GB
- one-year primary storage: approximately 70–135 GB
- with one local backup and safety margin: approximately 150–300 GB

Recommended layout:

### SSD

- active containers
- databases
- current checkpoints
- current snapshots and batches
- development environment

### HDD

- compressed raw archive
- completed Genesis batches
- old routine snapshots
- logs and local backups

## Image rule

Initially store:

- image URLs
- image count
- optional primary-image hash later

Do not download every full-resolution image unless a separate business requirement justifies the storage and bandwidth.
