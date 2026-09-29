---
status: canonical
last_audited: 2026-09-28
---

# Session Start

This is the short entrypoint for a new ChatGPT, Codex, Gemini or Jules project session.

Do not use it as a substitute for repository inspection. It tells the session where to look and which assumptions are currently safe.

## 1. Project map

There are three related repositories with different responsibilities:

### `scraper_project`

Source acquisition and source-readiness platform.

Owns:

- source discovery;
- protected source acquisition through approved adapters;
- raw snapshots and acquisition telemetry;
- source-neutral crawl state, queue, checkpoints and retries;
- offline source parsing;
- source-readiness signals;
- versioned dataset batches.

### YPI

Product/data-engine repository.

Owns:

- raw business ingestion;
- canonical normalization;
- normalized boats/listings;
- cross-source dedupe/business quality;
- valuation-ready comparables;
- comparable retrieval;
- scoring/valuation;
- product UI.

See [[YPI]].

### Scraper Project Second Brain

Human-readable project memory, architecture, decisions, source notes, experiments, handoffs and runbooks.

It does not override code, migrations, tests or runtime evidence.

## 2. Current checkpoint

### scraper_project

- Step 1 — **VERIFIED**
- Step 2 — **VERIFIED**
- Step 3A — **VERIFIED**
- Step 3B — **VERIFIED**
- separate remote-reliability gate — **VERIFIED at the 2026-09-20 checkpoint**
- Step 4 Boat24 source specification — **CURRENT**
- live Boat24 adapter/parser/pilot/Genesis/routine — **PLANNED**

Current audited `scraper_project` main checkpoint: merge commit `14d864c`.

### YPI

Current audited YPI main checkpoint: `00cc1eb`.

- manual-entry raw ingestion — **VERIFIED**
- narrow bootstrap raw→normalized pipeline — **VERIFIED**
- valuation-ready view/retrieval contracts — **IMPLEMENTED**
- same-builder/same-model comparable retrieval — **IMPLEMENTED**
- scoring contract — **IMPLEMENTED**, but real scoring is **PLANNED**
- final valuation range/confidence/UI — **PLANNED**
- atomic publication for high-volume scraper ingestion — **PARTIAL / required later**

### Home PC

The dual-node worker architecture was verified, including off-LAN administration, reboot recovery and backup/restore.

The Home PC's current power/network/service state is **UNKNOWN** until checked.

Vuk is currently at college. Routine planning and repository work must not require physical Home-PC access.

## 3. Do not confuse numbered roadmaps

Always say which repository owns the step.

Examples:

- `scraper_project Step 4` = Boat24 source specification;
- `YPI Step 4` = narrow raw-to-normalized bootstrap pipeline;
- `YPI Step 5` = valuation-ready/retrieval/scoring-contract work.

See D-016 in [[15_Decision_Log]].

## 4. Evidence labels

Use:

- `PLANNED`
- `IMPLEMENTED`
- `VERIFIED`
- `PARTIAL`
- `UNKNOWN`

Do not confuse these with maturity labels:

- `hypothesis`
- `prototype_decision`
- `experiment_supported`
- `production_approved`

See [[12_Second_Brain_Operating_System]].

## 5. Required reading for a new implementation task

Read only what the task requires, starting with:

1. this note;
2. [[14_Project_Status_and_Next_Actions]];
3. [[02_Project_Boundaries_and_AI_Roles]];
4. relevant source note;
5. `scraper_project/AGENTS.md`;
6. actual technical files/tests/migrations;
7. [[16_Implementation_Prompt_Sequence]] for detailed step instructions.

For YPI facts, inspect the YPI repository rather than treating this vault as authoritative technical evidence.

## 6. Current immediate task

Finalize and approve [[sources/01_Boat24]] as the complete `scraper_project Step 4` source specification.

This can be done while Vuk is at college.

Do not start live acquisition merely to complete the source specification.

## 7. Current important gates

Before later live/production phases:

- Boat24 specification must be approved;
- Gemini must produce a current evidence-based blueprint;
- Jules must implement protected source code/tests;
- representative fixtures must exist;
- Codex offline parser must pass;
- staged pilots must measure reliability/cost;
- Vuk must approve production/Genesis;
- YPI transactional publication must be hardened before scheduled/high-volume scraper→YPI ingestion;
- the local-only Docker log-rotation branch must be recovered/reviewed/merged before long unattended live worker operation.

## 8. Repository visibility

At the 2026-09-28 audit, GitHub reported the Second Brain, `scraper_project` and YPI repositories as public.

Do not add private-network identifiers, secrets, credentials, session material or proprietary operational details without first reconsidering repository visibility.

## 9. Session-end rule

Before ending a significant task:

1. state what technical evidence changed;
2. decide whether a durable Second Brain update is justified;
3. report exact Second Brain changes;
4. do not turn the vault into an activity log;
5. leave the exact next action unambiguous.
