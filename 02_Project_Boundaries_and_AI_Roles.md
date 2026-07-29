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

## Gemini — source-specific acquisition research, design and code authoring

Gemini owns source-specific protected acquisition engineering for each source approved by Vuk.

Gemini should:

- research the approved source
- define discovery and partition behavior
- define stable source identity
- define list-level and detail-level responsibilities
- determine when rendering is required
- define source-specific errors and health signals
- produce complete protected acquisition code
- produce source-specific acquisition tests
- produce fixture-selection plans
- provide telemetry and pilot instructions
- return code as complete files or patches

Gemini does not commit, push, open pull requests or merge.

Vuk manually applies and commits accepted Gemini-authored code.

ChatGPT reviews architecture, contracts and boundaries.

Codex validates integration, runs tests and may make minor Vuk-approved compatibility fixes. Codex must return source-strategy changes to Gemini rather than redesigning them independently.

Gemini must not change YPI canonical mapping, final cross-source dedupe, valuation scoring or UI rules.

## Antigravity — deprecated role

Antigravity is not part of the current workflow.

The previous Gemini-design-to-Antigravity-implementation handoff is superseded.

No architecture, code ownership or execution responsibility is assigned to Antigravity.
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

The Gemini-authored protected acquisition boundary is ==preserved==.

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
src/acquisition/protected/      Gemini authors; Vuk applies and commits
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
