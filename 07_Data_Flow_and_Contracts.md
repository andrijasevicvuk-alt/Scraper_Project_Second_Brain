# Data Flow and Contracts

The protected acquisition implementation and the source-neutral platform communicate through small, versioned contracts.

## Canonical flow

```text
source-neutral queue
→ Jules-authored protected acquisition adapter
→ DiscoveryObservation / RawFetchArtifact / FetchTelemetry
→ immutable snapshot manifest
→ Codex offline parser
→ ParsedListingCandidate
→ source-level validation and readiness
→ DatasetBatchManifest
→ YPI raw ingestion
```

## DetailFetchJob

A source-neutral request for an approved detail acquisition. The authoritative queue and job retry lifecycle are Codex-owned.

## DiscoveryObservation

A lightweight source-level observation from discovery or list pages.

## RawFetchArtifact

A protected acquisition output referencing the immutable source response. Acquisition ends at the raw artifact and telemetry boundary; it does not perform offline field extraction or source-readiness scoring.

## FetchTelemetry

Operational measurements for one acquisition attempt. It may contain safe labels and classified outcomes, but never credentials, cookies, tokens or browser profiles.

## ParsedListingCandidate

The Codex-owned offline parser returns:

- source-level identity;
- raw extracted fields;
- field-level evidence;
- extraction method per field;
- confidence per field;
- parser version;
- selector or fingerprint-set version when adaptive extraction is used;
- parser warnings and failure reasons.

The parser performs no network requests and must not decide:

- final canonical builder/model/variant;
- final cross-source duplicate merge;
- valuation eligibility;
- valuation scoring.

## Protected session state

Session cookies, user-agent values, browser state and source-specific session leases are internal protected runtime state.

They must not appear in shared contracts, Git, Obsidian, logs, manifests or prompts.

A protected Session Broker may coordinate session refresh inside one acquisition call, but it does not own the job queue, job retry transitions or checkpoints. After bounded protected attempts, it returns a classified result to the source-neutral orchestrator.

## Data ownership boundary

### Scraper project owns

- discovery;
- protected acquisition;
- raw snapshots;
- source-level offline parsing;
- acquisition telemetry;
- source-readiness signals;
- batch manifests.

### YPI owns

- canonical mapping;
- normalized boats and engines;
- cross-source duplicate clusters;
- final business data quality and valuation eligibility;
- valuation-ready publication;
- scoring and valuation UI.

## Source of truth

- raw snapshot = truth about what the source returned at a moment;
- scraper database = truth about source-neutral crawl and processing state;
- protected runtime state = temporary session material, never business data;
- YPI database = truth about normalized and valuation-ready business records;
- Obsidian = intended architecture, decisions and operating procedures.
