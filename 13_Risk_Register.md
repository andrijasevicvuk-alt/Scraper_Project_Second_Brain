# Risk Register

## R1 — Source access changes

Impact: high

Mitigation:

- isolate source adapters
- keep fixtures and parser versions
- classify errors
- avoid coupling YPI to one source

## R2 — Codex overwrites protected acquisition code

Impact: high

Mitigation:

- protected paths in `AGENTS.md`
- file-level plans before edits
- separate branches and ownership
- user approval before shared-contract changes

## R3 — Two orchestrators retry the same job

Impact: high because it can waste proxies and duplicate state.

Mitigation:

- one authoritative queue owner per run
- one retry policy
- contract adapters do not create independent job databases

## R4 — Genesis run looks complete but has missing work

Impact: high

Mitigation:

- terminal states for every discovered listing
- partition reconciliation
- batch manifest
- no unaccounted jobs

## R5 — Proxy budget is consumed by unchanged listings

Impact: medium/high

Mitigation:

- weekly light discovery
- delta comparison
- reason-coded detail fetches
- budget degradation states
- byte ledger

## R6 — False duplicate merges

Impact: high for valuation quality.

Mitigation:

- similarity creates candidates, not automatic truth
- preserve source listings
- manual review for ambiguous clusters

## R7 — Parser fallback returns the wrong field

Impact: high

Mitigation:

- field evidence
- confidence levels
- strict-first extraction
- regression fixtures
- review signal for fuzzy fallback

## R8 — Raw storage grows without limits

Impact: medium

Mitigation:

- compression
- storage metrics
- retention rules
- no full image archive initially
- HDD archive and backup policy

## R9 — Obsidian becomes inconsistent

Impact: medium

Mitigation:

- canonical status
- decision log
- archive old notes
- update checklist after every project response

## R10 — Project becomes dependent on one AI

Impact: medium

Mitigation:

- contracts and folder ownership
- code and tests remain in Git
- agents can be replaced without replacing the architecture
