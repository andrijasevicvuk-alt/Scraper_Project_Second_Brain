# Quality, Dedupe and Review

One number is not enough to explain whether a scraped record is useful.

## Separate quality signals

Keep:

- parse confidence
- field completeness
- source reliability
- identity confidence
- duplicate confidence
- source-readiness score
- final YPI data-quality score
- valuation eligibility

The scraper can calculate a source-readiness score. YPI remains responsible for final valuation eligibility.

## Validation order

```text
ingestion validation
→ field extraction
→ source-level validation
→ canonical mapping in YPI
→ unit and currency normalization
→ business validation
→ duplicate candidate generation
→ review queue
→ final quality and publication
```

## Critical source fields

A detail record should attempt to provide:

- source listing key
- listing URL
- title
- builder
- model
- variant if available
- year
- asking price
- currency
- location
- ownership signal if available
- engine information
- key dimensions
- description
- observation time

Unknown values should remain unknown with evidence and confidence. They must not be invented.

## Duplicate handling

Composite fingerprints and RapidFuzz-style similarity may generate candidates, but candidate generation is not the same as merging.

Cross-source merge evidence can include:

- normalized builder and model
- year
- length
- location proximity
- price proximity
- engine signature
- title and description similarity
- image hash later
- first-seen and last-seen patterns

Ambiguous clusters go to review.

## Review queue examples

- unclear builder/model
- unclear year
- unclear ownership status
- suspicious price
- weak location extraction
- conflicting engine values
- duplicate candidate requiring confirmation
- low-confidence parser fallback
- unexpected source layout

## Parser fallback rule

- strict extraction success → higher confidence
- fuzzy or adaptive fallback → lower confidence and visible warning
- no reliable result → `None` and review/failure reason
