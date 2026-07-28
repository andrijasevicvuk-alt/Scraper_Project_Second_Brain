# Project Status and Next Actions

## Current known state

- YPI foundation Steps 1–3 are complete.
- YPI Step 4 should implement extraction, normalization, validation and publication boundaries.
- The scraper project has an initial architecture and source research notes.
- The dual-node concept is defined but must be operationally proven.
- The routine architecture is weekly hybrid-delta discovery with conditional detail fetching.
- Gemini and Antigravity acquisition work must remain protected.
- Codex should build source-neutral infrastructure and offline processing.
- Step 1 source-neutral foundation is merged into `main`.
- Step 2 SQLite persistence and hardening are complete and merged into `main`.
- Step 2 includes packaged migrations, crawl and partition lifecycles, queues, leases, bounded retries, checkpoints, immutable snapshot manifests, parser-run state, proxy accounting, complete dataset manifests, backups and recovery.
- The installed package and Docker image use the same canonical packaged migrations.
- The complete synthetic test suite contains 29 passing tests and GitHub Actions is green.
## Immediate next action

Begin Step 3 from [[16_Implementation_Prompt_Sequence]]: implement the isolated Docker and dual-node fixture flow, then perform physical two-machine validation when the home PC is available.

### 1. Freeze repository ownership

Create or update the root `AGENTS.md` with:

- protected paths
- Codex-safe paths
- shared contracts
- secret rules
- source-of-truth rules

### 2. Audit the real repository

Create the private scraper_project repository with the approved AGENTS.md and boundary docs, then ask Codex to inspect the initial repository and implement the source-neutral contracts and scaffold only.

### 3. Implement source-neutral contracts

Do not begin a full Genesis crawl before the shared contracts and persistent job state exist.

### 4. Prove the dual-node fixture flow

Run a synthetic or saved-fixture job from the ThinkPad through the home PC and prove resume after restart.

### 5. Prepare Boat24 source specification

Use [[templates/Source Note Template]].

### 6. Run a controlled acquisition sample

Gemini + Antigravity provide fixtures and telemetry only within the protected area.

### 7. Build Boat24 offline parser

Codex uses saved fixtures and reports field coverage.

### 8. Run staged pilots

- 20–50 listings
- 100 listings
- optional 1,000 listings

### 9. Approve or reject Genesis readiness

Use measured proxy use, parser coverage, retry rate, snapshot integrity and quality distribution.

## Current blockers to record

- dual-node note was previously empty and needs implementation evidence
- actual proxy bytes per source are not yet measured
- Genesis completion accounting is not yet implemented
- Step 3 physical dual-node validation requires access to the home PC
- actual proxy bytes per source remain unmeasured until controlled live pilots
