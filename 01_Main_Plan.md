# Main Plan

`scraper_project` is a side project for [[YPI]], the boat market comparison and valuation project.

The scraper project has one main responsibility:

> Build and maintain a trustworthy collection of source-level boat listing data, then hand it to YPI without mixing acquisition logic with valuation logic.

## The complete system

```text
Target websites
    ↓
Source-specific acquisition adapters
    ↓
Raw snapshots + telemetry
    ↓
Offline parsing
    ↓
Source-level validation
    ↓
Dataset batch export
    ↓
YPI raw ingestion
    ↓
Normalization + cross-source dedupe
    ↓
Valuation-ready dataset
    ↓
Scoring + comparison UI
```

## The two scraping phases

### Phase 1 — Genesis Scrape

The Genesis Scrape creates the first complete source snapshot.

For every in-scope listing, the result must end in one known state:

- detail successfully collected
- list-level record accepted
- excluded with a reason
- failed with a classified reason
- waiting for manual review

A Genesis Scrape is not complete just because the script stopped.

### Phase 2 — Routine Scrape

The Routine Scrape keeps the Genesis dataset current while protecting the proxy budget.

The normal process is:

```text
Weekly list-page discovery
    ↓
Compare listing key, price, title, visible specs and status
    ↓
Fetch detail page only when required
    ↓
Update last_seen, price history and listing state
```

See [[06_Routine_Scrape]].

## Vertical spike strategy

I will finish one source at a time:

```text
source specification
→ controlled acquisition sample
→ saved fixtures
→ offline parser
→ small pilot
→ larger pilot
→ Genesis Scrape
→ quality review
→ YPI batch
→ weekly routine
```

The first completed source should prove the full platform. Later sources should reuse the same contracts and operational core.

## Recommended source order

1. Boat24
2. Croatian Yachting
3. MarineOne / YachtBrokerage
4. Njuškalo Nautika
5. TheYachtMarket
6. iNautia

This order is not a rule that deletes or replaces a target. It is an execution order based on YPI value and engineering risk.

## Definition of the final scraper project

The scraper project is complete when every selected source has:

- a registry entry
- a controlled acquisition adapter
- discovery and detail parsing
- saved fixtures and regression tests
- a versioned Genesis batch
- proxy-use and quality reports
- weekly routine scheduling
- stale and removed-listing handling
- monitoring and recovery documentation
- a controlled YPI handoff
