# Project Status and Next Actions

## Current known state

- YPI foundation Steps 1–3 are complete.
- YPI Step 4 should implement extraction, normalization, validation and publication boundaries.
- The scraper project has an initial architecture and source research notes.
- The dual-node concept is defined but must be operationally proven.
- The routine architecture is weekly hybrid-delta discovery with conditional detail fetching.
- Gemini and Antigravity acquisition work must remain protected.
- Codex should build source-neutral infrastructure and offline processing.

## Immediate next action

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

- real repository tree has not yet been audited against the intended architecture
- dual-node note was previously empty and needs implementation evidence
- actual proxy bytes per source are not yet measured
- Genesis completion accounting is not yet implemented
- source-neutral queue/checkpoint ownership needs to be confirmed
