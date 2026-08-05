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
- `prototype_version`
- `maturity_status`
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

## Source delivery status

- `planned`
- `researching`
- `source_specification`
- `prototype_blueprint`
- `prototype_implemented`
- `fixtures_collected`
- `parser_prototype`
- `pilot_ready`
- `genesis_ready`
- `genesis_complete`
- `routine_active`
- `paused`
- `deprecated`

## Design and evidence maturity

Use the maturity vocabulary defined in [[19_Prototype_Scraper_Workflow]]:

- `hypothesis`
- `prototype_decision`
- `experiment_supported`
- `production_approved`

A source can have a working prototype while still containing hypotheses. Production approval applies to a versioned source adapter and parser configuration, not to an entire website forever.

## Current targets

1. Boat24 — broad marketplace backbone.
2. Croatian Yachting — Croatian and Adriatic broker trust anchor.
3. MarineOne / YachtBrokerage — additional Croatian and Adriatic broker trust anchor.
4. Njuškalo Nautika — Croatian marketplace breadth for valuation-relevant categories.
5. TheYachtMarket — broad international and Mediterranean context.
6. iNautia — Mediterranean and European expansion layer.

Alternatives remain documented but do not replace an official target without a decision in [[15_Decision_Log]].

## Source note rule

Every source gets its own note created from [[templates/Source Note Template]].

A source note must separate:

- source facts and their evidence;
- hypotheses;
- prototype decisions selected for implementation;
- experiment-supported behaviour;
- production-approved versions;
- Gemini blueprint ownership;
- Jules protected implementation ownership;
- Codex parser and source-neutral integration ownership;
- linked experiment records and pilot reports.
