# llm_forecasts

Raw LLM forecast texts — the model's step-by-step reasoning and final predicted outcome rating for each activity, before grading. Files cover the validation set (~299 activities) unless otherwise noted.

The exact prompts sent to the LLM for each file are in `../forecast_prompts/`.

All files are JSONL with one record per activity.

## Common fields
- `activity_id` — IATI activity identifier
- `section` — document section the forecast was generated from (typically `"Outcome"`)
- `response` — dict with the LLM's forecast content (includes the reasoning chain and final `FORECAST: <rating>` statement)

---

## File naming conventions

Files follow a consistent naming pattern encoding the forecasting method used:

| Component in name | Meaning |
|---|---|
| `halawi_et_al` | Prompt structure adapted from Halawi et al. (2024) superforecasting paper |
| `rag_added` | Retrieval-augmented generation: similar past activities retrieved and shown |
| `knn` / `no_knn` | K-nearest-neighbour examples included / excluded |
| `no_rag` | RAG context excluded |
| `deepseek_minimal` | DeepSeek V3 model, minimal prompt variant |
| `gemini3pro` | Gemini 3 Pro model |
| `forced_rf` | Model forced to give a rating that matches the random forest prediction |
| `no_goodbadcalls` | No stage 1 (least reasons it may go well) or stage 2 (least reasons it may go badly) context |
| `onlysummary` | Prompt includes only the activity summary, the statistical base rates, and no other context|

---

## Files

### Deepseek forecasts (deepseek, validation set only, 299 rows each)

Note: the "validation set" shifted somewhat, so the overlap is not perfect between the statistical forecast validation set and the LLM validation set. This makes it more difficult to directly compare model performance to the statistical performance. Regardless, the statistical models are always more skilled forecasters by all metrics when applied to the validation set used by the LLMs.

- `outputs_exactly_like_halawi_et_al_rag_added_deepseek_minimal_val_s3_call_1.jsonl` — main DeepSeek run with KNN + RAG
- `outputs_exactly_like_halawi_et_al_rag_added_deepseek_minimal_val_no_goodbadcalls_s3_call_1.jsonl` — same but KNN examples presented without s1 and s2
- `outputs_exactly_like_halawi_et_al_rag_added_forced_rf_deepseek_val_forced_rf_s3_call_1.jsonl` — DeepSeek with random forest forced prediction
- `outputs_exactly_like_halawi_et_al_rag_added_no_knn_no_rag_deepseek_minimal_val_s3_call_1.jsonl` — DeepSeek with no KNN and no RAG (ablation)
- `outputs_exactly_like_halawi_et_al_rag_added_no_knn_no_rag_forced_rf_deepseek_val_no_knn_no_rag_forced_rf_s3_call_1.jsonl` — no KNN/RAG + forced rating format

### LLM prompt run on held-out set (~500 rows)
- `outputs_onlysummary_no_knn_no_rag_onlysummary_no_knn_no_rag_s3_call_1.jsonl` — prompt contains only the LLM-generated activity summary, no retrieved examples, no full document. This produced the best forecasts, but for the wrong reasons.

