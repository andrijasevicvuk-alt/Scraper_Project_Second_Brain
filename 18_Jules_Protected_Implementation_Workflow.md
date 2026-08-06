---
status: canonical
owner: Vuk / Jules
last_updated: 2026-08-05
---

# Jules Protected Implementation Workflow

## Role

Jules is the dedicated protected source-adapter implementation agent.

Gemini researches and specifies the source-specific acquisition blueprint. Jules translates the approved blueprint into complete protected Python code, configuration, tests and run instructions.

Jules inherits the implementation boundary previously assigned to Antigravity. Antigravity remains deprecated.

## Allowed paths

Jules may propose changes only inside the approved protected boundary unless Vuk explicitly expands the task:

```text
src/acquisition/protected/**
src/acquisition/custom_adapter/**
src/acquisition/protected_adapters/**
docker/protected/**
config/protected/**
tests/protected_acquisition/**
```

Safe usage documentation may be included when the task explicitly allows it.

## Required inputs

Jules receives:

- the approved Gemini blueprint;
- the source specification;
- shared contracts;
- existing protected implementation files that must be preserved;
- exact allowed paths;
- safe environment-variable names;
- fixture and test requirements;
- pilot limits;
- acceptance criteria.

## Responsibilities

Jules implements:

- discovery and detail acquisition behaviour defined by Gemini;
- source-specific browser or HTTP routing;
- protected session state and Session Broker logic when approved;
- source-specific classified errors;
- acquisition telemetry;
- acquisition-version identifiers;
- protected tests and fixture collection commands;
- safe configuration templates without secrets;
- repeatable worker commands.

Jules must return complete files or unified patches and must state which tests were actually executed.

## Experimental component rule

Jules implements an experimental component only when:

- Gemini included it in the approved source blueprint;
- the exact protected paths are defined;
- the experiment or prototype scope is bounded;
- existing protected work to preserve is listed;
- Vuk approved the implementation scope.

Jules must not silently remove or replace an existing tactic while implementing another component.

## Prohibited work

Jules must not:

- commit, push, open a pull request or merge;
- alter source-neutral contracts without approval;
- modify Codex's canonical runtime database or migrations;
- create a second crawl queue or job retry owner;
- implement offline parsers, selector promotion or source-readiness scoring inside acquisition;
- modify YPI normalization, dedupe, scoring or UI;
- silently replace an existing protected implementation;
- print or request secrets;
- claim unexecuted tests passed.

## Blueprint conflict rule

If the Gemini blueprint conflicts with repository contracts, existing protected code or measured evidence, Jules must stop and report:

- the exact conflict;
- affected files or interfaces;
- why the blueprint cannot be implemented safely;
- the minimum design question Gemini or Vuk must resolve.

Jules must not silently redesign the source strategy.

## Review and repair workflow

ChatGPT and Codex may review Jules' code and identify defects, but they do not directly overwrite it.

A review finding must include:

1. exact affected file or component;
2. reproducible or logically supported problem;
3. violated contract, boundary or expected behaviour;
4. required outcome;
5. precise repair scope;
6. tests and acceptance criteria.

Vuk then gives the repair prompt to Jules. Jules returns the replacement file or patch. Review continues until the protected implementation satisfies its contracts.

Use [[templates/Jules Repair Prompt Template]].

## Delivery checklist

- [ ] only allowed protected paths are changed;
- [ ] existing implementation is preserved unless replacement is explicitly approved;
- [ ] contracts validate;
- [ ] acquisition ends at raw artifacts and telemetry;
- [ ] internal attempts are bounded;
- [ ] no job queue or offline parser ownership is duplicated;
- [ ] secrets are absent;
- [ ] acquisition version is updated when behaviour changes;
- [ ] complete files or patch are provided;
- [ ] executed and unexecuted tests are distinguished;
- [ ] known limitations and unresolved questions are listed.
