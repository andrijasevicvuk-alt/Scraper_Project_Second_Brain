# YPI Integration Boundary

This note exists so scraper-project sessions understand the real state of the separate YPI product repository without confusing YPI's roadmap with `scraper_project`.

Audit checkpoint: **2026-09-28**.

## Product purpose

YPI is the boat market comparison and valuation application.

Its intended product flow is:

```text
target boat input
    ↓
retrieve defensible comparable listings
    ↓
score/explain comparable relevance
    ↓
estimate a price range with confidence/explanation
    ↓
search/comparison valuation UI
```

The scraper project is only the market-data acquisition/source-readiness subsystem.

## Boundary

```text
scraper_project
  source discovery
  → protected acquisition
  → immutable raw snapshots
  → offline parsing
  → source-level validation
  → versioned dataset batch
            ↓
        YPI raw ingestion
            ↓
      normalized business data
            ↓
      valuation-ready comparables
            ↓
       scoring / valuation
            ↓
        product UI
```

### scraper_project owns

- source discovery;
- protected acquisition;
- immutable raw source evidence;
- crawl state/checkpoints/retries;
- source-level parser evidence;
- acquisition telemetry;
- source-readiness signals;
- versioned source-level batch export.

### YPI owns

- raw-ingestion business records;
- canonical builder/model/variant mapping;
- unit/currency/business normalization;
- normalized boats/listings;
- cross-source duplicate handling;
- valuation eligibility;
- valuation-ready publication;
- comparable retrieval;
- scoring/confidence/valuation range;
- product UI.

The scraper must never write directly into YPI normalized or valuation-ready business tables.

## Current YPI implementation checkpoint

Repository evidence at the audit checkpoint shows:

### VERIFIED

- manual-entry bootstrap records enter the raw-ingestion layer;
- YPI's narrow Step 4 bootstrap pipeline processes controlled manual/bootstrap `raw_listings`;
- valid records can produce normalized `boats` and `listings`;
- invalid records follow the documented failed/error path;
- raw→normalized lineage/idempotency safeguards exist for the verified narrow path.

### IMPLEMENTED / PARTIAL

- `public.valuation_ready_comparables` read-only SQL view;
- comparable data-access query;
- first safe retrieval tier: same builder + same model;
- optional exact-variant/year filters and metadata;
- deterministic retrieval ordering;
- scoring input/output contracts.

The scoring package intentionally returns:

- no score;
- no rank;
- no confidence;
- no price estimate;
- no valuation range.

These are contract placeholders, not an implemented valuation model.

### PLANNED / not yet complete

- robust canonical/spec normalization beyond the narrow bootstrap slice;
- transactional YPI publication for scraper-driven volume;
- cross-builder/spec-similar comparable retrieval;
- broad-market fallback retrieval;
- Croatia → Slovenia → Adriatic → Mediterranean weighting;
- recency/confidence model;
- robust dedupe/quality/review flows;
- final scoring formula;
- valuation-range calculation;
- main search/comparison product UI;
- production security/deployment for non-local use.

## Critical publication gate

Current YPI publication uses multiple Supabase REST writes and is not one atomic transaction.

The current safeguards reduce duplicate risk, but they do not guarantee rollback of every partial write.

Therefore:

> **Before scheduled scraper-driven ingestion, large batch ingestion or production/non-local publication, complete and verify the YPI Step 4D transactional publication/recovery hardening.**

This gate does not block the current `scraper_project Step 4` source-specification work or later fixture/offline-parser development.

## Historical Boat24 collector

YPI contains a bounded draft Boat24 collector created before the dedicated scraper platform matured.

Recorded result:

- direct HTTP search attempts returned HTTP 403;
- zero Boat24 rows were collected;
- output remained draft/manual-review material;
- no production scraping or normalized-table insertion resulted.

This is useful historical evidence about that tested route only.

It is **not**:

- a current Boat24 adapter in `scraper_project`;
- proof that every possible Boat24 acquisition route fails;
- an approved production acquisition implementation.

Future Boat24 work follows [[sources/01_Boat24]] and the `scraper_project` source lifecycle.

## Source-of-truth rule

For YPI implementation facts, inspect the YPI repository code, migrations and tests first.

This note is a cross-repository checkpoint for scraper sessions, not a replacement for YPI's own canonical documentation.
