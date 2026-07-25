## Purpose

This note records every important repository, tool, service, dependency, machine, dataset and project responsibility used by `scraper_project`.

Inventory statuses:

- `approved`
- `active`
- `candidate`
- `experimental`
- `future`
- `deprecated`

## Repositories and documentation

|Item|Role|Status|Source of truth|
|---|---|---|---|
|Scraper Project Second Brain|Project memory and operating manual|active|Private GitHub repository|
|`scraper_project`|Source acquisition and source-readiness platform|next action|Future private GitHub repository|
|YPI repository|Normalization, valuation-ready data, scoring and UI|active|YPI GitHub repository|

## AI and agent tools

|   |   |   |   |
|---|---|---|---|
|Tool|Responsibility|Status|Boundary|
|ChatGPT|Architecture, integration, reviews, prompts and Obsidian updates|active|Does not replace protected acquisition|
|Codex|Source-neutral platform, offline parsers, tests and exports|active|Cannot edit protected acquisition paths|
|Gemini|Source-specific acquisition design|active|Does not own YPI business logic|
|Antigravity|Protected adapter implementation and controlled execution|active|Edits only approved protected paths|

## Machines

|   |   |   |
|---|---|---|
|Machine|Role|Status|
|ThinkPad laptop|Control node, development, GitHub, Codex and monitoring|active|
|Home PC|Docker worker, acquisition jobs, snapshots and runtime database|planned|

## Core infrastructure

|   |   |   |   |
|---|---|---|---|
|Tool|Role|Status|Notes|
|GitHub|Code, documentation, migrations and reviews|approved|No secrets or runtime data|
|Obsidian|Human-readable second brain|approved|Not the technical source of truth|
|Docker Compose|Local container orchestration|approved|Kubernetes is not required|
|SQLite|Local jobs, checkpoints and runtime ledger|approved|WAL mode, one writer|
|PostgreSQL/Supabase|YPI structured data layer|separate YPI responsibility|Do not store all raw HTML in PostgreSQL|

## Acquisition tools

|   |   |   |   |
|---|---|---|---|
|Tool|Role|Status|Notes|
|Existing custom acquisition engine|Protected acquisition implementation|approved|Must not be replaced|
|Scrapling|Optional isolated acquisition adapter|experimental|Must implement shared contracts|
|Crawlee|Possible orchestration/acquisition component|candidate|Must not duplicate Scrapling retries|
|DataImpulse residential proxies|Residential proxy traffic|active|Record bytes per source and run|
|Stealth-browser runtime|Protected browser acquisition|active design|Gemini/Antigravity-owned|

## Data and pipeline tools

|   |   |   |
|---|---|---|
|Tool|Role|Status|
|Python|Main scraper and pipeline language|approved|
|Pydantic v2|Contract and payload validation|approved|
|RapidFuzz|Duplicate-candidate and similarity signals|approved|
|Selectolax or BeautifulSoup|Offline HTML parsing|candidate|
|pytest|Unit, fixture and integration testing|recommended|
|Ruff|Formatting and linting|recommended|
|mypy|Static type checking|candidate|
|Typer|CLI implementation|candidate|
|SQLAlchemy/Alembic|Database access and migrations|candidate; Codex should justify|

Candidate tools are not automatically approved dependencies.

## Target sources

|   |   |   |
|---|---|---|
|Source|Business role|Current state|
|Boat24|Main marketplace backbone|first vertical slice|
|Croatian Yachting|Croatian/Jadratic broker anchor|planned|
|MarineOne / YachtBrokerage|Additional local broker anchor|planned|
|Njuškalo Nautika|Croatian marketplace coverage|planned|
|TheYachtMarket|Broad international coverage|planned|
|iNautia|Mediterranean expansion|planned|
|Band of Boats / YachtFocus|iNautia alternatives|optional|
|Burza Nautike / Index Oglasi / Mornar.net|Njuškalo alternatives|optional|

Alternatives do not automatically replace primary targets.

## Runtime storage

|   |   |   |
|---|---|---|
|Data|Location|Git status|
|Source code|GitHub and local clone|committed|
|Raw snapshots|Home-PC worker storage|ignored|
|Runtime SQLite database|Home-PC persistent volume|ignored|
|Checkpoints|Home-PC persistent volume|ignored|
|Logs|Home-PC runtime storage|ignored|
|Dataset exports|Worker storage and approved backup|ignored|
|Parser fixtures|Repository `tests/fixtures/`|committed|
|Secrets|Runtime environment only|never committed|

## Proxy and cost targets

|   |   |
|---|---|
|Phase|Starting target|
|Controlled pilots|Existing 5 GB balance|
|Genesis scrape|Measure first; expected approximately 25–40 GB for 50,000 listings|
|Routine operation|Target 5 GB per month or less|
|Upgrade rule|Increase only after measured usage shows 5 GB is insufficient|

## Protected paths

```
src/acquisition/protected/**
src/acquisition/custom_adapter/**
src/acquisition/protected_adapters/**
docker/protected/**
config/protected/**
tests/protected_acquisition/**
```

## Current deliverables

- Create private `scraper_project` repository
    
- Add the five initial boundary files
    
- Make initial manual commit
    
- Connect repository to Codex
    
- Send Step 1 from Note 16
    
- Review Codex file plan before implementation
    
- Verify tests and protected-path compliance
    

## Inventory maintenance rules

Update this note when:

- a dependency is added or removed;
- a tool changes status;
- a source changes priority;
- a repository is created;
- a machine role changes;
- proxy or storage assumptions change;
- an acquisition implementation is approved or deprecated.

Every inventory item should record:

- role;
- owner;
- status;
- version where important;
- location;
- source of truth;
- cost where relevant;
- whether it handles secrets;
- replacement policy.