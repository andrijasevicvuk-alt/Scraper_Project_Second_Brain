# Project Boundaries and AI Roles

The project uses ==Strict Separation of Concerns== so one AI does not destroy or replace another AI's work.

## Vuk — product owner and merge gate

I decide:

- which source is active
- which architecture is official
- which agent owns a folder
- when a pilot is accepted
- when a Genesis run can start
- whether an optional alternative is adopted
- what gets merged into `main`

No agent is allowed to silently replace a selected component.

## ChatGPT — architecture, logic and integration

ChatGPT can help with:

- architecture and system boundaries
- shared contracts
- source registry design
- queues, checkpoints and state machines
- Genesis and routine logic
- proxy budgeting and storage planning
- parser architecture
- normalization rules
- quality and dedupe design
- review queues
- YPI integration contracts
- Codex prompts and code review
- Obsidian structure and documentation

ChatGPT should review the protected acquisition zone from its outputs and contracts, not rewrite its internal implementation.

## Gemini — source-specific acquisition design

Gemini owns the design of the protected acquisition approach for each source.

Gemini should define:

- how discovery works
- what identifies a listing
- what list-level data is available
- when rendering is required
- what the acquisition adapter must return
- source-specific errors and health signals

Gemini should not change YPI canonical mapping, final cross-source dedupe, valuation scoring or UI rules.

## Antigravity — protected acquisition implementation and execution

Antigravity works inside the protected acquisition area.

It can:

- implement Gemini's acquisition plan
- run controlled live tests on the worker PC
- produce snapshots and telemetry
- provide repeatable worker commands
- classify acquisition failures

It must not edit Codex-owned parser, normalization, database, quality, publication or YPI modules.

## Codex — source-neutral platform engineer

Codex can build:

- typed contracts
- source registry
- SQLite migrations
- crawl runs and job queues
- checkpoints and bounded retries
- raw snapshot manifests
- proxy usage ledger
- offline parsers from fixtures
- validation, quality and dedupe candidate logic
- review queues
- batch exports
- Docker scaffolding
- monitoring and tests
- documentation

## Protected zone rule

The Gemini + Antigravity integration is ==preserved==.

Codex and ChatGPT must not silently:

- delete it
- rename it
- replace it with another framework
- move its responsibilities into the parser
- make it publish directly into YPI business tables

Optional alternatives may be documented, but they remain optional until I approve them.

## Folder ownership example

```text
src/contracts/                  shared, approval required
src/acquisition/protected/      Gemini + Antigravity
src/orchestration/              Codex
src/database/                   Codex
src/storage/                    Codex
src/parsers/                    Codex
src/normalizers/                Codex / YPI boundary
src/validators/                 Codex
src/dedupe/                     Codex candidate logic
src/quality/                    Codex
src/publication/                Codex
```
