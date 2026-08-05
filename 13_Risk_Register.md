# Risk Register

## R1 — Source access changes

Impact: high.

Mitigation:

- isolate source adapters;
- keep fixtures and acquisition/parser versions;
- classify errors;
- avoid coupling YPI to one source.

## R2 — Reviewer overwrites Jules protected code

Impact: high.

Mitigation:

- protected paths in `AGENTS.md` and [[02_Project_Boundaries_and_AI_Roles]];
- ChatGPT and Codex review without direct replacement;
- every protected correction becomes a precise Jules repair prompt;
- Vuk applies and commits accepted changes.

## R3 — Session Broker becomes a second orchestrator

Impact: high because it can duplicate retries, waste proxies and corrupt queue state.

Mitigation:

- one authoritative Codex queue owner;
- protected broker owns session refresh only;
- no broker job requeue or checkpoint ownership;
- bounded protected attempts return classified outcomes to orchestration.

## R4 — Genesis run looks complete but has missing work

Impact: high.

Mitigation:

- terminal states for every discovered listing;
- partition reconciliation;
- batch manifest;
- no unaccounted jobs.

## R5 — Proxy budget is consumed by unchanged listings or browser assets

Impact: medium/high.

Mitigation:

- weekly light discovery;
- delta comparison;
- reason-coded detail fetches;
- source-specific asset-blocking experiments;
- byte ledger and budget degradation states.

## R6 — False duplicate merges

Impact: high for valuation quality.

Mitigation:

- similarity creates candidates, not truth;
- preserve source listings;
- manual review for ambiguous clusters.

## R7 — Adaptive parser repair returns the wrong field

Impact: high.

Mitigation:

- strict-first extraction;
- field evidence and type/range validation;
- selector uniqueness and context checks;
- independent-extractor comparison;
- cross-page and shadow-mode tests;
- versioned selector/fingerprint sets;
- medium-confidence quarantine and low-confidence human review.

## R8 — Runtime auto-save silently changes production behaviour

Impact: high.

Mitigation:

- active production versions are immutable;
- candidates write only to quarantine;
- promotion creates a new version and evidence record;
- rollback retains the previous version.

## R9 — Prototype decisions are presented as proven architecture

Impact: medium/high.

Mitigation:

- canonical maturity vocabulary;
- Experiment Ledger links;
- Vuk production-approval gate;
- Master Inventory separates prototype and production status.

## R10 — Raw storage grows without limits

Impact: medium.

Mitigation:

- compression;
- storage metrics;
- retention rules;
- no full image archive initially;
- HDD archive and backup policy.

## R11 — Second Brain becomes inconsistent

Impact: medium.

Mitigation:

- canonical status;
- Decision Log;
- archive duplicate notes;
- update checklist after project changes.

## R12 — Project becomes dependent on one AI

Impact: medium.

Mitigation:

- contracts and folder ownership;
- code and tests remain in Git;
- blueprints and repair prompts are explicit;
- agents can be replaced without replacing the architecture.
