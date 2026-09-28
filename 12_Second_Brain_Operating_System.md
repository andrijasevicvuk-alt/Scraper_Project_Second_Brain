# Second Brain Operating System

The Second Brain is a human-readable control plane for architecture, source knowledge, experiments, roles, prompts and operating evidence.

## What it should do now

- remember architecture decisions;
- keep agent boundaries clear;
- track source and version status;
- store experiment plans and results;
- keep prompts and handoffs;
- record proxy and storage assumptions;
- maintain runbooks and recovery steps;
- prevent repeated mistakes;
- identify the exact next action.

## What it should not do yet

Do not build a complex autonomous memory or RAG platform before the scraper and YPI core are stable.

The Second Brain is not an activity log. Do not record every command, conversation, small test or code change. Add information only when it materially helps future understanding, debugging, maintenance, handoff or continuation of the project.

## Source-of-truth hierarchy

1. running code and tests;
2. migrations and database schema;
3. dataset manifests, fixtures and telemetry;
4. canonical Second Brain notes and Decision Log;
5. archived notes and old conversations.

Obsidian does not override verified repository evidence.

## Implementation evidence status

Use these labels when describing whether a concrete feature, service or operating capability exists:

- `PLANNED` — documented intention, but the implementation is not present;
- `IMPLEMENTED` — code/configuration exists, but the required acceptance evidence has not all been demonstrated;
- `VERIFIED` — the implementation exists and the relevant test, runtime or acceptance evidence has passed;
- `PARTIAL` — some of the intended boundary exists, but an important part is deliberately missing or incomplete;
- `UNKNOWN` — available evidence cannot establish the current state.

These labels describe implementation evidence. They are separate from source/design maturity such as `hypothesis`, `prototype_decision`, `experiment_supported` and `production_approved`.

A past verification proves the recorded checkpoint, not the present live state of a machine or external service. For example, a Home-PC reboot test that passed on one date does not justify saying the machine is online today without a current check.

## Repository-specific step numbering

YPI and `scraper_project` have independent historical step sequences.

Always use a repository/project prefix when referring to a numbered step if confusion is possible:

- `scraper_project Step 4` — currently the Boat24 source-specification phase;
- `YPI Step 4` — the YPI raw-to-normalized bootstrap pipeline;
- `YPI Step 5` — the valuation-ready/retrieval/scoring-contract work.

Do not translate one repository's step number into the other repository's roadmap.

## Note status

Use note lifecycle status separately from design maturity.

Possible note statuses:

- `canonical`
- `draft`
- `experiment`
- `superseded`
- `archive`

## Design and evidence maturity

Use these labels for architecture, tools and source-specific behaviour:

- `hypothesis` — untested idea or claim;
- `prototype_decision` — selected so implementation and testing can begin, but not proven;
- `experiment_supported` — validated by a recorded, reproducible controlled experiment;
- `production_approved` — accepted by Vuk for routine use after meeting defined criteria.

A production-approved component may later be deprecated if evidence changes.

## Canonical-note rule

Before creating a new note:

1. search for an existing canonical location;
2. avoid duplicated instructions;
3. link the note from HOME or the relevant source note;
4. identify anything it supersedes;
5. preserve useful historical evidence in `archive/` when necessary.

## Public-repository documentation rule

At the 2026-09-28 audit checkpoint, GitHub reports the Second Brain, `scraper_project` and YPI repositories as public.

Until Vuk changes repository visibility:

- do not store credentials, tokens, cookies, private keys or session material;
- do not store exact private-network addresses, SSH fingerprints, authentication links or other unnecessary operational identifiers;
- prefer role names, service names and generic local paths over personal-machine identifiers;
- treat operational details as publishable information before committing them;
- a future change to repository visibility requires Vuk approval and does not weaken the normal secret-handling rules.

## Second Brain transparency and selective updates

Nothing may be added, removed, rewritten, reorganized or otherwise changed in the Second Brain without Vuk being told.

Second Brain updates should be few and durable. Update it when information is genuinely useful for future guidance, such as:

- an important architectural or technical decision;
- a material project-state or roadmap change;
- a meaningful milestone;
- a significant incident or resolution;
- a durable constraint, known issue or recovery procedure;
- information another ChatGPT, Codex, Gemini or Jules session needs to continue safely.

Do not make wording, formatting or structural changes merely for cosmetic cleanup.

Before a change, identify where practical:

- the file and section;
- why the change is necessary;
- what will be added;
- what will be changed;
- what will be removed, if anything;
- whether roadmap, architecture, project state, assumptions, decisions or next steps are affected.

After every Second Brain modification, report the same information clearly. If nothing changed, state explicitly that no Second Brain changes were made.

Do not silently rewrite history. When a previous assumption or decision becomes wrong, preserve useful historical context and record what superseded it, why, and what the current decision is.

Git history is the primary lightweight Second Brain change log. The Decision Log records important decisions. Do not create a duplicate activity-log system unless Git history proves insufficient.

Where practical, significant Second Brain updates should have an obvious documentation commit such as:

`docs(second-brain): record remote worker operating model`

Small documentation changes directly associated with a feature may share that feature commit, but they must still be reported to Vuk.

Before ending any major project task, include a short Second Brain status stating either that no changes were required or which files/sections were updated and why.

## Session rule for ChatGPT

For project work, ChatGPT should:

- check the Second Brain first and `scraper_project` second;
- preserve Jules' protected implementation boundary;
- preserve Gemini's source-blueprint responsibility;
- never silently replace existing protected acquisition code;
- distinguish hypotheses, prototype decisions, experimental evidence and production approval;
- return protected-code defects as Jules repair prompts rather than direct rewrites;
- finish project answers with exact Obsidian changes.

## Session rule for Gemini

Gemini should maintain source and architecture blueprints, label maturity honestly, link experiments and generate implementation-ready Jules prompts.

## Session rule for Codex

Codex should implement source-neutral infrastructure and offline parsing, test protected code through contracts and return protected defects to Jules.

Codex must not silently modify the Second Brain as a side effect of another task. Second Brain edits require explicit reporting under the transparency rules above.
