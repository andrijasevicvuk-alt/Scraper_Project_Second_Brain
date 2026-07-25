# Scraper Project — Second Brain

This folder is the control center for my `scraper_project`, a side project that collects, stores, cleans and maintains boat listing data for [[YPI]].

The final goal is not just to make a scraper that works one time. The goal is to create a ==stable data acquisition system== that can:

- build the first complete dataset through a [[05_Genesis_Scrape|Genesis Scrape]]
- maintain the dataset through a [[06_Routine_Scrape|Routine Scrape]]
- preserve raw snapshots and source trace
- pass clean records into the YPI `raw → normalized → valuation-ready` pipeline
- keep Gemini and Antigravity work isolated from Codex-owned modules

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
11. [[11_Gemini_Antigravity_Handoff]]
12. [[12_Second_Brain_Operating_System]]
13. [[13_Risk_Register]]
14. [[14_Project_Status_and_Next_Actions]]
15. [[15_Decision_Log]]
16. [[16_Implementation_Prompt_Sequence]]
17. [[17_Master_Inventory]]
18. [[18_Repository_Starter_Files]]
## Project rule

> ==Obsidian is the project memory and operating manual. GitHub and the databases are the technical source of truth.==

## Current target sources

- TheYachtMarket
- Boat24
- MarineOne / YachtBrokerage
- Croatian Yachting
- iNautia
  - alternatives: Band of Boats, YachtFocus
- Njuškalo Nautika
  - alternatives: Burza Nautike, Index Oglasi/Nautika, Mornar.net

## Current phase

The source-neutral architecture and YPI data foundation are being prepared before full Genesis scraping starts.

See [[14_Project_Status_and_Next_Actions]] for the current checkpoint.
