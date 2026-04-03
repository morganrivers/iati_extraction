# quantitative_outcomes

Extracted and modelled quantitative outcome values for activities where numeric results were reported (e.g. CO2 reductions, beneficiaries reached, area reforested).

## Files

### `activity_outcomes.csv` (1,885 rows)
One row per (activity, outcome type) pair. Contains the raw and normalised quantitative outcome values extracted from evaluation documents, along with cost-effectiveness metrics.

**Key columns:**
- `activity_id` — IATI activity identifier
- `which_distribution` — the type of outcome measured. Values include:
  `co2_emission_reductions`, `beneficiaries`, `area_protected`, `area_reforested`, `trees_planted`, `water_connections`, `generation_capacity`, `economic_rate_of_return`, `financial_rate_of_return`, `benefit_cost_ratios`, `stoves`, `pollution_load_removed`, `yield_increases_percent`, `yield_increases_t_per_ha`, `area_under_management`
- `which_units` — unit of measurement (e.g. `tonnes_co2e`, `people`, `hectares`)
- `log_plot` — whether values are plotted on a log scale (bool as string)
- `per_dollar_ok` — whether cost-effectiveness (per USD) is a meaningful metric for this outcome type
- `has_exp` — whether actual expenditure data is available for this activity
- `actual_total_expenditure` — total actual expenditure in USD
- `outcome_norm` / `baseline_norm` / `target_norm` — actual, baseline, and target values normalised
- `outcome_norm_log10` etc. — log10 of the normalised values
- `outcome_norm_dollars_per_unit` etc. — cost per unit of outcome (USD / normalised unit)
- `outcome_over_target` — ratio of actual outcome to stated target (>1 means target exceeded)

### `all_outcome_values.csv` (1,221 rows)
Aggregated outcome metrics with model predictions, one row per (activity, outcome group) pair.

**Key columns:**
- `activity_id` — IATI activity identifier
- `which_group` — outcome group (e.g. `benefit_cost_ratios__ratio`, combining distribution and unit)
- `y_raw` — raw observed outcome value
- `y_z` — z-score of the observed value within its distribution
- `pred_z` — model-predicted z-score

### `zagg_predictions.csv` (564 rows)
Model predictions for aggregate outcome scores, one row per (activity, outcome column) pair covering activities with known outcomes.

**Key columns:**
- `activity_id` — IATI activity identifier
- `outcome_col` — which aggregate metric is being predicted (e.g. `outcomes_z_activity_mean`)
- `y_true` — observed aggregate outcome z-score
- `y_pred` — model prediction
- `split` — `train`, `val`, or `test`
