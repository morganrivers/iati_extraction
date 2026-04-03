# outcome_summaries

LLM-generated summaries of activity outcomes, written from the ex-post (post-completion) perspective.

## Files

### `outputs_summary_expost.jsonl` (1,800 rows)
One record per activity. The LLM was given the full evaluation document and asked to summarise what actually happened — outcomes achieved, shortfalls, and key factors. These summaries are used as retrieved context (RAG) when forecasting other activities.

**Fields:**
- `activity_id` — IATI activity identifier
- `prompt_type` — which prompt template was used
- `token_usage` — token count for this call
- `model_version` — LLM used
- `response_text` — free-text summary of the actual outcomes
