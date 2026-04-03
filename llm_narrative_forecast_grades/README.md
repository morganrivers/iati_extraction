# llm_grades

LLM-assigned grades evaluating how accurate each forecast was, relative to the known outcome. Two families of files exist here.

---

## `grades_*` files (raw grading responses, ~290–480 rows)

These correspond 1-to-1 with files in `llm_forecasts/` — the same naming conventions apply. Each record is the grading LLM's assessment of one forecast.

**Fields:**
- `activity_id` — IATI activity identifier
- `prompt_type` — grading prompt variant used
- `token_usage` — token count for the grading call
- `model_version` — LLM used for grading
- `response_text` — raw grading output (includes reasoning and a letter grade such as `A`, `B+`, `C-`)

---

## `combined_*` files (parsed grades, ~299 rows each)

Processed versions of the raw grades with extracted numeric scores. Named by model and method combination.

**Fields:**
- `activity_id` — IATI activity identifier
- `method_name` — human-readable description of the forecasting method (e.g. `"deepseek-v3.2 (KNN+RAG+S1+S2)"`)
- `forecast_text` — the full forecast reasoning text from `llm_forecasts/`
- `grading_reasoning` — the grading LLM's explanation of the score
- `extracted_grade_letter` — letter grade parsed from `response_text` (e.g. `"B+"`, or `None` if parsing failed)
- `extracted_grade_numeric` — numeric equivalent of the letter grade (0–100 scale)

### Files
- `combined_deepseek_v3_2__KNN_RAG_S1_S2_.jsonl` — main DeepSeek run, full pipeline (KNN + RAG + stages 1 & 2 summaries)
- `combined_deepseek_v3_2___KNN_and_RAG_added__no_S1__no_S2_.jsonl` — KNN + RAG but without S1/S2 summaries
- `combined_deepseek_v3_2__minimal_prompt_.jsonl` — minimal prompt variant
- `combined_deepseek_v3_2__no_KNN__no_RAG__no_S1__no_S2_.jsonl` — no retrieval, no summaries (ablation baseline; ~498 rows as it covers val + test)
- `combined_deepseek_v3_2_RF_forced__KNN_RAG_S1_S2_.jsonl` — DeepSeek with forced rating format, full pipeline
- `combined_deepseek_v3_2_RF_forced__no_KNN__no_RAG__no_S1__no_S2_.jsonl` — forced format, no retrieval
- `combined_gemini_3_pro__KNN_RAG_S1_S2_.jsonl` — Gemini 1.5 Pro, full pipeline (~476 rows)
