# Project Status and Next Actions

## Current known state

- YPI foundation Steps 1–3 are complete.
- `scraper_project` Step 1 source-neutral foundation is merged.
- Step 2 SQLite persistence and hardening are complete and merged.
- Step 2 includes packaged migrations, crawl/partition lifecycles, one authoritative queue, leases, bounded retries, checkpoints, immutable snapshot manifests, parser-run state, proxy accounting, complete dataset manifests, backups and recovery.
- The synthetic suite has 29 passing tests and GitHub Actions was green at the recorded checkpoint.
- The dual-node concept is defined but must be operationally proven.
- Gemini created acquisition, Session Broker and parser-resilience blueprints. They are preserved as prototype decisions or experiments, not production replacements.
- Gemini owns source research and blueprints.
- Jules owns new protected acquisition implementation and protected repairs.
- ChatGPT and Codex review protected code but return implementation changes to Jules.
- Codex owns source-neutral infrastructure, the authoritative queue and offline processing.

## Immediate next action

Begin Step 3A from [[16_Implementation_Prompt_Sequence]]: extend and validate the existing source-neutral Docker scaffold using repository work, synthetic fixtures and CI.

Physical ThinkPad-to-Home-PC validation is Step 3B and is deferred until Vuk has access to the Home PC.

Step 3 does not implement, remove or evaluate protected acquisition tactics, experimental interaction tools, Session Sync, Session Broker, Scrapling resilience or live source access.

## Later source sequence

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

- Step 3B physical dual-node validation requires access to the Home PC. This does not block Step 3A repository and CI implementation.
- Actual proxy bytes per source remain unmeasured.
- Source volume and defense-profile claims require recorded evidence.
- The Session Sync Bridge and Session Broker are not experiment-supported yet.
- Scrapling adaptive repair is not production-approved.
- Boat24 production acquisition and parser versions do not yet exist.
- Before Step 5–6 implementation, synchronize `scraper_project/AGENTS.md`, `README.md` and `docs/project-boundaries.md` with D-010 so repository rules no longer assign protected code authoring to Gemini.

These unknowns do not block Step 3. They are resolved through the prototype and experiment sequence rather than more speculative architecture.
