# IATI Extractions

All data extracted from the International Aid Transparency Initiative (IATI) database and associated forecasting pipeline. This covers ~1,800 environmental and sustainability-improving activities from 4 reporting organizations (World Bank, BMZ, UK FCDO, and the Asian Development Bank, split into train/val/test sets.

## Directory Structure

### `ratings/`
Overall activity success ratings on a ~1–6 scale (IEG/DSGF ratings).
- `merged_overall_ratings.jsonl` — primary ratings file used throughout the pipeline

### `outcome_tags/`
Binary outcome tags extracted by LLM from evaluation reports.
- `applied_tags.jsonl` — final applied tags (signed, threshold-calibrated); each entry has activity_id and a dict of tag_name → binary value

### `outcome_summaries/`
LLM-generated ex-post summaries of activity outcomes.
- `outputs_summary_expost.jsonl` — free-form text summaries of what actually happened for each activity (used as context when grading LLM forecasts)

### `quantitative_outcomes/`
Quantitative outcome measurements and model predictions.
- `activity_outcomes.csv` — quantitative outcome values (CO2, beneficiaries, costs, etc.) with baseline/target/actual normalised per activity and outcome type; columns include `outcome_norm`, `baseline_norm`, `target_norm`, `outcome_norm_dollars_per_unit`, etc.
- `zagg_predictions.csv` — aggregate cost-effectiveness metric (z-score aggregated across all quantifiable outcomes per activity); columns: activity_id, outcome_col, y_true, y_pred, split
- `all_outcome_values.csv` — individual outcome values with z-scores per activity per outcome group 

### `features/`
Input features used by the GLM/RF rating prediction model.
- `all_features.csv` — all ~58 feature values for every activity (train+val+test), with split label

Features include: LLM-extracted activity properties (finance, integratedness, implementer_performance, targets, context, risks, complexity, activity_scope), country characteristics (gdp_percap, cpia_score, WGI governance indicators), region dummies, planned_duration, planned_expenditure, UMAP embeddings (umap3_x/y/z), sector_distance, country_distance, sector_cluster dummies, missingness indicators, and org dummies.

### `llm_extracted_preactivity_features/`
LLM outputs used as inputs to the GLM/RF rating prediction model .

**LLM-graded activity properties** (1–6 scale ratings extracted by LLM from evaluation docs):
- `outputs_finance_grades.jsonl` → `finance` feature
- `outputs_integratedness_grades.jsonl` → `integratedness` feature
- `outputs_implementer_performance_grades.jsonl` → `implementer_performance` feature
- `outputs_targets_grades.jsonl` → `targets` feature
- `outputs_context_grades.jsonl` → `context` feature
- `outputs_risks_grades.jsonl` → `risks` feature
- `outputs_complexity_grades.jsonl` → `complexity` feature

**UMAP embedding source** (targets/objectives text → 3D UMAP used for umap3_x/y/z features):
- `outputs_targets.jsonl` — LLM extraction of activity target objectives text
- `outputs_targets_embeddings.jsonl` — text embeddings of the targets (input to UMAP)
- `outputs_targets_context_maps.jsonl` — pairwise similarity maps (val set); used for sector_distance, country_distance
- `outputs_targets_context_maps_trainval.jsonl` — same for train+val (test set run)

**Other LLM feature sources**:
- `outputs_misc.jsonl` — miscellaneous LLM extractions (activity_scope and other fields)
- `llm_planned_expenditure.jsonl` — LLM-extracted planned expenditure → `planned_expenditure` feature
- `llm_planned_duration.jsonl` — LLM-extracted planned duration → `planned_duration` feature
- `outputs_finance_sectors_disbursements_baseline_gemini2p5flash.jsonl` — sector/finance/disbursement extraction used for `sector_cluster_*` features

### `llm_forecasts/`
LLM free-form forecast texts. Files prefixed `val` are the ~300-activity validation set; these are the forecasts plotted in the 2-pane comparison figure. The `test` file is the held-out test set.

**Validation set (2-pane plot — all methods with overlapping activity coverage):**
- `outputs_exactly_like_halawi_et_al_rag_added_gemini3pro_val_s3_call_1.jsonl` — gemini-3-pro (KNN+RAG+S1+S2)
- `outputs_exactly_like_halawi_et_al_rag_added_deepseek_minimal_val_s3_call_1.jsonl` — deepseek-v3.2 (KNN+RAG+S1+S2)
- `outputs_exactly_like_halawi_et_al_rag_added_forced_rf_deepseek_val_forced_rf_s3_call_1.jsonl` — deepseek-v3.2 RF forced (KNN+RAG+S1+S2)
- `outputs_exactly_like_halawi_et_al_rag_added_no_knn_no_rag_forced_rf_deepseek_val_no_knn_no_rag_forced_rf_s3_call_1.jsonl` — deepseek-v3.2 RF forced (no KNN, no RAG)
- `outputs_exactly_like_halawi_et_al_rag_added_deepseek_minimal_val_no_goodbadcalls_s3_call_1.jsonl` — deepseek-v3.2 (KNN+RAG, no S1/S2)
- `outputs_exactly_like_halawi_et_al_rag_added_no_knn_no_rag_deepseek_minimal_val_s3_call_1.jsonl` — deepseek-v3.2 (no KNN, no RAG, no S1, no S2)
- `outputs_onlysummary_no_knn_no_rag_onlysummary_no_knn_no_rag_s3_call_1.jsonl` — deepseek-v3.2 (minimal prompt, summary only)

**Note on test set coverage:** `outputs_exactly_like_halawi_et_al_rag_added_no_knn_no_rag_deepseek_minimal_val_s3_call_1.jsonl` contains predictions for 498 activities: ~299 val + ~199 test (it was run across both splits). This file therefore also serves as the test set deepseek-minimal predictions.

### `llm_narrative_forecast_grades/`
Grading of the LLM forecasts against actual outcomes. Contains two types of files:

**Raw grades** (`grades_*.jsonl`): output of the grading LLM per activity, includes `response_text` (reasoning + GRADE: letter), `activity_id`, and token usage.

**Combined files** (`combined_*.jsonl`):
- `activity_id`
- `method_name`
- `forecast_text` — the LLM's forecast
- `grading_reasoning` — the grader's full response (reasoning + extracted grade)
- `extracted_grade_letter` — e.g. "B+"
- `extracted_grade_numeric` — numeric score (55–97 scale)

