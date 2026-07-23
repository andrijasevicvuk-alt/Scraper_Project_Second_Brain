# Genesis Scrape

The Genesis Scrape builds the first source foundation.

It is a deep collection phase, but it still needs checkpoints, budgets and terminal states.

## Source-by-source process

```text
source specification
→ controlled acquisition sample
→ save fixtures
→ build offline parser
→ 20–50 listing pilot
→ 100 listing pilot
→ optional 1,000 listing pilot
→ measure cost and quality
→ approve Genesis
→ complete all discovery partitions
→ create dataset batch
→ validate YPI import
```

## Required terminal states

Every discovered listing must end as:

- `detail_success`
- `list_only_accepted`
- `excluded_with_reason`
- `failed_classified`
- `manual_review`

No discovered listing should disappear from accounting.

## Genesis completion gate

A source is complete only when:

- all discovery partitions completed
- no unaccounted queue jobs remain
- retries are exhausted or resolved
- all successful detail records have immutable snapshots
- source listing identity is stored
- parser version is recorded
- field coverage is measured
- proxy use is measured
- a manual QA sample is reviewed
- the batch is versioned
- known limitations are documented
- the YPI raw-ingestion validation passes
- a second discovery test proves delta detection

## Batch manifest

Every batch must include:

- source
- batch ID and version
- snapshot date
- discovered count
- detail-success count
- list-only count
- excluded count
- failed count
- review count
- parser version
- acquisition version
- quality distribution
- total proxy bytes
- known limitations
- checksum or manifest hash

## Stop conditions

A Genesis run must stop safely when:

- the proxy budget limit is reached
- disk free space falls below the safety threshold
- parser failure rate exceeds the configured threshold
- repeated access failures occur
- the worker is shutting down
- a human pauses the run

A stopped run must be resumable from its last checkpoint.
