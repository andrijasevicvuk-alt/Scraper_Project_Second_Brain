# Gemini Acquisition Code Workflow

## Objective

For one approved source at a time, Gemini researches, designs and produces the source-specific protected acquisition code required by `scraper_project`.

Gemini does not commit directly to the repository.

Vuk controls code application, commits, pilot approval and merges.

## Inputs

Gemini receives:

- approved source specification
- discovery entry point
- required source-level fields
- shared contracts
- source registry information
- worker and proxy environment-variable names
- explicit pilot limits
- current repository tree
- relevant fixtures and verified source evidence

## Gemini responsibilities

Gemini owns:

- source research
- discovery design
- stable source identity
- list-page acquisition
- detail-page acquisition
- rendering requirements
- source-specific errors
- source health signals
- protected adapter code
- source-specific acquisition tests
- fixture-selection plans
- telemetry requirements
- pilot runbooks

## Required code delivery

Gemini returns:

- intended protected file tree
- complete file contents or unified patches
- acquisition-version identifier
- configuration templates without secrets
- source-specific tests
- repeatable local commands
- expected contract outputs
- known limitations
- unresolved questions

Gemini does not:

- commit
- push
- open a pull request
- merge
- request repository write access
- claim unexecuted tests passed

## Required adapter outputs

The adapter must produce or consume the approved contracts:

- `DiscoveryObservation`
- `DetailFetchJob`
- `RawFetchArtifact`
- `FetchTelemetry`

It must also provide:

- repeatable worker command
- classified failure output
- saved fixture plan
- source health report
- proxy-byte telemetry requirements
- acquisition version

## Review workflow

```
Gemini produces code proposal
→ Vuk reviews and applies it to a feature branch
→ ChatGPT reviews architecture and boundaries
→ Codex runs tests and repairs approved integration issues
→ Vuk commits and merges
```

## Codex integration boundary

Codex may fix:

- imports
- package paths
- typing
- shared-contract adapters
- configuration loading
- packaging
- integration tests
- CLI integration
- source-neutral orchestration compatibility

Codex must not independently change:

- source identity
- discovery strategy
- acquisition method
- rendering strategy
- proxy behavior
- source-specific retry meaning
- source-specific error classification

Those changes return to Gemini.

## Protected paths

Gemini-authored code belongs in:

```
src/acquisition/protected/**
src/acquisition/custom_adapter/**
src/acquisition/protected_adapters/**
docker/protected/**
config/protected/**
tests/protected_acquisition/**
```

## Prohibited changes

Gemini must not modify:

- source-neutral contracts without approval
- Codex orchestration database
- offline parsers
- YPI canonical mapping
- final cross-source dedupe
- valuation eligibility
- scoring
- web application logic

## Pilot progression

```
10–20 representative fixtures
→ 20–50 listing pilot
→ 100 listing pilot
→ optional 1,000 listing pilot
→ Genesis approval
```

Vuk must explicitly approve every live pilot increase and the Genesis run.

## Acceptance checklist

- stable source identity where available
- outputs validate against shared contracts
- completed snapshot records include path and content hash
- proxy-byte telemetry is defined
- attempts remain bounded
- no direct publication into YPI normalized tables
- offline parser fixtures are usable
- only protected source-specific code is proposed
- ChatGPT architecture review completed
- Codex integration tests completed
- Vuk approved the commit