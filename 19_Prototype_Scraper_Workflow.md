---
status: canonical
owner: Vuk / ChatGPT
last_updated: 2026-08-05
---
# Prototype Scraper Workflow

## Decision

The Second Brain uses one general prototype workflow as the reusable template for all sources, plus a separate canonical source note for each website and separate Experiment Log records for its experimental history.

The general note is not one universal scraper engine. It defines what a complete prototype must prove while allowing every source to use a different acquisition method.

## Lifecycle

```text
generic prototype contract
        ↓
source specification
        ↓
Gemini source-specific prototype blueprint
        ↓
Jules protected source prototype
        ↓
saved fixtures and controlled experiments
        ↓
Codex offline parser and resilience tests
        ↓
optimized source adapter and parser versions
        ↓
staged pilots
        ↓
production-approved scraper version
```

## Maturity vocabulary

### Hypothesis

An untested source claim, component idea, route, performance estimate or compatibility assumption.

### Prototype decision

A design selected so implementation and testing can begin. It must be concrete enough for Gemini to specify, Jules or Codex to implement and experiments to evaluate. It is not proof and does not replace a production-approved component automatically.

### Experiment-supported

A recorded controlled experiment supports the claim within its tested conditions. Evidence includes versions, samples, commands, measurements and limitations.

### Production-approved

A versioned source adapter/parser configuration meets defined pilot criteria and is explicitly accepted by Vuk for routine use.

## Definition of a complete source prototype

A source prototype may include conditional experimental components when Gemini determines that the source requires them and Vuk approves the test.

The generic prototype does not activate every experimental component by default. It preserves them as available source-specific options until experiments establish their value.

A source-specific prototype should be theoretically complete for one website within a bounded test scope. It includes:

- source specification and business scope;
- discovery entry points and identity strategy;
- one or more bounded acquisition routes;
- immutable raw artifact output;
- telemetry and classified failures;
- protected code and tests from Jules;
- representative fixture plan;
- repeatable commands;
- Codex offline parser prototype;
- explicit hypotheses and experiment plan;
- no claim of production readiness.

## General versus source-specific documentation

### General prototype note

This note defines shared lifecycle, maturity, roles and promotion gates.

### Source note

Each source note records:

- source facts and evidence;
- business role and scope;
- source-specific prototype decisions;
- Gemini blueprint;
- Jules implementation version and paths;
- Codex parser version;
- linked experiments;
- pilot evidence;
- current maturity and production status.

Use [[templates/Source Note Template]].

### Experiment notes

Each experiment is an independent reproducible record. The source note links all related experiment IDs instead of embedding an ever-growing experiment history.

Use [[templates/Experiment Log Template]].

## Optimization rule

Experiments improve a working prototype by measuring and changing one controlled concern at a time:

- reliability;
- efficiency and proxy bytes;
- speed and resource use;
- session durability;
- parser accuracy;
- maintainability;
- recovery behaviour;
- compatibility with shared contracts.

An optimization is not accepted merely because it is theoretically attractive. It must preserve correctness and compare against the current baseline.

## Promotion gates

### From hypothesis to prototype decision

- problem and expected value are clear;
- implementation boundary is identified;
- existing approved components are preserved;
- experiment and rollback plans exist;
- Vuk approves the prototype direction where architecture is affected.

### From prototype decision to experiment-supported

- implementation is versioned;
- experiment is reproducible;
- success and failure criteria are met;
- limitations are recorded;
- results are compared with the baseline.

### From experiment-supported to production-approved

- representative staged pilots pass;
- data integrity and terminal-state accounting pass;
- proxy, storage and reliability budgets are acceptable;
- parser and selector/fingerprint versions are fixed;
- recovery and rollback work;
- Vuk explicitly approves the version.

## Relationship to implementation steps

- Step 3: source-neutral Docker and dual-node proof only.
- Step 4: source specification.
- Step 5: Gemini prototype blueprint.
- Step 6: Jules protected prototype implementation.
- Step 7: Codex offline parser and resilience implementation.
- Step 8: source-neutral integration.
- Step 9: controlled experiments and staged pilots.
- Steps 10–12: production/Genesis readiness and execution.
