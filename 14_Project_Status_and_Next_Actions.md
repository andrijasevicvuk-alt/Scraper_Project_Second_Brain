# Project Status and Next Actions

## Current known state

- YPI scraper foundation Steps 1–3 are complete.
- `scraper_project` Step 1 source-neutral foundation is merged.
- Step 2 SQLite persistence and hardening are complete and merged.
- Step 2 includes packaged migrations, crawl/partition lifecycles, one authoritative queue, leases, bounded retries, checkpoints, immutable snapshot manifests, parser-run state, proxy accounting, complete dataset manifests, backups and recovery.
- Step 3A isolated synthetic-worker repository work is complete and merged. The Home PC validated `scraper_project` main at merge commit `14d864c`.
- Step 3B physical dual-node validation is complete.
- Step 3 therefore remains complete; the later remote-readiness work did not reopen or redefine Step 3.

### Step 3 completion evidence

Step 3 demonstrated, with synthetic/local-only data:

- ThinkPad control of the Home PC through SSH;
- native non-root Docker operation;
- source-neutral synthetic worker execution;
- persistent SQLite/runtime state across container recreation;
- persistence across a full physical Home-PC reboot;
- same-run idempotency without duplicate queue work, snapshot creation or dataset-batch creation;
- abandoned leased-job recovery after expiry;
- persistent runtime layout under the named Docker volume `scraper_project_scraper-runtime`;
- Ubuntu automatic default boot;
- successful manual Windows 10 boot followed by automatic return to Ubuntu.

The Step 3 runtime checkpoint still uses approximately:

```text
/app/runtime/
├── database/scraper.sqlite
├── checkpoints/
├── snapshots/
├── logs/
└── exports/
```

## Pre-departure remote reliability gate

**Status: passed/accepted on 2026-09-20 with documented residual physical-recovery risk.**

The Home PC was prepared for approximately one month of remote-only access after Step 3.

Verified acceptance evidence:

- Tailscale installed on both machines;
- Home PC Tailscale IP `100.108.39.117`;
- ThinkPad Tailscale IP observed as `100.117.143.124`;
- genuine off-LAN ThinkPad test passed over a phone hotspot;
- DERP Frankfurt relay fallback worked when no direct Tailscale path was established;
- existing OpenSSH remains the remote shell; Tailscale SSH remains disabled;
- key-only SSH works;
- password-only SSH is rejected;
- root SSH login is disabled;
- SSH syntax/effective-policy checks passed;
- host-wide suspend, hibernate, hybrid sleep and suspend-then-hibernate are disabled;
- after a controlled reboot and without local graphical login, `tailscaled`, `ssh`, `NetworkManager`, `docker` and `containerd` were active and enabled;
- final root-disk check showed approximately 352 GiB available;
- runtime volume identity remained unchanged;
- runtime SQLite `PRAGMA quick_check` returned `ok`.

### Backup and recovery acceptance

A coherent recovery checkpoint was demonstrated:

```text
Home PC backup:
/home/vuk/Backups/ypi/20260920-035331

ThinkPad off-machine copy:
C:\Users\HT-ICT\YPI-Backups\20260920-035331

Isolated restore test:
/home/vuk/RestoreTests/20260920-035331
```

Evidence:

- SQLite backup created using the project's backup implementation;
- snapshot, export, log and checkpoint directories copied into the backup set;
- backup inventory and SHA-256 manifest created;
- every recorded checksum validated;
- backup SQLite `PRAGMA integrity_check` returned `ok`;
- ThinkPad copy was confirmed with the expected 163840-byte database;
- isolated restore through `restore_database()` succeeded;
- restored SQLite `PRAGMA integrity_check` returned `ok`;
- schema migrations `0001` and `0002` were preserved.

### Docker unattended-log follow-up

A scoped Docker Compose change was validated and committed locally in `scraper_project`:

```text
branch: chore/bounded-docker-logging
commit: 85329f6
policy: json-file, max-size 10m, max-file 3
```

At the remote-readiness checkpoint:

- no scraper Compose services were running;
- no containers needed recreation;
- the runtime volume remained unchanged;
- the branch had **not** yet been merged into `main`.

Treat bounded Docker logging as prepared but not active on `main` until that branch is reviewed and merged.

## Immediate next action

The remote-reliability gate no longer blocks source development.

The next project task is **Step 4 — prepare and approve the Boat24 source specification**.

Sequence:

1. ChatGPT reviews the current Second Brain and repository boundary.
2. Prepare the Boat24 source specification without inventing protected implementation details.
3. Preserve the existing role split:
   - Gemini — source research and prototype blueprint;
   - Jules — protected source-specific implementation and protected tests;
   - Codex — source-neutral platform, offline parser and integration;
   - Vuk — approval, application, commit and merge gate.
4. Do not begin live Boat24 acquisition until the later source-specific experiment and pilot gates authorize it.

## Remaining operational risks

These do not reopen Step 3 or block Step 4, but they remain real:

- a total power loss, failed boot before networking, hardware failure or home-router/ISP failure cannot be repaired through Tailscale;
- there is no independently tested out-of-band KVM/power-control path;
- the physical fallback remains a trusted local helper following Vuk's simple power/restart instruction;
- Tailscale/account/provider availability remains part of the remote-access failure domain;
- the bounded Docker logging branch should be reviewed and merged before long unattended scraper workloads;
- the 2026-09-20 backup is a recovery checkpoint, not a substitute for future periodic backups once live data starts accumulating;
- ThinkPad/Windows local-only project state and any unrelated local databases still require their own normal backup discipline.

## Source-development unknowns retained for later steps

- Actual proxy bytes per source remain unmeasured.
- Source volume and defense-profile claims require recorded evidence.
- The Session Sync Bridge and Session Broker are not experiment-supported yet.
- Scrapling adaptive repair is not production-approved.
- Boat24 production acquisition and parser versions do not yet exist.

These unknowns are resolved through the source specification, prototype, controlled-experiment, pilot and Genesis sequence. They do not invalidate the completed foundation or remote-readiness acceptance.
