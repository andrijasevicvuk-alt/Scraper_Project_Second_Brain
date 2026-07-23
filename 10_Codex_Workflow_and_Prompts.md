# Codex Workflow and Prompts

Codex should receive one narrow task at a time.

## Before every Codex task

Codex must:

1. read `AGENTS.md` and the relevant docs
2. list the files it intends to change
3. confirm protected acquisition paths are not included
4. work in small commits
5. run tests
6. report conflicts instead of changing the architecture silently

## Safe Codex sequence

### Task 1 — repository and boundary audit

No edits. Map the repository, protected paths, current contracts, missing components and safe commit sequence.

### Task 2 — source-neutral contracts

Implement versioned types for:

- DetailFetchJob
- DiscoveryObservation
- RawFetchArtifact
- FetchTelemetry
- ParsedListingCandidate
- DatasetBatchManifest

### Task 3 — orchestration foundation

Implement:

- crawl runs and partitions
- job queue
- checkpoints
- bounded retries
- snapshot manifests
- proxy ledger
- parser runs
- batch manifests
- crash recovery

No live target requests.

### Task 4 — one offline source parser

Use saved fixtures only. Build strict parsing, evidence, confidence, warnings and regression tests.

### Task 5 — controlled pilot integration

Connect an existing protected adapter through the contracts without modifying its internals. Apply explicit pilot limits and budget stop conditions.

### Task 6 — weekly hybrid-delta routine

Implement discovery comparison, detail reason codes, stale windows, missing verification, proxy-budget degradation and versioned delta batches.

## Protected paths instruction

Every prompt must include:

> Do not modify, rename, move, replace or reformat Gemini/Antigravity-owned acquisition paths. Treat them as opaque implementations of the shared acquisition contracts.

## Secret handling

Codex must not:

- read or print `.env`
- receive real proxy credentials in a prompt
- commit runtime databases or snapshots
- write secrets into examples or logs
