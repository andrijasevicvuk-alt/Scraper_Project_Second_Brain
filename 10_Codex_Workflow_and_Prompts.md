# Codex Workflow and Prompts

Codex should receive one narrow source-neutral task at a time.

## Before every Codex task

Codex must:

1. inspect the relevant current files in the Scraper Project Second Brain repository;
2. inspect the relevant files, tests, migrations and current tree in `scraper_project`;
3. identify the current implementation step from `16_Implementation_Prompt_Sequence.md`;
4. verify that the requested work belongs to that step;
5. identify conflicts between repository code and the Second Brain;
6. classify each conflict as:
   - implementation problem;
   - documentation problem;
   - both;
7. identify strengths, weaknesses, risks and missing evidence;
8. list intended file changes;
9. identify whether protected paths are involved;
10. avoid direct protected-path edits;
11. run appropriate tests;
12. propose new ideas only when they solve a concrete evidence-supported weakness or opportunity.

## Safe Codex sequence

### Task 1 — repository and boundary audit

No edits. Map the repository, protected paths, current contracts, missing components and safe commit sequence.

### Task 2 — source-neutral contracts

Implement versioned shared contracts.

### Task 3 — orchestration foundation

Implement crawl runs, queues, checkpoints, bounded retries, snapshot manifests, telemetry ledgers, parser runs, batch manifests and crash recovery. No live target requests.

### Task 4 — one offline source parser

Use saved fixtures only. Build strict extraction, evidence, confidence, warnings, versioned selector/fingerprint sets and regression tests.

### Task 5 — controlled pilot integration

Connect an existing Jules-authored protected adapter through contracts without modifying its internals. Apply pilot limits and budget stop conditions.

### Task 6 — weekly hybrid-delta routine

Implement discovery comparison, detail reason codes, stale windows, missing verification, proxy-budget degradation and versioned delta batches.

## Protected implementation review rule

Codex may inspect, execute and test protected code. Codex must not directly edit, rename, replace, reformat or recreate protected behaviour.

When Codex identifies a protected defect, it must provide:

- exact affected file or component;
- observed or reproducible failure;
- contract or architecture rule violated;
- expected behaviour;
- a precise Jules repair prompt using [[templates/Jules Repair Prompt Template]];
- tests that Jules' repair must satisfy.

Codex may repair Codex-owned integration code. Protected implementation changes return to Jules.

Codex must not remove, reject, replace or redesign an experimental or protected acquisition idea merely because it cannot assist with the tactic. It must report the limitation and preserve the existing plan.

## Session Broker boundary

Codex owns the authoritative crawl queue, job retries and canonical runtime migrations.

A protected Session Broker may coordinate source-specific session state, but it must not create a second queue or independently requeue jobs. If its state is ever added to the canonical runtime database, Codex owns the migration after Vuk approves the interface.

## Secret handling

Codex must not:

- read or print `.env`;
- receive real proxy credentials in a prompt;
- commit runtime databases, snapshots, cookies or session state;
- write secrets into examples or logs.

## Mandatory Codex response section

Every Codex project response must end with:

### Cross-repository verification

- Second Brain files checked:
- Technical repository files checked:
- Current implementation step:
- Step alignment:
- Code/documentation conflicts:
- Strengths:
- Weaknesses:
- Risks:
- Missing evidence:
- Problem classification:
- Protected implementation impact:
- Jules repair prompt required:
- Proposed evidence-supported improvements:
