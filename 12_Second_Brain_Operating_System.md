# Second Brain Operating System

A second brain makes sense for the scraper project ==now==, but it should start simple.

## What it should do now

Obsidian should help me:

- remember architecture decisions
- keep agent boundaries clear
- track source status
- store experiment results
- keep prompts and handoffs
- record proxy/storage assumptions
- maintain runbooks and recovery steps
- prevent repeated mistakes
- know the exact next action

## What it should not do yet

Do not build a complex AI API, vector database or autonomous agent memory layer before the scraper and YPI core are stable.

That would create another system to debug before the main project works.

## Stage A — now

Use Obsidian as a human-readable control plane:

- canonical project notes
- templates
- decision log
- source dossiers
- experiment logs
- project status
- prompts
- runbooks

Use simple frontmatter and stable filenames so the vault can be indexed later.

## Stage B — after the scraper foundation and YPI core work

Add machine-assisted retrieval only when:

- canonical notes are stable
- duplicate/outdated notes are separated
- the repository contracts are stable
- YPI has a working dataset and valuation flow
- there is a real repeated workflow that retrieval will improve

Later options may include:

- indexed canonical Obsidian notes
- retrieval for Codex or AI assistants
- automatic project-status summaries
- source-health summaries
- prompt generation from current decisions

## Source-of-truth hierarchy

1. running code and tests
2. migrations and database schema
3. dataset manifests and telemetry
4. canonical Obsidian notes
5. archived notes and old conversations

Obsidian controls the project, but it does not override verified repository evidence.

## Note-status convention

Use frontmatter or a visible field:

```yaml
status: canonical
owner: vuk
last_verified: 2026-07-23
related_repo: scraper_project
```

Possible statuses:

- `canonical`
- `draft`
- `experiment`
- `superseded`
- `archive`

## Session rule for ChatGPT

For project prompts, ChatGPT should:

- review relevant available previous project context
- preserve the Gemini/Antigravity protected zone
- not replace architecture silently
- distinguish facts from assumptions
- finish every answer with an `Obsidian changes` section containing:
  - Add
  - Replace
  - Remove
  - Keep unchanged

If an older conversation is not available, ChatGPT must say so rather than inventing its content.
