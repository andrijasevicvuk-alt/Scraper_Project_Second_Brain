# Scraper Project — Second Brain

This vault is the human-readable control center for `scraper_project`, a side project that collects, stores, cleans and maintains boat-listing data for [[YPI]].

The goal is a stable data-acquisition system that can:

- build the first complete dataset through a [[05_Genesis_Scrape|Genesis Scrape]];
- maintain it through a [[06_Routine_Scrape|Routine Scrape]];
- preserve raw snapshots, telemetry and source trace;
- pass source-level batches into the YPI `raw → normalized → valuation-ready` pipeline;
- keep Gemini source blueprints, Jules protected implementation and Codex source-neutral modules separated.

## Main navigation

1. [[01_Main_Plan]]
2. [[02_Project_Boundaries_and_AI_Roles]]
3. [[03_Dual_Node_Infrastructure]]
4. [[04_Source_Registry]]
5. [[05_Genesis_Scrape]]
6. [[06_Routine_Scrape]]
7. [[07_Data_Flow_and_Contracts]]
8. [[08_Proxy_Storage_and_Costs]]
9. [[09_Quality_Dedupe_and_Review]]
10. [[10_Codex_Workflow_and_Prompts]]
11. [[11_Gemini_Acquisition_Blueprint_Workflow]]
12. [[12_Second_Brain_Operating_System]]
13. [[13_Risk_Register]]
14. [[14_Project_Status_and_Next_Actions]]
15. [[15_Decision_Log]]
16. [[16_Implementation_Prompt_Sequence]]
17. [[17_Master_Inventory]]
18. [[18_Jules_Protected_Implementation_Workflow]]
19. [[19_Prototype_Scraper_Workflow]]

## Architecture navigation

- [[architecture/01_Acquisition_Routing_and_Session_Sync_Prototype]]
- [[architecture/02_Offline_Parser_Resilience_and_Scrapling_Validation]]
- [[architecture/03_Protected_Session_Broker_Prototype]]

## Source and experiment navigation

- [[sources/01_Boat24]]
- [[experiments/01_[Gemini]_Experiment_Log_Ledger|Experiment Log Ledger]]
- [[templates/Source Note Template]]
- [[templates/Experiment Log Template]]
- [[templates/Jules Repair Prompt Template]]

## Project rule

> ==Obsidian is the project memory and operating manual. Running code, tests, migrations, manifests and telemetry are the technical source of truth.==

## Current target sources

1. Boat24
2. Croatian Yachting
3. MarineOne / YachtBrokerage
4. Njuškalo Nautika
5. TheYachtMarket
6. iNautia

Alternatives remain documented but do not replace an approved target without a recorded decision.

## Current phase

Steps 1–2 of `scraper_project` are complete. The immediate action is Step 3: prove the isolated Docker and dual-node synthetic fixture flow.

The new acquisition, Session Broker and parser-resilience notes are prototype blueprints for later steps. They do not replace the existing approved acquisition implementation and are not production-approved until supported by experiments and accepted by Vuk.

See [[14_Project_Status_and_Next_Actions]] for the current checkpoint.
