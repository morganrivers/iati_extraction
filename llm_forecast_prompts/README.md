# forecast_prompts

The exact prompts (system message + user message) sent to the LLM for each forecast in `llm_forecasts/`. One file per forecasting run, named to match the corresponding `outputs_*` file.

All files are JSONL with one record per activity.

## Common fields
- `activity_id` — IATI activity identifier
- `system_msg` — system prompt given to the LLM
- `prompt` — full user-facing prompt (includes activity description, KNN examples and/or RAG context if applicable, and the forecasting instruction)
- `prompt_type` — label identifying which prompt template was used

---

## Files

The file naming conventions match those in `llm_forecasts/` — see that folder's README for the key.

### DeepSeek (stage 3 / final forecast, 299 rows each)
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_deepseek_minimal_val_s3_call_1.jsonl`
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_deepseek_minimal_val_no_goodbadcalls_s3_call_1.jsonl`
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_forced_rf_deepseek_val_forced_rf_s3_call_1.jsonl`
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_no_knn_no_rag_forced_rf_deepseek_val_no_knn_no_rag_forced_rf_s3_call_1.jsonl`
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_no_knn_no_rag_deepseek_minimal_val_s3_call_1.jsonl` (199 rows — subset)

### Deepseek multi-stage prompts (all were generated using Deepseek v3.2 pro, although the final forecast for these was gemini3pro)
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_gemini3pro_val_s1_call_1.jsonl` (199 rows) — stage 1: reasons the activity may go well
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_gemini3pro_val_s2_call_1.jsonl` (299 rows) — stage 2: reasons it may go badly
- `dryrun_prompts_exactly_like_halawi_et_al_rag_added_gemini3pro_val_s3_call_1.jsonl` (299 rows) — stage 3: final forecast using s1+s2 as context

### Summary-only ablation (test-set only rows, unfortunately original validation prompts were lost but can be recovered - contact me if these are needed)
- `dryrun_prompts_onlysummary_no_knn_no_rag_onlysummary_no_knn_no_rag_s3_call_1.jsonl`
