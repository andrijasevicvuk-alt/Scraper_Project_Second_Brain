# Scraper Project — Second Brain

This vault is the human-readable control center for `scraper_project`, the source-acquisition and source-readiness subsystem used by [[YPI]].

The scraper project exists to build and maintain trustworthy source-level boat-listing data, preserve raw evidence and operational trace, and hand controlled batches to YPI without mixing scraping logic with valuation logic.

> ==Running code, tests, migrations, manifests and runtime evidence are stronger than documentation. The Second Brain is the long-term project memory and operating manual and must be corrected when implementation evidence proves it stale.==

## Start here in a new session

1. [[20_Session_Start]]
2. [[14_Project_Status_and_Next_Actions]]
3. [[02_Project_Boundaries_and_AI_Roles]]
4. [[07_Data_Flow_and_Contracts]]
5. [[15_Decision_Log]]
6. the relevant source note under `sources/`
7. only then the detailed [[16_Implementation_Prompt_Sequence]] if implementation-step detail is required

This reading order is intentionally short. A new ChatGPT, Codex, Gemini or Jules session should not need to read the full implementation playbook merely to discover the current state.

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
20. [[20_Session_Start]]
21. [[21_Remote_Worker_Runbook]]
22. [[YPI]]

## Architecture navigation

- [[architecture/01_Acquisition_Routing_and_Session_Sync_Prototype]]
- [[architecture/02_Offline_Parser_Resilience_and_Scrapling_Validation]]
- [[architecture/03_Protected_Session_Broker_Prototype]]

These architecture notes are prototype designs unless their maturity explicitly says otherwise. They are not proof that the component is implemented or production-approved.

## Source and experiment navigation

- [[sources/01_Boat24]]
- [[experiments/01_[Gemini]_Experiment_Log_Ledger|Experiment Log Ledger]]
- [[templates/Source Note Template]]
- [[templates/Experiment Log Template]]
- [[templates/Jules Repair Prompt Template]]
- [[templates/Obsidian Update Checklist]]

## Current target sources

1. Boat24
2. Croatian Yachting
3. MarineOne / YachtBrokerage
4. Njuškalo Nautika
5. TheYachtMarket
6. iNautia

Alternatives remain documented but do not replace an approved target without a recorded decision.

## Current phase — checkpoint 2026-09-28

`scraper_project` Steps 1, 2 and 3 are complete.

The separate remote-reliability acceptance work performed after Step 3 passed at the 2026-09-20 checkpoint. The Home PC's **current live power/network/service state is unknown when it has not been checked from college**; do not turn a previous successful acceptance test into a claim that the machine is online now.

The active scraper-development step is:

> **scraper_project Step 4 — finalize and approve the Boat24 source specification.**

This is not the same numbering as YPI's own historical Steps 3–5. Always prefix a step with the repository/project name when ambiguity is possible. See [[15_Decision_Log]] and [[20_Session_Start]].

No production Boat24 adapter, Boat24 offline parser, live pilot, Genesis run or routine scheduler exists in `scraper_project` yet.

See [[14_Project_Status_and_Next_Actions]] for the detailed checkpoint and roadmap.
