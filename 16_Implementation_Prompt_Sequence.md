# Scraper Project Implementation Playbook

## Step 0 — Create the repository boundary

### Goal

Create the private repository and place the rules inside it before implementation begins.

### Tool

Manual work by Vuk.

### Prompt

No AI prompt.

### Actions

1. Create a private GitHub repository named:
    
    `scraper_project`
    
2. Clone it to the ThinkPad.
    
3. Add these initial files:
    
    - `AGENTS.md`
        
    - `README.md`
        
    - `.gitignore`
        
    - `docs/project-boundaries.md`
        
    - `docs/data-contracts.md`
        
4. Make the initial commit:
    
    `chore: establish scraper project boundaries`
    

### Expected result

An almost-empty but version-controlled repository in which Codex already knows what it may and may not edit.

### Check before continuing

- Repository is private.
    
- `AGENTS.md` is in the root.
    
- Protected Gemini/Antigravity paths are documented.
    
- No `.env`, proxy credentials, databases, snapshots, cookies or logs are committed.
    

---

## Step 1 — Build the source-neutral foundation

### Goal

Create the first real project structure and shared contracts without implementing any live source acquisition.

### Tool

Codex.

### Exact prompt

You are implementing the first source-neutral foundation of `scraper_project`.

Before changing anything:

1. Read the complete root `AGENTS.md`.
    
2. Read all existing files under `docs/`.
    
3. Show the current repository tree.
    
4. List every file you intend to create or modify.
    
5. Confirm explicitly that no Gemini-authored protected acquisition path will be modified.
    

Project purpose:

This repository collects public boat-listing source artifacts for the YPI valuation project. Its output stops at source-level raw artifacts, parsed candidates, telemetry and versioned dataset batches. YPI remains responsible for final canonical mapping, cross-source deduplication, valuation eligibility, scoring and the web application.

Implement only the source-neutral foundation:

- Python package structure;
    
- typed and versioned shared contracts;
    
- source-registry configuration model;
    
- command-line entry point;
    
- test structure;
    
- configuration-loading boundary;
    
- safe logging setup;
    
- basic Docker development scaffold;
    
- project documentation.
    

Implement these contracts:

- `DetailFetchJob`
    
- `DiscoveryObservation`
    
- `RawFetchArtifact`
    
- `FetchTelemetry`
    
- `ParsedListingCandidate`
    
- `DatasetBatchManifest`
    

Architectural rules:

- Do not implement live website acquisition.
    
- Do not implement CAPTCHA handling, anti-bot bypass, cookie harvesting, token interception, fingerprint manipulation or behavioral evasion.
    
- Do not modify, rename, move, replace or reformat Gemini-authored protected acquisition paths.
    
- Treat protected acquisition code as an opaque future implementation of the shared contracts.
    
- Parsers must not perform network requests.
    
- Acquisition must not write directly into YPI normalized or valuation-ready tables.
    
- Raw snapshots must be treated as immutable.
    
- Unknown data must remain unknown rather than being invented.
    
- Do not read or print `.env`.
    
- Do not add unnecessary dependencies.
    

Create a minimal CLI with placeholder-safe commands such as:

- `scraper source list`
    
- `scraper contract validate`
    
- `scraper health`
    

Do not yet implement queues, retries, live adapters, source parsers, Genesis scraping or routine scraping.

Add unit tests for contract validation and configuration loading.

At completion:

1. Run all tests.
    
2. Show the final tree.
    
3. Explain every created file.
    
4. Report any architecture ambiguity.
    
5. Propose the next small commit only; do not implement it.
    

Use small, logical commits.

### Expected result

A clean repository containing:

- shared contracts;
    
- source-registry model;
    
- CLI skeleton;
    
- tests;
    
- protected acquisition paths remain absent or untouched;
    
- initial Docker development files;
    
- architecture documentation.
    

### Check before continuing

- All tests pass.
    
- No live requests exist.
    
- Protected paths remain empty or untouched.
    
- Contracts match `docs/data-contracts.md`.
    
- Codex did not add canonical YPI mapping or valuation logic.
    
- The first commit is small and understandable.
    

---

## Step 2 — Add persistent crawl state

### Goal

Create the reliable database and job-state layer needed before any real scraping.

### Tool

Codex.

### Exact prompt

Read `AGENTS.md`, all relevant docs and the shared contracts already implemented.

Do not modify any Gemini-authored protected acquisition path.

Implement the source-neutral persistence and orchestration foundation using SQLite for local scraper runtime state.

Add versioned migrations and repositories for:

- source registry state;
    
- crawl runs;
    
- crawl partitions;
    
- discovery observations;
    
- detail-fetch jobs;
    
- job attempts;
    
- checkpoints;
    
- raw snapshot manifests;
    
- parser runs;
    
- proxy-usage ledger;
    
- dataset-batch manifests.
    

Required behavior:

- idempotent job creation;
    
- `source_name + source_listing_key` as the main source identity;
    
- explicit job reason codes;
    
- bounded retry counts;
    
- crash-safe state transitions;
    
- recovery of abandoned `processing` jobs;
    
- one authoritative queue owner;
    
- immutable completed snapshot records;
    
- append-only important observations;
    
- no silent loss of discovered listings;
    
- migration versioning;
    
- SQLite WAL mode;
    
- one-writer policy;
    
- database backup command;
    
- restore smoke test.
    

Use synthetic records only.

Add CLI commands for:

- creating a fixture crawl run;
    
- inspecting queue status;
    
- recovering abandoned jobs;
    
- showing proxy usage;
    
- backing up the runtime database.
    

Add tests for:

- duplicate job insertion;
    
- job leasing;
    
- retry exhaustion;
    
- crash recovery;
    
- checkpoint resume;
    
- backup and restore;
    
- database migration from an empty database.
    

Do not implement source acquisition, source parsers, Genesis execution or weekly scheduling.

Before coding, show the exact file plan. At completion, run tests and provide a state-transition report.

### Expected result

The project can safely remember every job, retry, checkpoint, snapshot record and proxy byte across restarts.

### Check before continuing

- Restart tests preserve work.
    
- Jobs cannot retry forever.
    
- Duplicate jobs are prevented.
    
- Backups restore correctly.
    
- Exactly one layer owns retries.
    
- Runtime databases are ignored by Git.
    
### Completion status

Step 2 is complete.

Core persistence commit:

`4c0debe — feat: add sqlite runtime persistence`

Hardening commits:

- `12f900a — feat: harden sqlite persistence lifecycle`
- `cddb3e4 — fix: package runtime migrations`

Final result:

- 29 passing synthetic tests
- GitHub Actions green
- installed-package migration test passed
- Docker migration smoke test passed
- protected Gemini/Antigravity paths untouched
- shared contracts unchanged

## Step 2A — Harden SQLite persistence

Status: complete and merged through PR #2.

---

## Step 3 — Prove the isolated Docker and dual-node flow

### Goal

Prove the platform works between the ThinkPad control node and home-PC worker without involving a live target.

### Tool

Codex creates the infrastructure. Vuk runs it manually.

### Exact prompt

Implement the isolated local-worker infrastructure for `scraper_project`.

Read and obey `AGENTS.md`.

Do not modify protected acquisition code and do not make live requests.

Create:

- a non-root worker Dockerfile;
    
- Docker Compose configuration;
    
- persistent volumes for runtime database, checkpoints, snapshots, logs and exports;
    
- a local synthetic fixture server;
    
- worker health command;
    
- graceful shutdown handling;
    
- resume-after-restart support;
    
- CPU and memory configuration defaults suitable for a 16 GB RAM worker;
    
- PowerShell helper scripts for starting, stopping, checking and resuming the worker;
    
- redacted structured logging;
    
- disk-space safety checks.
    

Restrictions:

- do not mount the Docker socket;
    
- do not bake secrets into the image;
    
- do not expose a public port by default;
    
- do not commit runtime data;
    
- do not create a second queue database;
    
- do not add Kubernetes;
    
- use Docker Compose.
    

Create an end-to-end fixture test:

`synthetic discovery → discovery observation → detail job → synthetic snapshot → manifest → parser placeholder → dataset batch`

Document how Vuk can:

1. start the worker on the home PC;
    
2. check it from the ThinkPad;
    
3. stop it safely;
    
4. restart it;
    
5. prove the queue resumes.
    

### Expected result

The project runs inside an isolated container, persists its state and recovers after interruption.

### Check before continuing

- Container runs as non-root.
    
- A rebuild does not remove snapshots.
    
- A restart does not lose queue state.
    
- No secrets appear in logs.
    
- Synthetic end-to-end test passes.
    
- Only Docker Compose is used.
    

---

## Step 4 — Write the Boat24 source specification

### Goal

Define exactly what Boat24 contributes before designing its acquisition implementation.

### Tool

ChatGPT, reviewed manually by Vuk.

### Exact prompt

Check the Scraper Project Second Brain repository first.

Using the current source note template, prepare the complete Boat24 source specification.

Include:

- business role in YPI;
    
- in-scope listing categories;
    
- excluded categories;
    
- geographic scope;
    
- required source-level fields;
    
- stable identity requirements;
    
- list-level fields;
    
- detail-level fields;
    
- expected discovery partitions;
    
- snapshot requirements;
    
- acquisition output contract;
    
- parser responsibilities;
    
- quality limitations;
    
- pilot limits;
    
- proxy metrics to record;
    
- Genesis completion requirements;
    
- routine refresh policy;
    
- evidence status for every claim: observed, hypothesis, verified or deprecated.
    

Do not design or replace the protected Gemini/Antigravity acquisition internals.

End with exact Obsidian changes.

### Expected result

One approved source specification that Gemini can use without inventing YPI requirements.

### Check before continuing

- Source scope is clear.
    
- Required fields are explicit.
    
- Identity rule is explicit.
    
- List-page and detail-page responsibilities are separate.
    
- Unverified assumptions are labelled.
    
- Vuk approves the note.
    

---

## Step 5 — Design Boat24 acquisition

### Goal

Create the source-specific acquisition design while respecting shared contracts.

### Tool

Gemini.

### Exact prompt

You are designing the protected Boat24 acquisition adapter for `scraper_project`.

Read:

- the Boat24 source specification;
    
- the shared contracts;
    
- `AGENTS.md`;
    
- the Gemini/Antigravity handoff documentation.
    

Do not modify code yet.

Design only:

- discovery entry points;
    
- pagination or partition strategy;
    
- stable source-listing identity;
    
- list-card observation fields;
    
- conditions requiring detail fetches;
    
- acquisition modes required by different page types;
    
- expected artifact format;
    
- telemetry requirements;
    
- checkpoint boundaries;
    
- source-specific error classes;
    
- safe pilot progression;
    
- expected proxy-cost measurements;
    
- fixture selection strategy.
    

The adapter must return:

- `DiscoveryObservation`
    
- `RawFetchArtifact`
    
- `FetchTelemetry`
    

It must not:

- modify Codex orchestration;
    
- modify offline parsers;
    
- modify YPI normalization;
    
- publish into YPI business tables;
    
- replace shared contracts;
    
- redesign the repository.
    

Separate:

- verified observations;
    
- hypotheses requiring testing;
    
- implementation decisions;
    
- unresolved questions.
    

End with Gemini’s own implementation plan, protected file list and test plan. Do not write code during this design step. Do not begin Genesis scraping.

### Expected result

An approved source-specific design that Gemini will use during its separate code-authoring step.

### Check before continuing

- The design uses existing contracts.
    
- It does not redefine the queue.
    
- It does not redefine the parser.
    
- Every uncertain claim is labelled.
    
- Pilot limits are explicit.
    
- Vuk approves the design.
    

---

## Step 6 — Implement the protected Boat24 adapter

### Goal

Produce the complete Boat24 protected acquisition implementation from your approved Gemini design.

Return complete files or unified patches. Do not commit, push, open a pull request or merge.

### Tool

Gemini produces the code. Vuk applies it manually. ChatGPT and Codex review it.

### Exact prompt

Implement the approved Boat24 acquisition design inside the protected Gemini/Antigravity acquisition zone.

Read and obey:

- root `AGENTS.md`;
    
- Boat24 source specification;
    
- Gemini acquisition design;
    
- shared contract definitions.
    

You may modify only:

- protected Boat24 acquisition adapter files;
    
- protected worker configuration;
    
- source-specific protected acquisition tests;
    
- safe adapter usage documentation.
    

Do not modify:

- shared contracts;
    
- Codex orchestration;
    
- database migrations;
    
- offline parsers;
    
- normalizers;
    
- validators;
    
- dedupe;
    
- quality scoring;
    
- publication;
    
- YPI code.
    

Required outputs:

- valid `DiscoveryObservation` records;
    
- valid `RawFetchArtifact` records;
    
- valid `FetchTelemetry` records;
    
- acquisition-version identifier;
    
- classified failure output;
    
- repeatable worker command;
    
- 10–20 representative saved list/detail fixtures;
    
- byte-usage telemetry;
    
- short source health report.
    

Use explicit pilot limits and bounded attempts.

Do not run a full Genesis scrape.

At completion, show:

1. all modified protected files;
    
2. the exact repeatable command;
    
3. contract-validation results;
    
4. fixture inventory;
    
5. proxy bytes consumed;
    
6. failure classifications;
    
7. unresolved source limitations.
    

### Expected result

A protected Boat24 adapter that returns source artifacts without changing the rest of the platform.

### Check before continuing

- Only protected files changed.
    
- Contracts validate.
    
- Snapshots have hashes and paths.
    
- Proxy bytes are recorded.
    
- Attempts are bounded.
    
- Fixtures open offline.
    
- No direct YPI publication exists.
    

---

## Step 7 — Build the offline Boat24 parser

### Goal

Turn saved Boat24 snapshots into source-level parsed candidates without live access.

### Tool

Codex.

### Exact prompt

Implement the Boat24 offline parser using only the approved fixtures under `tests/fixtures/boat24/`.

Do not make network requests.

Do not modify Gemini-authored protected acquisition paths.

Implement:

- list-card parser;
    
- detail-page parser;
    
- parser version;
    
- field-level evidence;
    
- extraction method per field;
    
- extraction confidence per field;
    
- parser warnings;
    
- explicit failure reason codes;
    
- strict selector path;
    
- lower-confidence fallback path;
    
- fixture-based tests;
    
- regression tests.
    

Preserve original source strings.

Return `None` for unknown fields.

Do not perform:

- final canonical builder mapping;
    
- final model or variant mapping;
    
- cross-source duplicate merging;
    
- valuation eligibility;
    
- valuation scoring;
    
- YPI publication.
    

Produce a coverage report for:

- source identity;
    
- title;
    
- builder signal;
    
- model signal;
    
- variant signal;
    
- year;
    
- asking price;
    
- currency;
    
- location;
    
- ownership signal;
    
- engine information;
    
- dimensions;
    
- description;
    
- image URLs or image count if available.
    

At completion, list every fixture and every missing or low-confidence field.

### Expected result

A deterministic and tested offline Boat24 parser.

### Check before continuing

- No network imports or requests.
    
- All fixtures are tested.
    
- Parser failures are explicit.
    
- Raw strings are preserved.
    
- Missing values are not invented.
    
- Field coverage is acceptable for the pilot.
    

---

## Step 8 — Connect the controlled pilot

### Goal

Connect the protected adapter to the source-neutral queue and offline parser without changing either responsibility.

### Tool

Codex.

### Exact prompt

Connect the existing protected Boat24 acquisition adapter to the source-neutral orchestration platform through the existing contracts.

Do not modify the protected adapter internals.

Implement the controlled pilot flow:

`discovery → observation validation → detail decision → detail job → protected acquisition call → raw artifact validation → snapshot manifest → offline parser → source-readiness result → pilot report`

Add:

- explicit maximum discovery records;
    
- explicit maximum detail jobs;
    
- maximum proxy-byte budget;
    
- maximum failure-rate stop condition;
    
- maximum parser-failure stop condition;
    
- disk-space stop condition;
    
- graceful pause and resume;
    
- snapshot-integrity validation;
    
- pilot progress reporting;
    
- parser coverage summary;
    
- proxy-cost summary;
    
- retry summary;
    
- terminal-state accounting.
    

Create commands for:

- 20–50 listing pilot;
    
- 100 listing pilot;
    
- optional 1,000 listing pilot.
    

Do not run the pilots yourself.

Do not implement full Genesis crawling.

At completion, provide a Genesis-readiness checklist that remains unapproved until Vuk reviews actual pilot evidence.

### Expected result

The system is ready for controlled live Boat24 pilots.

### Check before continuing

- Protected adapter internals remain unchanged.
    
- Limits stop the pilot correctly.
    
- Every discovered record receives a terminal state.
    
- Resume works.
    
- Reports show bytes, retries and parser coverage.
    
- Vuk approves the pilot configuration.
    

---

## Step 9 — Execute staged Boat24 pilots

### Goal

Measure real acquisition cost, reliability and data quality.

### Tool

Vuk executes the approved Gemini-authored adapter manually. Codex may assist with source-neutral runtime operation. ChatGPT reviews the evidence.

### Exact prompt

Run the approved Boat24 pilot through the existing source-neutral orchestration.

Do not edit Codex-owned modules.

Run only the currently approved pilot size.

Return:

- discovered count;
    
- detail-job count;
    
- successful artifacts;
    
- classified failures;
    
- retry count;
    
- proxy bytes;
    
- average bytes per list page;
    
- average bytes per detail page;
    
- average duration;
    
- snapshot-integrity result;
    
- parser coverage;
    
- source-readiness distribution;
    
- terminal-state accounting;
    
- checkpoint and resume evidence.
    

Stop when any configured budget or safety threshold is reached.

Do not continue to a larger pilot without explicit approval from Vuk.

### Expected result

Measured evidence from 20–50, then 100, and optionally 1,000 listings.

### Check before continuing

- Costs are measured, not guessed.
    
- Failure types are understood.
    
- Parser coverage is adequate.
    
- No records disappear from accounting.
    
- Resume has been demonstrated.
    
- Projected Genesis proxy cost is acceptable.
    

---

## Step 10 — Review Genesis readiness

### Goal

Decide whether Boat24 is ready for full collection.

### Tool

ChatGPT reviews. Vuk decides manually.

### Exact prompt

Check the Second Brain repository first.

Review the Boat24 pilot report, terminal-state report, parser coverage, proxy usage, retries, snapshot integrity and known limitations.

Return one decision:

- `READY_FOR_GENESIS`
    
- `READY_WITH_CONDITIONS`
    
- `NOT_READY`
    

For every failed requirement, state:

- evidence;
    
- severity;
    
- owner;
    
- exact fix;
    
- whether it blocks Genesis.
    

Do not change or replace the Gemini-authored acquisition implementation.

End with exact Obsidian changes.

### Expected result

A documented readiness decision.

### Check before continuing

Vuk explicitly approves `READY_FOR_GENESIS` or accepts all stated conditions.

---

## Step 11 — Finalize Genesis controls

### Goal

Make the complete source run measurable, stoppable and resumable.

### Tool

Codex.

### Exact prompt

Implement the final source-neutral Genesis controls for Boat24.

Do not modify protected acquisition code.

Requirements:

- partitioned discovery;
    
- persistent checkpoints;
    
- resumable detail queue;
    
- configured proxy-budget stop;
    
- configured disk-space stop;
    
- configured parser-failure stop;
    
- configured acquisition-failure stop;
    
- graceful human pause;
    
- terminal-state accounting;
    
- immutable snapshot manifests;
    
- acquisition and parser versions;
    
- dataset-batch manifest;
    
- batch checksum;
    
- manual-review export;
    
- YPI raw-ingestion validation export;
    
- second-discovery delta validation.
    

Every discovered listing must end as exactly one of:

- `detail_success`
    
- `list_only_accepted`
    
- `excluded_with_reason`
    
- `failed_classified`
    
- `manual_review`
    

Do not run Genesis.

At completion, run synthetic Genesis tests and show the complete approval checklist.

### Expected result

A Genesis controller that cannot silently lose jobs or overrun the approved budget.

### Check before continuing

- Stop conditions work.
    
- Resume works.
    
- Every record has a terminal state.
    
- Batch manifest is complete.
    
- YPI export validates syntactically.
    
- Vuk gives final execution approval.
    

---

## Step 12 — Execute Boat24 Genesis

### Goal

Build the first complete Boat24 source foundation.

### Tool

Vuk executes the approved Gemini-authored protected adapter through the approved source-neutral controller.

### Exact prompt

Execute the approved Boat24 Genesis run through the existing source-neutral orchestration.

Do not change architecture or source scope during execution.

Use:

- approved discovery partitions;
    
- approved proxy budget;
    
- approved concurrency;
    
- approved retry limits;
    
- approved stop conditions;
    
- approved snapshot storage;
    
- approved parser version.
    

Produce:

- final terminal-state reconciliation;
    
- complete raw snapshot manifest;
    
- parser-run report;
    
- proxy-usage report;
    
- failure report;
    
- manual-review export;
    
- versioned dataset-batch manifest;
    
- checksum;
    
- YPI raw-ingestion validation result;
    
- second-discovery delta-test result.
    

Pause safely rather than changing architecture when a blocker appears.

### Expected result

A complete, versioned Boat24 Genesis batch.

### Check before continuing

- All partitions completed or are explicitly classified.
    
- No unaccounted jobs remain.
    
- Batch checksum is valid.
    
- Manual QA sample passes.
    
- YPI raw-ingestion validation passes.
    
- Delta test works.
    

---

## Step 13 — Implement weekly routine scraping

### Goal

Maintain Boat24 efficiently after Genesis.

### Tool

Codex.

### Exact prompt

Implement the weekly hybrid-delta routine engine using the approved source-neutral architecture.

Do not modify protected acquisition code.

The routine must:

1. perform a complete light discovery sweep;
    
2. identify records by `source_name + source_listing_key`;
    
3. compare visible price, title, status and visible specifications;
    
4. update `last_seen_at` for unchanged records;
    
5. append price history when price changes;
    
6. create detail jobs only for approved reason codes;
    
7. support priority scores;
    
8. support source-specific stale-refresh windows;
    
9. count complete-sweep absences;
    
10. verify missing listings before marking them unavailable;
    
11. preserve all price history and source trace;
    
12. enforce monthly proxy-budget degradation rules;
    
13. publish a versioned delta batch;
    
14. produce weekly source-health reports.
    

Use these detail reason codes:

- `NEW_LISTING`
    
- `NO_DETAIL_SNAPSHOT`
    
- `PRICE_CHANGE_REQUIRES_DETAIL`
    
- `CARD_FINGERPRINT_CHANGED`
    
- `HIGH_PRIORITY_LISTING`
    
- `CRITICAL_FIELDS_MISSING`
    
- `STALE_DETAIL_REFRESH`
    
- `PARSER_VERSION_RECHECK`
    
- `MISSING_LISTING_VERIFICATION`
    
- `MANUAL_REPROCESS_REQUEST`
    

Add scheduler configuration, tests and runbook documentation.

Tests must use fixtures only.

### Expected result

Boat24 can be maintained weekly without deep-fetching every unchanged listing.

### Check before continuing

- Weekly sweep completes.
    
- Unchanged records do not trigger details.
    
- Price changes create history.
    
- Missing records require verification.
    
- Monthly proxy limits degrade work by priority.
    
- Delta batch imports correctly.
    

---

## Step 14 — Add YPI handoff

### Goal

Create a stable output boundary between the scraper and YPI.

### Tool

Codex.

### Exact prompt

Implement the versioned scraper-to-YPI export boundary.

Do not write directly into YPI normalized, deduplicated, valuation-ready or scoring tables.

Export only:

- source identity;
    
- raw source values;
    
- snapshot references;
    
- acquisition metadata;
    
- parser metadata;
    
- field evidence;
    
- parser confidence;
    
- source-readiness signals;
    
- batch manifest;
    
- known limitations.
    

Add:

- JSON schema;
    
- export command;
    
- checksum;
    
- import-validation fixture;
    
- compatibility version;
    
- documentation;
    
- backward-compatibility test.
    

YPI remains authoritative for:

- canonical builder/model/variant;
    
- normalized boats and engines;
    
- final ownership status;
    
- cross-source dedupe;
    
- final quality eligibility;
    
- valuation-ready publication;
    
- scoring.
    

### Expected result

Boat24 data can enter YPI raw ingestion without coupling the two repositories.

### Check before continuing

- Scraper export contains source-level data only.
    
- Version compatibility is tested.
    
- YPI import validation passes.
    
- No direct business-table writes exist.
    

---

## Step 15 — Repeat for each remaining source

### Goal

Add the remaining sources without changing the platform architecture.

### Tool

Gemini, Codex, ChatGPT and Vuk repeat their assigned parts.

### Exact prompt template

We are adding `<SOURCE>` to the existing stable scraper platform.

Do not redesign the source-neutral architecture.

Follow this sequence:

1. ChatGPT prepares the source specification.
2. Gemini researches and designs the source-specific acquisition.
3. Gemini produces the protected adapter code and source-specific tests.
4. Vuk applies the approved code to a feature branch.
5. ChatGPT reviews architecture and boundaries.
6. Codex builds the offline parser and validates integration.
7. Codex fixes only approved compatibility problems.
8. Vuk runs staged pilots.
9. ChatGPT reviews Genesis readiness.
10. Vuk approves or rejects Genesis.
11. Vuk executes Genesis through the approved controller.
12. Codex activates weekly routine maintenance.
13. Codex validates the YPI handoff.

Any source-specific requirement must remain inside the source adapter, parser, source registry or source configuration. Do not change the shared architecture unless Vuk explicitly approves a documented architecture decision.

### Recommended source order

1. Boat24
    
2. Croatian Yachting
    
3. MarineOne/YachtBrokerage
    
4. Njuškalo Nautika
    
5. TheYachtMarket
    
6. iNautia
    

### Expected result

Each source has:

- source specification;
    
- protected adapter;
    
- offline parser;
    
- fixture tests;
    
- pilot report;
    
- Genesis batch;
    
- routine schedule;
    
- proxy report;
    
- source-health monitoring;
    
- YPI export validation.
    

### Check before declaring the scraper complete

All selected sources have passed their own Genesis and routine-operation gates, and failure of one source cannot stop the others.