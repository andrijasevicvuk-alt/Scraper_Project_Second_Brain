# Project Boundaries and AI Roles

The project uses ==Strict Separation of Concerns== so one agent does not silently destroy, replace or absorb another agent's responsibility.

## Vuk — product owner and merge gate

Vuk decides:

- which source is active;
- which architecture or prototype decision is official;
- which agent owns a folder or implementation boundary;
- which experiment criteria are accepted;
- when a pilot is accepted;
- when a Genesis run can start;
- whether an alternative is adopted;
- what gets applied, committed and merged into `main`.

No agent may silently replace a selected component or promote an experiment into production.

## ChatGPT — lead architecture and review

ChatGPT owns:

- architecture and system boundaries;
- shared-contract review;
- queue, state-machine and lifecycle review;
- Genesis and routine logic;
- parser and quality architecture;
- YPI integration boundaries;
- consistency audits;
- Codex and Jules prompts;
- Second Brain structure and documentation review.

ChatGPT may inspect Jules-authored code, tests and outputs. ChatGPT must not directly rewrite or replace Jules' protected implementation.

When ChatGPT identifies a protected-code problem, it must return:

1. what is wrong;
2. why it matters;
3. the exact file, class, function or component involved;
4. the expected behaviour;
5. a precise implementation prompt for Jules;
6. tests or evidence required to accept the repair.

## Gemini — source research, acquisition blueprints and Second Brain

Gemini owns source-specific research and design for each source approved by Vuk.

Gemini should:

- research source structure and behaviour;
- define discovery and partition strategy;
- define stable source identity;
- separate list-level and detail-level responsibilities;
- define source-specific acquisition modes;
- define rendering and session requirements;
- define source-specific errors and health signals;
- define telemetry, fixture and pilot requirements;
- maintain source notes, experiment plans and architecture blueprints;
- hand Jules an implementation-ready protected file map and acceptance plan.

Gemini does not implement, commit, push, open pull requests or merge protected production code in the current workflow.

Gemini must not change source-neutral orchestration, offline parser implementation, YPI canonical mapping, final cross-source dedupe, valuation scoring or UI rules.

## Jules — protected source-specific implementation

Jules inherits the protected implementation boundary previously assigned to Antigravity.

Jules owns:

- protected acquisition adapter code;
- protected source-specific acquisition tests;
- protected worker configuration;
- source-specific session and browser runtime code;
- source-specific acquisition commands and runbooks;
- repairs to Jules-authored protected code after review findings.

Jules implements the approved Gemini blueprint and shared contracts. Jules must not silently redesign the source strategy. If the blueprint is impossible, contradictory or unsafe, Jules reports the conflict to Vuk and Gemini before changing the design.

Jules does not commit, push, open pull requests or merge. Vuk applies and commits accepted Jules files or patches.

## Codex — source-neutral platform engineer

Codex owns:

- typed contracts;
- source registry;
- SQLite migrations and runtime persistence;
- crawl runs, partitions and the authoritative job queue;
- checkpoints and bounded job retries;
- snapshot manifests and storage;
- proxy usage ledger;
- offline parsers from saved fixtures;
- parser evidence, validation and source-readiness logic;
- review queues and dataset batches;
- Docker and dual-node source-neutral infrastructure;
- monitoring, exports and integration tests.

Codex may inspect and test Jules-authored protected code through its contracts. Codex must not directly edit, replace, reformat or recreate Jules' protected implementation.

When Codex finds a protected-code problem, it follows the same reviewer-to-Jules repair workflow used by ChatGPT. Codex may fix source-neutral integration code in Codex-owned paths, but protected repairs return to Jules.

## Antigravity — deprecated role

Antigravity is not part of the current workflow.

Its former implementation boundary is now assigned to Jules. No active architecture or execution responsibility remains assigned to Antigravity.

## Protected zone rule

The protected acquisition boundary is preserved. Existing protected acquisition implementations must not be replaced merely because a new prototype or framework is proposed.

Protected paths:

```text
src/acquisition/protected/**
src/acquisition/custom_adapter/**
src/acquisition/protected_adapters/**
docker/protected/**
config/protected/**
tests/protected_acquisition/**
```

Jules authors new protected implementation changes. Vuk applies and commits them.

ChatGPT and Codex may review, run tests and identify defects, but must not directly:

- delete or rename protected code;
- replace it with another framework;
- reformat it as part of unrelated work;
- move acquisition responsibilities into the parser;
- copy its implementation into a competing path;
- make it publish directly into YPI business tables.

## Correction workflow for protected code

```text
Reviewer finds defect
→ reviewer records evidence and affected component
→ reviewer writes a precise Jules repair prompt
→ Vuk approves the repair scope
→ Jules returns complete replacement files or a patch
→ Vuk applies the patch on a feature branch
→ ChatGPT and Codex verify architecture, contracts and tests
→ unresolved protected defects return to Jules
→ Vuk commits and merges
```

Use [[templates/Jules Repair Prompt Template]] for every protected-code correction.

## Folder ownership

```text
src/contracts/                  shared; Vuk approval required
src/acquisition/protected/      Jules implementation; Gemini blueprint; Vuk commits
src/acquisition/custom_adapter/ Jules implementation; Gemini blueprint; Vuk commits
src/acquisition/protected_adapters/ Jules implementation; Gemini blueprint; Vuk commits
docker/protected/               Jules implementation; Vuk commits
config/protected/               Jules implementation; no secrets
tests/protected_acquisition/    Jules implementation tests
src/orchestration/              Codex
src/database/                   Codex
src/storage/                    Codex
src/parsers/                    Codex
src/validators/                 Codex
src/quality/                    Codex
src/review/                     Codex
src/publication/                Codex / YPI handoff boundary
```

Changes to shared contracts or ownership require Vuk's explicit approval and a Decision Log entry.
