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
    
- Protected acquisition paths are documented.
    
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
    
5. Confirm explicitly that no protected acquisition path will be modified.
    

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
    
- Do not modify, rename, move, replace or reformat protected acquisition paths.
    
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

Do not modify any protected acquisition path.

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
- protected acquisition paths untouched
- shared contracts unchanged

## Step 2A — Harden SQLite persistence

Status: complete and merged through PR #2.

---

## Step 3 — Prove the isolated Docker and dual-node flow

### Goal

Prove the platform works between the ThinkPad control node and home-PC worker without involving a live target.

### Tool

Codex extends the source-neutral infrastructure. Vuk runs it manually.

### Exact prompt

Implement the isolated local-worker infrastructure for `scraper_project`.

Read and obey `AGENTS.md`.

Before editing:

1. Check the relevant current Second Brain files.
2. Confirm that Step 3 is the current implementation step.
3. Inspect the current technical repository tree, Docker files, runtime documentation and tests.
4. Report any conflict between the Second Brain and repository documentation.
5. Distinguish documentation conflicts from implementation defects.

Do not add or evaluate source-acquisition dependencies during Step 3.

Specifically, do not implement or remove:

- `curl_cffi`;
- Nodriver;
- Playwright;
- Scrapling;
- Crawlee;
- Stagehand;
- AgentQL;
- OxyMouse;
- HumanMoveMouse;
- source-specific scrolling, keyboard or header-locality behaviour.

These remain later source-specific or experimental concerns.

Do not modify protected acquisition code and do not make live requests.

Extend the existing root `Dockerfile` and `docker-compose.yml` rather than creating competing Docker scaffolding.

Preserve the existing:

- non-root worker user;
- read-only container filesystem;
- temporary filesystem configuration;
- persistent `/app/runtime` volume;
- packaged migration source.

Add:

- persistent volumes or approved subdirectories for runtime database, checkpoints, snapshots, logs and exports;
- a local synthetic fixture server;
- worker health command;
- graceful shutdown handling;
- resume-after-restart support;
- CPU and memory defaults suitable for a 16 GB RAM worker;
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
- use Docker Compose;
- do not implement Session Sync, Session Broker, Scrapling or live source acquisition in Step 3.

Create an end-to-end fixture test:

`synthetic discovery → discovery observation → detail job → synthetic snapshot → manifest → parser placeholder → dataset batch`

Document how Vuk can:

1. start the worker on the home PC;
2. check it from the ThinkPad;
3. stop it safely;
4. restart it;
5. prove the queue resumes.

### Step 3A — Repository and CI validation

Codex implements and validates everything that can be completed using the repository, synthetic fixtures, Docker configuration and CI.

### Step 3B — Physical dual-node validation

This is performed later by Vuk when the Home PC is available:

- start the worker on the Home PC;
- inspect it from the ThinkPad;
- stop and restart it;
- verify persistent runtime state;
- verify snapshots survive;
- verify queue resume.

Step 3A may be completed while Step 3B remains pending. Step 3 is fully complete only after both pass.
### Expected result

The existing project scaffold runs inside an isolated container, persists state and recovers after interruption.

### Check before continuing

- Container runs as non-root.
- Existing Docker safeguards remain.
- A rebuild does not remove snapshots.
- A restart does not lose queue state.
- No secrets appear in logs.
- Synthetic end-to-end test passes.
- Only Docker Compose is used.

---

## Step 4 — Write the Boat24 source specification

### Goal

Define exactly what Boat24 contributes before finalizing its prototype acquisition blueprint.

### Tool

ChatGPT, reviewed manually by Vuk.

### Exact prompt

Check the Scraper Project Second Brain repository first.

Using [[templates/Source Note Template]] and [[19_Prototype_Scraper_Workflow]], prepare the complete Boat24 source specification.

Include:

- business role in YPI;
- in-scope and excluded categories;
- geographic scope;
- required and optional source-level fields;
- stable identity requirements;
- list/detail responsibilities;
- expected discovery partitions;
- snapshot and contract requirements;
- offline parser responsibilities;
- quality limitations;
- pilot limits;
- proxy metrics;
- Genesis completion requirements;
- routine refresh policy;
- evidence references and maturity status.

Use only:

- `hypothesis`;
- `prototype_decision`;
- `experiment_supported`;
- `production_approved`.

Do not design, implement or replace Jules-authored protected acquisition internals. Preserve existing approved protected implementations.

End with exact Obsidian changes.

### Expected result

One approved source specification that Gemini can use without inventing YPI requirements.

### Check before continuing

- Source scope is clear.
- Required fields are explicit.
- Identity rule is explicit.
- List and detail responsibilities are separate.
- Unverified assumptions are labelled.
- Vuk approves the note.

---

## Step 5 — Design the Boat24 acquisition prototype

### Goal

Create an implementation-ready source-specific acquisition blueprint while respecting existing protected implementations and shared contracts.

### Tool

Gemini.

### Exact prompt

You are designing the protected Boat24 acquisition prototype for `scraper_project`.

Read:

- the approved Boat24 source specification;
- shared contracts;
- root `AGENTS.md`;
- [[11_Gemini_Acquisition_Blueprint_Workflow]];
- [[19_Prototype_Scraper_Workflow]];
- the three architecture prototype notes;
- relevant Experiment Ledger records;
- the current protected implementation constraints.

Do not modify code.

Design:

- discovery entry points and partitions;
- stable source identity;
- list-card observation fields;
- conditions requiring detail fetches;
- acquisition route by page type;
- existing protected behaviour that must be preserved;
- optional Session Sync and Session Broker prototype boundaries;
- raw artifact format and telemetry;
- bounded internal attempts;
- source-specific error classes;
- fixtures and experiment plan;
- safe pilot progression and proxy measurements.

The protected adapter may return:

- `DiscoveryObservation`;
- `RawFetchArtifact`;
- `FetchTelemetry`.

It must not redefine:

- Codex orchestration, queue or job retries;
- canonical database migrations;
- offline parsing, regex fallback or selector promotion;
- quality scoring;
- YPI normalization or publication;
- shared contracts.

Separate hypotheses, prototype decisions, experiment-supported claims and production-approved components.

End with:

1. protected file map;
2. complete Jules implementation prompt;
3. protected test plan;
4. Experiment Log plan;
5. unresolved questions.

Do not write code or run Genesis.

### Expected result

An approved Gemini blueprint that Jules can implement without inventing architecture.

### Check before continuing

- Existing approved protected implementation is preserved.
- Contracts are used without redefinition.
- Queue and parser boundaries remain intact.
- Every uncertain claim has honest maturity.
- Pilot and experiment limits are explicit.
- Vuk approves the prototype direction.
- Repository role documents are synchronized with D-010 before Jules implementation begins.

---

## Step 6 — Implement the protected Boat24 prototype

### Goal

Produce the complete Boat24 protected acquisition prototype from the approved Gemini blueprint.

### Tool

Jules produces protected code and tests. Vuk applies it manually. ChatGPT and Codex review without directly rewriting protected implementation.

### Exact prompt

Implement the approved Boat24 acquisition prototype inside the protected acquisition zone.

Read and obey:

- root `AGENTS.md`, which must already reflect D-010 Jules ownership;
- approved Boat24 source specification;
- approved Gemini blueprint;
- shared contracts;
- [[18_Jules_Protected_Implementation_Workflow]].

You may modify only the protected files explicitly listed in the approved blueprint.

Do not modify:

- shared contracts;
- Codex orchestration or queue;
- database migrations;
- offline parsers or selector/fingerprint promotion;
- normalizers, validators, dedupe or quality scoring;
- YPI code;
- unrelated protected implementations.

Required outputs:

- valid `DiscoveryObservation`, `RawFetchArtifact` and `FetchTelemetry` values;
- acquisition-version identifier;
- classified failures;
- bounded internal attempts;
- protected Session Broker only if approved by the blueprint;
- repeatable worker command;
- fixture collection plan and bounded commands;
- byte telemetry;
- protected tests;
- known limitations.

If root repository ownership rules still assign protected code authoring to Gemini, stop and request documentation synchronization before editing.

Do not run a full Genesis scrape.

Return complete files or unified patches. Do not commit, push, open a pull request or merge.

### Review rule

If ChatGPT or Codex finds a defect, they must provide a precise Jules repair prompt. They must not directly overwrite the protected implementation.

### Expected result

A Jules-authored Boat24 prototype that returns raw source artifacts without changing source-neutral responsibilities.

### Check before continuing

- Only approved protected files changed.
- Existing approved protected behaviour was not silently replaced.
- Contracts validate.
- Attempts are bounded.
- No queue, parser or YPI ownership is duplicated.
- Fixtures can be opened offline.
- Executed tests are distinguished from planned tests.

---

## Step 7 — Build the offline Boat24 parser and resilience prototype

### Goal

Turn saved Boat24 snapshots into source-level parsed candidates without live access and validate the automated repair flow.

### Tool

Codex.

### Exact prompt

Implement the Boat24 offline parser using only approved fixtures under `tests/fixtures/boat24/`.

Read [[architecture/02_Offline_Parser_Resilience_and_Scrapling_Validation]].

Do not make network requests. Do not modify Jules-authored protected acquisition paths.

Implement:

- list-card and detail-page parsers;
- parser version;
- field-level evidence, extraction method and confidence;
- strict structured and semantic extraction;
- controlled regex/text fallback;
- parser warnings and failure reasons;
- versioned selector/fingerprint sets;
- candidate-repair quarantine;
- validation signals for similarity, type/range, context, uniqueness, previous values, cross-page consistency and independent extractors;
- shadow-mode comparison;
- high/medium/low outcome handling;
- fixture-based regression tests.

Promotion rules:

- high confidence may automatically create a new immutable promoted version after all hard gates and configured shadow criteria pass;
- medium confidence remains quarantined for additional testing;
- low confidence requires human review;
- runtime auto-save never mutates the active production version.

Do not perform final YPI mapping, cross-source merge, valuation eligibility, scoring or publication.

At completion, list fixtures, missing fields, candidate versions, validation evidence and rollback behaviour.

### Expected result

A deterministic Boat24 offline parser plus a versioned, reproducible repair-validation prototype.

### Check before continuing

- No network requests.
- All fixtures are tested.
- Active versions are immutable.
- Candidate repair state is quarantined.
- Missing values are not invented.
- Every promoted version is traceable and reproducible.

---

## Step 8 — Connect the controlled pilot

### Goal

Connect the Jules-authored protected adapter to the source-neutral queue and Codex offline parser without changing either responsibility.

### Tool

Codex integrates source-neutral paths. ChatGPT reviews architecture. Protected defects return to Jules.

### Exact prompt

Connect the existing protected Boat24 adapter to the source-neutral orchestration platform through existing contracts.

Do not modify protected adapter internals.

Implement:

`discovery → observation validation → detail decision → detail job → protected acquisition call → raw artifact validation → immutable snapshot manifest → offline parser → source-readiness result → pilot report`

Add explicit discovery, detail-job, proxy-byte, failure-rate, parser-failure and disk-space limits; graceful pause/resume; integrity validation; coverage, cost, retry and terminal-state reporting.

Do not run pilots or Genesis.

If integration reveals a protected defect:

1. record the exact file/component and evidence;
2. explain the contract or behaviour problem;
3. create a precise Jules repair prompt;
4. continue only after Vuk applies the Jules repair.

Codex may fix only Codex-owned integration code.

### Expected result

The system is ready for controlled live Boat24 experiments and pilots.

### Check before continuing

- Protected internals remain unchanged by Codex.
- Limits stop the pilot correctly.
- Every discovered record receives a terminal state.
- Resume works.
- Reports show bytes, retries and parser coverage.
- Vuk approves pilot configuration.

---

## Step 9 — Execute staged Boat24 experiments and pilots

### Goal

Measure acquisition cost, session reliability, parser behaviour and data quality.

### Tool

Vuk executes the approved Jules-authored prototype manually. Codex assists with source-neutral operation. Gemini maintains source and experiment notes. ChatGPT reviews evidence.

### Exact prompt

Run only the approved Boat24 experiment or pilot through existing source-neutral orchestration.

Do not edit Codex-owned or Jules-authored modules during execution.

Return the experiment ID, versions, sample, discovered/detail counts, artifact outcomes, classified failures, job retries, protected internal attempts, proxy bytes, duration, session-refresh evidence, snapshot integrity, parser coverage, repair-validation outcomes, readiness distribution and terminal-state accounting.

Stop at any configured limit. Do not advance maturity or pilot size without Vuk approval.

### Expected result

Reproducible evidence that can move specific claims from hypothesis to experiment-supported and later support production approval.

### Check before continuing

- Costs are measured, not guessed.
- Queue retries and protected internal attempts are distinguished.
- Failure types are understood.
- Parser and repair versions are recorded.
- No records disappear from accounting.
- Resume and rollback are demonstrated.

---

## Step 10 — Review Genesis readiness

### Goal

Decide whether the Boat24 acquisition and parser versions are ready for full collection.

### Tool

ChatGPT reviews. Vuk decides manually.

### Exact prompt

Check the Second Brain repository first.

Review experiment records, pilot reports, terminal-state accounting, parser coverage, repair-validation evidence, proxy usage, job retries, protected internal attempts, session recovery, snapshot integrity and known limitations.

Return one decision:

- `READY_FOR_GENESIS`;
- `READY_WITH_CONDITIONS`;
- `NOT_READY`.

For every failed requirement, state evidence, severity, owner, exact fix and whether it blocks Genesis.

If the defect is inside protected implementation, create a precise Jules repair prompt. Do not change or replace Jules-authored protected code directly.

End with exact Obsidian changes.

### Expected result

A documented readiness decision for explicit acquisition and parser versions.

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

Vuk executes the approved Jules-authored acquisition version through the approved Codex source-neutral controller and parser version.

### Exact prompt

Execute the production-approved Boat24 Genesis run through existing source-neutral orchestration.

Do not change architecture, maturity, implementation version or source scope during execution.

Use approved discovery partitions, budgets, concurrency, queue retry limits, protected internal attempt limits, stop conditions, storage, acquisition version, session policy, parser version and selector/fingerprint version.

Produce final terminal-state reconciliation, raw snapshot manifest, parser report, proxy/session report, failure report, manual-review export, versioned dataset-batch manifest, checksum, YPI raw-ingestion validation and second-discovery delta test.

Pause safely rather than editing architecture or protected code when a blocker appears.

### Expected result

A complete, versioned Boat24 Genesis batch.

### Check before continuing

- All partitions completed or are explicitly classified.
- No unaccounted jobs remain.
- Batch checksum is valid.
- Acquisition, parser and selector/fingerprint versions are recorded.
- Manual QA and YPI ingestion validation pass.
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

Add the remaining sources without changing the source-neutral platform architecture.

### Tool

ChatGPT, Gemini, Jules, Codex and Vuk repeat their non-overlapping roles.

### Exact prompt template

We are adding `<SOURCE>` to the existing stable scraper platform.

Follow [[19_Prototype_Scraper_Workflow]] and do not redesign the source-neutral architecture.

Sequence:

1. ChatGPT prepares the source specification.
2. Gemini researches the source and produces the prototype blueprint and experiment plan.
3. Vuk approves the prototype direction.
4. Jules implements protected adapter code and protected tests.
5. Vuk applies Jules' files to a feature branch.
6. ChatGPT audits architecture and boundaries.
7. Codex builds the offline parser and validates source-neutral integration.
8. Protected defects return as precise Jules repair prompts; Codex fixes only Codex-owned integration code.
9. Vuk runs controlled experiments and staged pilots.
10. Gemini updates source evidence and experiment notes.
11. ChatGPT reviews production and Genesis readiness.
12. Vuk approves or rejects production/Genesis.
13. Vuk executes Genesis through the approved controller.
14. Codex activates weekly routine maintenance and validates YPI handoff.

Any source-specific requirement remains inside the source specification, protected adapter, parser, registry or configuration. Shared architecture changes require a documented Vuk decision.

### Recommended source order

1. Boat24
2. Croatian Yachting
3. MarineOne / YachtBrokerage
4. Njuškalo Nautika
5. TheYachtMarket
6. iNautia

### Expected result

Each source has a specification, Gemini blueprint, Jules protected adapter, Codex offline parser, fixtures, experiment evidence, pilot report, approved versions, Genesis batch, routine schedule, proxy report, source-health monitoring and YPI validation.

### Check before declaring the scraper complete

All selected sources have passed their own production, Genesis and routine-operation gates, and failure of one source cannot stop the others.
