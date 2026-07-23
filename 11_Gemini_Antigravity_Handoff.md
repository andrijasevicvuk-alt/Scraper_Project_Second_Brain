# Gemini and Antigravity Handoff

This note protects the Gemini and Antigravity part of the architecture.

## Their objective

For one source at a time, produce a controlled acquisition adapter that satisfies the shared contracts.

## Inputs

- source specification
- discovery entry point
- required source-level fields
- DetailFetchJob contract
- proxy and worker environment configuration
- explicit pilot limits

## Required outputs

- DiscoveryObservation
- RawFetchArtifact
- FetchTelemetry
- repeatable worker command
- acquisition version
- classified failure output
- saved list and detail fixtures
- short source health report

## What they may edit

- protected acquisition adapters
- protected worker configuration
- source-specific acquisition tests
- safe documentation of adapter usage

## What they must not edit

- source-neutral contracts without approval
- Codex orchestration database
- offline parsers
- YPI canonical mapping
- cross-source dedupe decisions
- final valuation eligibility
- scoring or web application logic

## Pilot progression

```text
10–20 representative samples
→ 20–50 listing pilot
→ 100 listing pilot
→ optional 1,000 listing pilot
→ Genesis approval
```

## Acceptance checklist

- stable source identity where available
- complete snapshot path and content hash
- telemetry includes byte usage
- bounded attempts
- no direct publication into YPI normalized tables
- output passes shared contract validation
- parser fixtures are usable offline
- protected paths are the only modified code area
