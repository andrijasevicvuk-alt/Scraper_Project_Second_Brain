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

**Probability:** reduced for ordinary reboot; still plausible for hard power, boot or home-network failure.

**Impact:** critical; the Home PC can become completely inaccessible.

**Validated mitigation:** Ubuntu automatic default boot, controlled reboot recovery, boot-enabled networking/Tailscale/OpenSSH, no last-minute BIOS/kernel/NVIDIA changes, and a simple physical-helper procedure.

**Residual boundary:** there is no independently tested out-of-band power/KVM path. If the machine is powered off, frozen before networking, unable to boot, or the home router/ISP is down, Tailscale cannot recover it.

**Physical recovery:** if Vuk asks and the PC is off, the local helper presses the power button once. If Vuk explicitly says it is frozen, hold the power button until off, wait 10 seconds, then press once. No terminal diagnosis is required.

**Status:** mitigated with accepted manual fallback; residual risk remains.

## R16 — sleep, network, overlay or remote-auth failure removes access

**Probability:** reduced after acceptance testing.

**Impact:** high/critical depending on whether another path remains.

**Validated mitigation:**

- Tailscale installed and boot-enabled;
- genuine off-LAN ThinkPad test passed over a phone hotspot;
- DERP relay fallback was observed and accepted;
- existing OpenSSH is the shell boundary;
- ThinkPad key-only login passed;
- password SSH and root SSH login are disabled;
- host-wide suspend, hibernate, hybrid sleep and suspend-then-hibernate are disabled;
- `tailscaled`, `ssh`, `NetworkManager`, `docker` and `containerd` returned automatically after a controlled reboot without GUI login.

**Residual boundary:** Tailscale/account availability, home router/ISP and hardware remain shared failure domains.

**Status:** operationally accepted for the university-away period.

## R17 — runtime exists as a single copy or backup cannot actually restore

**Probability:** materially reduced.

**Impact:** critical for queue, snapshots and collected data.

**Validated mitigation:**

- coherent SQLite backup created through the project's backup mechanism;
- snapshot/export/log/checkpoint artifacts included in the backup set;
- SHA-256 manifest verified;
- SQLite integrity check returned `ok`;
- off-machine copy confirmed on the ThinkPad;
- isolated restore through the project restore helper succeeded;
- restored SQLite integrity check returned `ok`;
- schema migrations `0001` and `0002` were preserved.

**Accepted checkpoint:** Home-PC backup `/home/vuk/Backups/ypi/20260920-035331`; ThinkPad copy `C:\Users\HT-ICT\YPI-Backups\20260920-035331`.

**Status:** mitigation validated. Future live-data operation still requires periodic fresh backups rather than relying indefinitely on this one checkpoint.

## R18 — unattended update, kernel or driver regression

**Probability:** low/unknown but consequential.

**Impact:** medium to critical depending on affected networking/boot/graphics services.

**Detection:** package logs, boot/service health and recorded kernel/driver versions.

**Current mitigation:** preserve the known-good Ubuntu/kernel/NVIDIA/Docker foundation, avoid opportunistic upgrades immediately before or during the remote-only period, and perform controlled maintenance when recovery is available.

**Status:** open operational risk; no regression was observed during remote-readiness acceptance.

## R19 — logs or generated runtime data fill the disk

**Probability:** low while idle, increasing with unattended workloads.

**Impact:** high; database writes, containers and remote administration can fail.

**Validated evidence:** final root-disk check showed approximately 352 GiB free; runtime volume remained approximately 204 KiB during acceptance.

**Mitigation:** application free-space thresholds and bounded Docker logs. A per-service Compose configuration using `json-file`, `max-size: "10m"` and `max-file: "3"` was validated and committed locally on `scraper_project` branch `chore/bounded-docker-logging`, commit `85329f6`.

**Important:** that logging branch was not yet merged into `main` at the acceptance checkpoint, so the limit is prepared but must not be treated as active on `main` until reviewed and merged.

**Remote recovery:** stop the offending workload and remove only approved expendable logs/cache; never auto-prune the runtime volume.

**Status:** partially mitigated; merge/activation of the bounded-logging change remains a small follow-up before long unattended scraper runs.

## R20 — important ThinkPad/Windows/local-service state is not included in backup

**Probability:** unknown until each device is inventoried.

**Impact:** medium/high; local-only branches, uncommitted work, local databases or configuration may be lost.

**Detection:** per-device Git/worktree/database inventory.

**Prevention:** push committed branches, back up uncommitted and database state appropriately, separate secrets from ordinary source archives.

**Remote recovery:** recover from verified off-device copies.

**Physical recovery:** access original machine/storage when no copy exists.
