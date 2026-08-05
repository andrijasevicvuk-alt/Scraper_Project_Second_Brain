# Jules Protected Repair Prompt Template

## Context

You are repairing an existing protected acquisition implementation. Preserve all unaffected Jules-authored and previously approved protected behaviour.

## Problem

- observed defect:
- why it matters:
- evidence or reproduction:
- violated contract, blueprint or boundary:

## Affected component

- repository path:
- class/function/configuration:
- acquisition version:

## Required behaviour

Describe the exact corrected outcome without redesigning unrelated source strategy.

## Allowed changes

List exact protected files Jules may modify.

## Prohibited changes

- no source-neutral contracts;
- no Codex queue, migrations or parser code;
- no YPI code;
- no unrelated refactor;
- no secrets;
- no commit, push, pull request or merge.

## Tests and acceptance criteria

- test to reproduce old failure:
- test for corrected behaviour:
- contract validation:
- regression tests:
- telemetry or fixture evidence:

## Required delivery

- complete replacement files or unified patch;
- changed-file list;
- executed tests and results;
- unexecuted tests clearly identified;
- acquisition-version change if behaviour changes;
- known limitations and unresolved questions.
