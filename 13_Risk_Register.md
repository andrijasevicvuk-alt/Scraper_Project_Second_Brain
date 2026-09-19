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
- bound log growth;
- use an approved archive policy when the HDD is intentionally brought into service;
- maintain an off-machine recovery copy for important runtime/data rather than treating the currently unmounted HDD as a backup.

## R11 — Second Brain becomes inconsistent

Impact: medium.

Mitigation:

- canonical status;
- Decision Log;
- archive duplicate notes;
- selective updates under [[12_Second_Brain_Operating_System]];
- explicit reporting of every Second Brain change.

## R12 — Project becomes dependent on one AI

Impact: medium.

Mitigation:

- contracts and folder ownership;
- code and tests remain in Git;
- blueprints and repair prompts are explicit;
- agents can be replaced without replacing the architecture.

### R13 — silent architecture replacement

**Description:** A reviewer removes, downgrades or replaces an existing acquisition idea because it cannot implement or support the tactic.

**Mitigation:** D-013, Vuk approval, protected ownership boundaries and mandatory conflict reporting.

### R14 — uncontrolled experimental-tool accumulation

**Description:** Every possible tool is added to each source adapter without evidence.

**Mitigation:** Gemini evaluates tools per source, Jules implements only approved components, and promotion requires controlled experiments.

## R15 — complete remote lockout from power or boot failure

**Probability:** unknown / plausible during unattended operation.

**Impact:** critical; the Home PC can become completely inaccessible.

**Detection:** the worker stops responding from an independent external network and no in-band service is reachable.

**Prevention:** preserve the known-good boot path, avoid last-minute kernel/driver/BIOS changes, document power dependencies and establish a simple local recovery owner.

**Remote recovery:** only possible if an independently tested out-of-band path exists.

**Physical recovery:** trusted local person powers on or performs an explicitly requested simple restart; no terminal diagnosis.

## R16 — sleep, network, overlay or remote-auth failure removes access

**Probability:** plausible.

**Impact:** high/critical depending on whether an alternate path remains.

**Detection:** external reachability and service-status checks.

**Prevention:** host-wide sleep prohibition, wired autoconnect, boot-enabled remote services, off-LAN reboot/logout tests, account/key-expiry review and narrow access policy.

**Remote recovery:** restart the failed service if another shell remains available.

**Physical recovery:** wake/restart only when Vuk explicitly requests it.

## R17 — runtime exists as a single copy or backup cannot actually restore

**Probability:** unknown until backup acceptance is complete.

**Impact:** critical for queue, snapshots and collected data.

**Detection:** backup-age/inventory checks and an isolated restore test.

**Prevention:** coherent SQLite/artifact backups, checksums, off-machine copy and periodic restore validation.

**Remote recovery:** restore into a fresh target from a verified backup.

**Physical recovery:** disk recovery or hardware replacement if no valid remote copy exists.

## R18 — unattended update, kernel or driver regression

**Probability:** low/unknown but consequential.

**Impact:** medium to critical depending on affected networking/boot/graphics services.

**Detection:** package logs, boot/service health and recorded kernel/driver versions.

**Prevention:** no opportunistic upgrades before departure, explicit no-automatic-reboot policy, controlled maintenance windows and preservation of known-good boot state.

**Remote recovery:** package/service rollback when SSH remains available.

**Physical recovery:** local boot recovery if remote access is lost.

## R19 — logs or generated runtime data fill the disk

**Probability:** low while idle, increasing with unattended workloads.

**Impact:** high; database writes, containers and remote administration can fail.

**Detection:** disk/inode checks plus Docker/application log-size monitoring.

**Prevention:** Docker log rotation, application retention policy and free-space thresholds; never auto-prune the runtime volume.

**Remote recovery:** remove only approved expendable logs/cache and stop the offending process.

**Physical recovery:** local cleanup only if the disk state prevents remote login.

## R20 — important ThinkPad/Windows/local-service state is not included in backup

**Probability:** unknown until each device is inventoried.

**Impact:** medium/high; local-only branches, uncommitted work, local databases or configuration may be lost.

**Detection:** per-device Git/worktree/database inventory.

**Prevention:** push committed branches, back up uncommitted and database state appropriately, separate secrets from ordinary source archives.

**Remote recovery:** recover from verified off-device copies.

**Physical recovery:** access original machine/storage when no copy exists.
