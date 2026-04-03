# features

Pre-computed feature matrix combining all inputs used by the forecasting model.

## Files

### `all_features.csv` (1,181 rows)
One row per activity. Contains every feature used in the GLM/prediction models, assembled from the other folders. Activities here are the rated subset (those with `merged_overall_ratings`).

**Key columns:**
- `activity_id` — IATI activity identifier (e.g. `44000-P111760`)
- `split` — `train`, `val`, or `test`
- `rating` — ground-truth overall rating (numeric, ~1–6 scale)
- `finance`, `integratedness`, `implementer_performance`, `targets`, `context`, `risks`, `complexity` — LLM-extracted rubric scores (numeric, parsed from `llm_features/outputs_*_grades.jsonl`)
- `activity_scope` — numeric activity scope code from IATI
- `gdp_percap` — GDP per capita of recipient country at activity start
- `cpia_score` — World Bank Country Policy and Institutional Assessment score
- `region_*` — one-hot dummies for World Bank region (AFE, AFW, EAP, ECA, LAC, MENA, SAS)
- `finance_is_loan` — binary: 1 if primary finance type is a loan
- `planned_duration` — planned activity duration in years (from `llm_features/llm_planned_duration.jsonl`)
- `planned_expenditure` — planned total expenditure in USD (from `llm_features/llm_planned_expenditure.jsonl`)
- `rep_org_0/1/2/3` — one-hot dummies for reporting organisation
- `umap3_x/y/z` — 3D UMAP embedding coordinates (from `llm_features/outputs_targets_context_maps_trainval.jsonl`)
- `sector_distance`, `country_distance` — distance in embedding space to nearest same-sector / same-country activities
- `sector_cluster_*` — one-hot dummies for thematic sector cluster
- `governance_composite`, `log_planned_expenditure`, `expenditure_per_year_log`, `expenditure_x_complexity` — derived features
- `*_missing` / `*_missing_count` / `*_present_ratio` — missingness indicators for LLM and covariate features
