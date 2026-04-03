# ratings

Ground-truth overall outcome ratings extracted from activity evaluation documents.

## Files

### `merged_overall_ratings.jsonl` (2,109 rows)
One record per activity. The overall outcome rating was extracted by an LLM reading the evaluation report and identifying the headline rating. This is the primary prediction target for the forecasting model.

**Fields:**
- `activity_id` — IATI activity identifier
- `token_usage` — token count for the extraction call
- `model_version` — LLM used
- `response_text` — JSON string with the extracted rating. Structure:
  ```json
  {
    "description": "Overall Outcome Rating",
    "rating_value": "Moderately Satisfactory",
    "min": "Highly Unsatisfactory",
    "max": "Highly Satisfactory"
  }
  ```
  `rating_value` is the raw text label from the evaluation document. Labels are mapped to a numeric scale (~1–6) before use in the model: Highly Unsatisfactory = 1, Unsatisfactory = 2, Moderately Unsatisfactory = 3, Moderately Satisfactory = 4, Satisfactory = 5, Highly Satisfactory = 6.

Take a look at activity features csv to see what the final rating applied was (converted from 1-6 to 0-5, and flipped for BMZ as the rating is reversed there).