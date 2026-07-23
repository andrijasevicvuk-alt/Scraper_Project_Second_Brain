# Routine Scrape

The Routine Scrape is the long-term maintenance architecture.

The current starting plan is ==one complete light discovery sweep per source per week==, with conditional detail fetching.

## Main routine flow

```text
Source registry
    ↓
Weekly list-page discovery crawl
    ↓
Extract listing key + URL + price + title + visible specs + status
    ↓
Compare against local database
    ↓
Apply detail-fetch decision rules
    ↓
Fetch detail only when required
    ↓
Save raw snapshot
    ↓
Parse → validate → quality signals
    ↓
Publish a versioned delta batch
```

## Detail-fetch decision rules

Fetch the detail page if:

1. the listing is new
2. no detail snapshot exists
3. price changed and list-level data is not sufficient
4. title or visible-spec fingerprint changed
5. the listing is high priority for target models or regions
6. critical stored fields are missing
7. the last detail refresh is older than its refresh window
8. the parser version changed and a new fixture or live verification is required
9. the listing needs missing-status verification
10. a manual reprocess was requested

## Otherwise update only

- `last_seen_at`
- visible price
- visible title and specs
- visible listing status
- card fingerprint
- last discovery run
- missing counter reset
- price history if the price changed

## Identity rule

Primary identity:

```text
source_name + source_listing_key
```

Title and price are change signals, not identity.

## Fingerprints

Keep separate:

1. identity key
2. list-card fingerprint
3. meaningful detail-content hash

Do not trigger a business update because an advertisement script or cosmetic page element changed.

## Missing-listing flow

```text
seen in complete sweep
→ active and missing count reset

missing from one complete sweep
→ missing_once

missing from two complete sweeps
→ direct verification job

confirmed unavailable
→ inactive_confirmed

still absent after repeated complete sweeps without confirmation
→ stale_or_removed_unconfirmed
```

Never delete price history or source trace.

## Starting weekly schedule

| Day | Source |
|---|---|
| Monday | Boat24 |
| Tuesday | TheYachtMarket |
| Wednesday | iNautia |
| Thursday | Njuškalo relevant categories |
| Friday | Croatian Yachting + MarineOne |
| Saturday | detail backlog, stale refreshes, missing verification |
| Sunday | backup, metrics, failed-job review, no normal crawl |

The schedule is a starting configuration and should later be adjusted from measured proxy use and source turnover.
