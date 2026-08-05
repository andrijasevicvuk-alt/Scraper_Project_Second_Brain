---
status: prototype_decision
architecture_owner: ChatGPT
source_evidence_owner: Gemini
implementation_owner: Codex
layer: offline_parser_and_quality
production_status: not_approved
last_updated: 2026-08-05
---

# Offline Parser Resilience and Scrapling Validation

## Purpose

Preserve Gemini's useful DOM-resilience and adaptive-repair ideas while keeping all extraction offline, versioned and reproducible.

This design consumes saved immutable snapshots. It performs no network requests and is not part of Jules' protected acquisition adapter.

## Extraction hierarchy

1. **Structured data and embedded state** — JSON-LD, documented embedded JSON or other stable source state.
2. **Semantic locators** — labels, headings, table relationships and stable document structure.
3. **Controlled text or regex fallback** — source-specific patterns with type/range checks, evidence and warnings.
4. **Adaptive repair candidate** — Scrapling or another approved tool proposes a selector/fingerprint replacement when the active version fails.

A fallback does not overwrite the active parser version.

## Candidate lifecycle

```text
active immutable parser + selector/fingerprint version
        ↓ failure or layout change
candidate repair generated in quarantine
        ↓ validation evidence
high / medium / low confidence decision
        ↓
new version promoted / more tests / human review
```

Runtime `auto_save` may write only to candidate quarantine. Production reads only an explicitly promoted immutable version.

## Validation signals

Every candidate repair is evaluated using:

- element and structural similarity to the previous target;
- expected field type;
- valid field value ranges;
- surrounding labels, headings and context;
- selector uniqueness within the page;
- comparison with previous values for the same listing when available;
- consistency across multiple pages and archived snapshots;
- agreement with JSON-LD, embedded JSON or another independent extractor;
- shadow-mode results against the active parser;
- false-positive and regression checks on unrelated fields.

Hard validation failures, such as invalid type/range or nonunique selection, prevent automatic promotion regardless of the aggregate score.

## Confidence outcomes

### High confidence

A candidate may be automatically promoted when:

- all hard gates pass;
- it meets the configured high-confidence threshold;
- it is consistent across the required sample;
- an independent extractor agrees where available;
- shadow mode shows no unacceptable regressions.

Promotion creates a new immutable selector/fingerprint-set version and parser version. The previous version remains available for rollback.

### Medium confidence

The candidate is quarantined and tested across additional pages, snapshots, edge cases and time periods. It does not affect production until it reaches the high-confidence gate or receives an explicit human decision.

### Low confidence

The candidate requires human review. Examples include conflicting extractors, implausible values, ambiguous context, low uniqueness or cross-page inconsistency.

## Version and trace requirements

Every promoted repair records:

- parser version;
- selector/fingerprint-set version;
- source and field;
- parent version;
- generation method;
- validation score and hard-gate results;
- test fixture set;
- shadow-mode report;
- promotion timestamp and policy version;
- maturity transition;
- rollback version.

## Responsibility boundary

- Gemini provides source DOM observations, failure examples and experiment proposals.
- ChatGPT reviews parser architecture and validation policy.
- Codex implements the offline parser, candidate quarantine, validation, versioning and promotion machinery.
- Vuk approves validation policy and production criteria.
- Jules does not implement this parser-resilience flow inside protected acquisition.

## Experiment sequence

1. Baseline extraction from approved archived fixtures.
2. Controlled HTML mutation experiment.
3. Candidate generation without promotion.
4. Validation-signal measurement.
5. Shadow-mode comparison with the active parser.
6. Calibrate high/medium/low thresholds from evidence.
7. Approve or reject production promotion policy.

AgentQL, Stagehand or another AI tool may propose a repair only as an experimental candidate. It never directly edits or promotes production code.
