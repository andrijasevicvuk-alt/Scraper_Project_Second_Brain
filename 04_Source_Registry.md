# Source Registry

Every source must be defined before its scraper is treated as production-ready.

## Registry fields

- `source_name`
- `base_url`
- `source_type`
- `market_focus`
- `business_role`
- `in_scope_categories`
- `excluded_categories`
- `discovery_entry_points`
- `source_listing_key_strategy`
- `list_level_fields`
- `required_detail_fields`
- `parser_version`
- `acquisition_version`
- `reliability_score`
- `price_signal_strength`
- `year_signal_strength`
- `location_signal_strength`
- `ownership_signal_strength`
- `routine_frequency`
- `stale_refresh_window`
- `is_active`
- `status`
- `notes`

## Source status vocabulary

- `planned`
- `researching`
- `acquisition_prototype`
- `fixtures_collected`
- `parser_prototype`
- `pilot_ready`
- `genesis_ready`
- `genesis_complete`
- `routine_active`
- `paused`
- `deprecated`

## Current targets

### Boat24

Role: broad marketplace backbone.

### Croatian Yachting

Role: Croatian and Adriatic broker trust anchor.

### MarineOne / YachtBrokerage

Role: additional Croatian and Adriatic broker trust anchor.

### Njuškalo Nautika

Role: Croatian marketplace breadth. Only valuation-relevant vessel categories should be included.

### TheYachtMarket

Role: broad international and Mediterranean context.

### iNautia

Role: Mediterranean and European expansion layer.

Alternatives remain documented but do not replace the official target without a decision in [[15_Decision_Log]].

## Source note rule

Every source gets its own note created from [[templates/Source Note Template]].

Claims must be labelled:

- ==Observed== — directly seen in a test or fixture
- ==Verified== — repeated and confirmed
- ==Hypothesis== — suspected but not proven
- ==Deprecated== — previously believed but no longer current
