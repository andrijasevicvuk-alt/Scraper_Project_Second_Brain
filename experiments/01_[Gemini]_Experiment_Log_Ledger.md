---
status: canonical
owner: Gemini / Vuk
implementation_support: Jules / Codex
last_updated: 2026-08-05
---

# Experiment Log Ledger

## Purpose

This directory is the empirical source of truth for acquisition, session, parser-resilience, proxy and performance claims.

A prototype decision may be implemented before it is proven. It must not be presented as experiment-supported or production-approved without a reproducible record.

## Maturity transitions

```text
hypothesis
→ prototype_decision
→ experiment_supported
→ production_approved
```

- `hypothesis`: untested idea or source claim.
- `prototype_decision`: selected so a complete prototype can be implemented and tested.
- `experiment_supported`: controlled testing supports the claim within recorded conditions.
- `production_approved`: Vuk accepts a version for routine use after it meets defined criteria.

A failed experiment may return a design to `hypothesis`, replace the prototype decision or deprecate it.

## Golden rules

- Every experiment has an ID, date, owner, software versions, sample, command, evidence and result.
- Exact counts, request rates, bandwidth reductions, session lifetimes and accuracy claims require evidence.
- Experiments never expose credentials or cookies.
- Production components are versioned and reproducible.
- Runtime adaptive state cannot silently become production state.
- An experiment supports only the conditions actually tested.

## Current experiment families

### Acquisition-route experiments

- existing custom implementation baseline;
- fast HTTP compatibility and throughput;
- browser-mediated acquisition;
- Scrapling fetcher comparison;
- source/page routing decision.

### Session experiments

- browser-to-client Session Sync compatibility;
- identity bundle lifetime;
- broker generation and refresh lease;
- thundering-herd prevention;
- worker-crash lease recovery.

### Proxy and asset experiments

- list/detail bytes;
- browser-bootstrap bytes;
- source-specific asset blocking;
- correctness after blocking;
- cost per accepted record.

### Parser-resilience experiments

- strict structured extraction baseline;
- semantic locator stability;
- regex fallback accuracy;
- controlled DOM mutation;
- adaptive candidate accuracy;
- high/medium/low validation calibration;
- shadow-mode regression testing.

## Promotion record

Every maturity change must record:

- previous and proposed maturity;
- experiment IDs and evidence;
- acceptance criteria;
- known limitations;
- version being promoted;
- rollback version;
- whether Vuk approval is required and received.

Use [[templates/Experiment Log Template]] for each experiment.
