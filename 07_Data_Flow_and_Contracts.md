# Data Flow and Contracts

The protected acquisition implementation and the source-neutral platform communicate through small, versioned contracts.

## DetailFetchJob

- job ID
- source name
- source listing key
- listing URL
- reason code
- priority
- attempt number
- scheduled time

## DiscoveryObservation

- source name
- source listing key
- listing URL
- observation time
- visible title
- visible price
- visible currency
- visible specifications
- visible status
- card fingerprint

## RawFetchArtifact

- source name
- source listing key
- listing URL
- fetch time
- fetch method label
- acquisition version
- snapshot path
- content hash
- response status
- artifact status

## FetchTelemetry

- job ID
- source name
- bytes sent
- bytes received
- duration
- attempt number
- outcome
- error class
- proxy-pool label

## ParsedListingCandidate

The offline parser returns:

- source-level identity
- raw extracted fields
- field-level evidence
- extraction method per field
- confidence per field
- parser version
- parser warnings

The parser must not decide:

- final canonical builder/model/variant
- final cross-source duplicate merge
- valuation eligibility
- scoring weight

## Data ownership boundary

### Scraper project owns

- discovery
- raw snapshots
- source-level parsing
- acquisition telemetry
- source-readiness signals
- batch manifests

### YPI owns

- canonical mapping
- normalized boats and engines
- cross-source duplicate clusters
- final data quality and eligibility
- valuation-ready publication
- scoring and valuation UI

## Source of truth

- raw snapshot = truth about what the source returned at a moment
- scraper database = truth about crawl state
- YPI database = truth about normalized and valuation-ready business records
- Obsidian = truth about intended architecture, decisions and operating procedures
