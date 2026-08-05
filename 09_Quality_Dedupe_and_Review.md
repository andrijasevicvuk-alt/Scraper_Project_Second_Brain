# Quality, Dedupe and Review

One number is not enough to explain whether a scraped record is useful.

## Separate quality signals

Keep separate:

- parse confidence;
- field completeness;
- source reliability;
- identity confidence;
- selector or fingerprint repair confidence;
- duplicate confidence;
- source-readiness score;
- final YPI data-quality score;
- valuation eligibility.

The scraper may calculate source-level readiness signals. YPI remains responsible for final valuation eligibility.

A single acquisition-layer Yacht Quality Score is not authoritative and must not replace these separate signals.

## Validation order

```text
raw artifact validation
→ immutable snapshot
→ field extraction
→ source-level validation
→ source-readiness signals
→ canonical mapping in YPI
→ unit and currency normalization
→ business validation
→ duplicate candidate generation
→ review queue
→ final quality and publication
```

## Critical source fields

A detail record should attempt to provide:

- source listing key;
- listing URL;
- title;
- builder;
- model;
- variant if available;
- year;
- asking price;
- currency;
- location;
- ownership signal if available;
- engine information;
- key dimensions;
- description;
- observation time.

Unknown values remain unknown with evidence and confidence. They must not be invented.

## Duplicate handling

Composite fingerprints and similarity may generate candidates, but candidate generation is not merging.

Ambiguous clusters go to review.

## Parser fallback and repair rule

Extraction proceeds strict-first:

1. structured data or embedded state;
2. semantic locators;
3. controlled regex or text fallback;
4. adaptive selector/fingerprint candidate.

Adaptive candidates never modify the active production parser in place.

Promotion follows [[architecture/02_Offline_Parser_Resilience_and_Scrapling_Validation]]:

- high confidence: automatic promotion to a new immutable version after all hard gates and shadow tests pass;
- medium confidence: quarantine and broader testing;
- low confidence: human review.

Every fallback records evidence, method, warnings, parser version and selector/fingerprint-set version.
