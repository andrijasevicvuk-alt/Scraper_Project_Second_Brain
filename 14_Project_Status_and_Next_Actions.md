# Project Status and Next Actions

## Current known state

- YPI foundation Steps 1–3 are complete.
- `scraper_project` Step 1 source-neutral foundation is merged.
- Step 2 SQLite persistence and hardening are complete and merged.
- Step 2 includes packaged migrations, crawl/partition lifecycles, one authoritative queue, leases, bounded retries, checkpoints, immutable snapshot manifests, parser-run state, proxy accounting, complete dataset manifests, backups and recovery.
- Step 3A isolated synthetic-worker repository work is complete and merged. The Home PC validated `scraper_project` main at merge commit `14d864c`.
- Step 3B physical dual-node validation is complete as of 2026-09-19.
- Step 3B verified ThinkPad-to-Home-PC SSH control, native non-root Docker, synthetic execution, persistence through container recreation and physical reboot, same-run idempotency, expired-lease recovery, Ubuntu default boot and successful manual Windows boot.
- Step 3 is therefore complete.
- A separate pre-departure reliability audit found that the Home PC is **not yet demonstrated ready for approximately one month of unattended off-site operation**. The main remaining acceptance gaps are off-LAN private access, host-wide sleep prevention, off-machine restore-tested runtime backup and recovery from power/boot/network failure.
- The Step 3 result remains valid. Remote-readiness gaps are an operational acceptance gate, not a Step 3 failure.
- Gemini created acquisition, Session Broker and parser-resilience blueprints. They are preserved as prototype decisions or experiments, not production replacements.
- Gemini owns source research and blueprints.
- Jules owns new protected acquisition implementation and protected repairs.
- ChatGPT and Codex review protected code but return implementation changes to Jules.
- Codex owns source-neutral infrastructure, the authoritative queue and offline processing.

## Immediate next action

Complete the **pre-departure remote reliability acceptance gate** before unrelated scraper feature development.

Current priorities are:

1. close the remaining privileged/read-only host evidence gaps;
2. establish and test private off-LAN administration, with Tailscale → existing OpenSSH as the current proposed baseline;
3. prevent host-wide suspend/hibernate;
4. create and restore-test an off-machine runtime backup;
5. establish a simple physical recovery boundary for failures that cannot be fixed in-band;
6. verify SSH/private-service exposure safely;
7. perform realistic outside-LAN, logout and reboot acceptance tests.

Do not begin Boat24 Step 4 until this remote-reliability gate has passed or Vuk explicitly accepts any remaining risk.

The remote-readiness work must preserve the currently working Ubuntu/kernel/NVIDIA/Docker foundation and avoid unrelated upgrades or architecture changes immediately before departure.

## Later source sequence

After the remote-reliability gate:

1. ChatGPT completes the Boat24 source specification.
2. Gemini finalizes the Boat24 prototype blueprint.
3. Jules implements the protected Boat24 prototype and tests.
4. Vuk applies the protected files to a feature branch.
5. ChatGPT and Codex review; protected defects return to Jules.
6. Codex builds the offline Boat24 parser from fixtures.
7. Controlled experiments validate acquisition, session, bandwidth and parser-resilience hypotheses.
8. Vuk runs staged pilots.
9. ChatGPT audits Genesis readiness.
10. Vuk approves or rejects production and Genesis progression.

## Current blockers and unknowns

Pre-departure operational unknowns:

- Tailscale/private off-LAN access is not yet installed and externally tested.
- Effective privileged SSH/firewall configuration still needs confirmation before hardening.
- Host-wide sleep prohibition has not yet been implemented and accepted.
- A real off-machine runtime backup plus isolated restore test has not yet been demonstrated.
- Recovery after power loss, failed boot or complete home-network failure still depends on a physical helper or independently tested out-of-band capability.
- ThinkPad/Windows local-only project state and any local Supabase state require their own inventory where relevant.
- Docker logs currently lack configured rotation limits; this is a pre-unattended-workload reliability concern, not a Step 3 defect.

Source-development unknowns retained for later steps:

- Actual proxy bytes per source remain unmeasured.
- Source volume and defense-profile claims require recorded evidence.
- The Session Sync Bridge and Session Broker are not experiment-supported yet.
- Scrapling adaptive repair is not production-approved.
- Boat24 production acquisition and parser versions do not yet exist.

A previous status note said repository role documents still needed synchronization with D-010. That blocker is now superseded: current `scraper_project/AGENTS.md` and project-boundary documentation assign protected implementation to Jules. Preserve the earlier state in Git history rather than treating it as an active blocker.

These source unknowns do not invalidate completed Step 3. They are resolved through the later prototype and experiment sequence.
