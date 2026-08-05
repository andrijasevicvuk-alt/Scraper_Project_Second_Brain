# Gemini Acquisition Blueprint Workflow

## Objective

For one approved source at a time, Gemini researches the source, selects an implementable prototype design, maintains the source-specific Second Brain knowledge and prepares the protected acquisition blueprint that Jules will implement.

Gemini does not commit or implement protected production code in the current workflow.

Vuk approves the source scope, prototype decision, pilot limits and merge.

## Inputs

Gemini receives:

- approved source specification;
- discovery entry points and available evidence;
- required source-level fields;
- shared contracts;
- source registry information;
- protected environment-variable names;
- explicit pilot limits;
- current repository tree;
- relevant fixtures, probes and experiment records;
- existing protected acquisition implementation constraints.

## Gemini responsibilities

Gemini owns:

- source research;
- discovery and partition design;
- stable source identity design;
- list/detail acquisition responsibilities;
- acquisition-mode selection;
- rendering and session requirements;
- source-specific errors and health signals;
- fixture-selection plan;
- telemetry and experiment requirements;
- pilot runbook design;
- source-note and architecture-note maintenance;
- implementation-ready Jules handoff.

Gemini must preserve existing approved protected implementations unless Vuk explicitly approves replacement.

## Required blueprint delivery

Gemini returns:

- maturity status for every major design choice;
- intended protected file tree;
- component responsibilities and interfaces;
- contract inputs and outputs;
- configuration schema without secrets;
- session and browser runtime requirements;
- bounded internal-attempt rules;
- source-specific error classification;
- source-specific test and fixture plan;
- repeatable commands expected from the implementation;
- known limitations and unresolved questions;
- exact Second Brain updates;
- a complete Jules implementation prompt.

Gemini does not:

- commit, push, open a pull request or merge;
- directly implement protected production code;
- claim tests or experiments passed without evidence;
- modify source-neutral contracts or the Codex runtime database;
- define offline parser implementation as part of acquisition;
- silently promote hypotheses or prototype decisions to production.

## Jules handoff

Gemini's implementation prompt must specify:

- exact allowed protected paths;
- complete expected behaviour;
- existing code that must be preserved;
- shared contracts to satisfy;
- safe configuration placeholders;
- tests Jules must add or update;
- fixture requirements;
- telemetry requirements;
- acceptance criteria;
- prohibited changes.

See [[18_Jules_Protected_Implementation_Workflow]].

## Acquisition boundary

The protected adapter may produce or consume:

- `DetailFetchJob`;
- `DiscoveryObservation`;
- `RawFetchArtifact`;
- `FetchTelemetry`.

It ends at raw artifacts and telemetry.

The blueprint must not put these inside protected acquisition:

- offline field extraction;
- regex fallback parsing;
- selector/fingerprint promotion;
- source-readiness scoring;
- YPI normalization or valuation logic.

## Prototype and experiment rule

Use the maturity model in [[19_Prototype_Scraper_Workflow]].

A `prototype_decision` may be detailed enough for Jules to implement, but it remains unproven until the Experiment Ledger records controlled results. Production approval requires defined criteria and Vuk's decision.

## Review workflow

```text
Gemini researches and writes blueprint
→ Vuk approves prototype direction
→ Jules implements protected code and tests
→ Vuk applies it to a feature branch
→ ChatGPT reviews architecture and boundaries
→ Codex runs source-neutral integration tests
→ protected defects return as precise Jules repair prompts
→ Jules returns repairs
→ Vuk commits and merges
```

## Protected paths

Jules implements Gemini-approved source-specific code inside:

```text
src/acquisition/protected/**
src/acquisition/custom_adapter/**
src/acquisition/protected_adapters/**
docker/protected/**
config/protected/**
tests/protected_acquisition/**
```

## Second Brain responsibility

Every substantial Gemini task must also identify exact Second Brain changes for:

- source evidence;
- prototype decisions;
- experiments;
- implementation status;
- acquisition versions;
- known limitations;
- inventory status;
- next approved action.
