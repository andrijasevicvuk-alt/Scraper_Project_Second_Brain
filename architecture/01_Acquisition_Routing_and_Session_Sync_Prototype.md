---
status: prototype_decision
blueprint_owner: Gemini
implementation_owner: Jules
layer: protected_acquisition
production_status: not_approved
last_updated: 2026-08-05
---

# Acquisition Routing and Session Sync Prototype

## Purpose

Preserve the useful Gemini Unified Engine ideas while keeping the acquisition boundary compatible with `scraper_project`.

This is a prototype blueprint, not a replacement for the existing approved protected acquisition implementation.

## Boundary

The protected acquisition layer may:

- discover source listings;
- fetch list and detail responses;
- establish and refresh source-specific sessions;
- store immutable raw responses through the approved storage boundary;
- emit `DiscoveryObservation`, `RawFetchArtifact` and `FetchTelemetry`;
- record safe source-health and byte telemetry.

It must not:

- parse source fields into `ParsedListingCandidate`;
- calculate source-readiness or valuation-quality scores;
- run regex field fallback;
- promote selectors or fingerprints;
- own the source-neutral queue, job retries or checkpoints;
- publish into YPI normalized tables.

## Kept concepts

### Source and page-type routing

Each source note selects an acquisition route for each page type. Routes may include:

- existing approved custom acquisition implementation;
- fast HTTP acquisition;
- browser-mediated acquisition;
- a Session Sync Bridge prototype;
- an experimental Scrapling fetcher.

This is configuration-driven and source-specific. No single route is assumed to work for every source.

### Browser as session bootstrap

Where permitted and required by a source, a controlled browser runtime may establish a usable session or render a page. The browser is not automatically the bulk-fetch engine.

### Session Sync Bridge

A browser-established identity bundle may be handed to a compatible fast HTTP client only when controlled experiments demonstrate that the session, proxy affinity, user-agent and client profile remain compatible.

The bridge never copies secrets into shared contracts or documentation.

### Fast HTTP path

A compatible HTTP client may perform bulk list/detail acquisition with bounded concurrency and byte telemetry. The exact client, impersonation profile and request rate remain prototype decisions until experiments support them.

### Network asset controls

Browser requests may block or avoid nonessential assets to reduce proxy use, but the rule is source-specific.

Images, media, fonts, stylesheets and generic `Other` requests must not be universally blocked without proving the page still renders and exposes required content. Each block list requires a bandwidth and correctness experiment.

## Prototype flow

```text
Codex DetailFetchJob or discovery instruction
        ↓
source-specific routing decision
        ↓
existing approved route / fast HTTP / browser route
        ↓
optional protected Session Broker and Session Sync Bridge
        ↓
raw response saved immutably
        ↓
DiscoveryObservation / RawFetchArtifact / FetchTelemetry
        ↓
Codex offline parser
```

## Ideas moved to other layers

The original Unified Engine note placed Selectolax extraction, Pydantic field scoring, regex fallback and a Yacht Quality Score inside acquisition.

These concepts are moved as follows:

- JSON-LD and embedded-state extraction → Codex offline parser;
- semantic locator extraction → Codex offline parser;
- controlled regex fallback → Codex offline parser;
- adaptive selector/fingerprint validation → Codex parser-resilience framework;
- completeness and confidence → separate Codex quality signals;
- final valuation quality → YPI.

The single acquisition-layer YQS is removed because it duplicates and compresses several distinct quality responsibilities.

## Experiment requirements

Before a route becomes `experiment_supported`, record:

- source and page type;
- exact software versions;
- fixture or target sample;
- success and classified failure rates;
- bytes sent and received;
- session lifetime and refresh behaviour if applicable;
- raw snapshot integrity;
- comparison with the existing approved implementation;
- rollback path.

Production adoption requires Vuk's approval and must not silently replace the existing protected implementation.
