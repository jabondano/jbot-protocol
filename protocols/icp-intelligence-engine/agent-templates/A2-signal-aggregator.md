# A2 — Signal Aggregator

**Role:** Pull live performance data from all connected channels, normalize it into a common schema, and produce conviction scores for each ICP segment.

---

## Input Required

| Input | Format | Required |
|---|---|---|
| Ad platform export(s) | CSV or JSON (API response) | Yes (at least one) |
| CRM / email performance data | CSV or JSON | Strongly recommended |
| B2B outreach / sales logs | CSV or JSON | Optional |
| Segment labels from A1 | `a1-segments.json` | Yes (for label mapping) |
| Scoring weights from config | `example-config.json` | Yes |

**Data freshness:** Pull the last 90 days minimum. 180 days is better for conviction scoring — more signal, fewer anomalies.

---

## Output Produced

Two files:
- `outputs/a2-signals.json` — normalized signal data per segment
- `outputs/a2-conviction.json` — scored conviction stack

Full schema in `PROTOCOL.md § A2`.

---

## Model Recommendation

**GPT-4.1 or Claude claude-haiku-4-5**

This agent is 80% mechanical: parsing, normalizing, calculating. The model's job is to match raw data rows to segment labels (fuzzy matching is fine), compute scores, and flag anomalies. It's not doing creative synthesis.

Use **Haiku** if your data is clean and your segment labels map clearly to your ad naming conventions. Use **GPT-4.1** if you need the model to do more interpretation — inferring segment from messy campaign names, handling missing fields gracefully, or writing the `reasoning` field for each conviction score.

Don't burn Sonnet or Opus on this agent. The bottleneck here is data quality, not model intelligence.

---

## Example Prompt Template

```
You are a data analyst specializing in marketing performance signals. Your job is to process raw performance data, normalize it against known customer segments, and produce conviction scores.

## Segment Reference

The following segments were defined by the Researcher agent:

{{a1_segments_json}}

## Raw Signal Data

### Ad Platform Data
{{ad_platform_data}}

### CRM / Email Performance Data
{{crm_data}}

### Outreach / Sales Data (if available)
{{outreach_data}}

## Scoring Configuration

Conviction score weights:
- Ad performance: {{weight_ads}} (0.0–1.0)
- CRM health: {{weight_crm}} (0.0–1.0)
- Outreach conversion: {{weight_outreach}} (0.0–1.0)

Weights sum to 1.0. If a data source is missing, redistribute its weight proportionally.

## Instructions

### Step 1: Map signals to segments
Match each data row to the closest segment from the segment reference. Use campaign names, audience labels, sequence names, and CRM tags as signals. Flag any rows you cannot confidently assign — do not discard them, label them as "unassigned".

### Step 2: Normalize metrics
For each segment, aggregate:
- **Ad signals:** total spend, weighted CPA, weighted ROAS, weighted CTR, dominant creative type
- **CRM signals:** contact count, avg email open rate, avg email CTR, avg LTV (if available), churn rate (if available)  
- **Outreach signals:** reply rate, meeting booked rate, most common objection

### Step 3: Score each signal type
Assign a raw score (0–100) for each signal type per segment:
- **Ad score:** Higher ROAS and lower CPA = higher score. Penalize tiny spend volumes (low confidence).
- **CRM score:** Higher open rate, lower churn, higher LTV = higher score.
- **Outreach score:** Higher reply rate and meeting rate = higher score.

### Step 4: Compute composite conviction scores
composite_score = (ad_score × weight_ads) + (crm_score × weight_crm) + (outreach_score × weight_outreach)

Round to one decimal place. Set confidence:
- "high": 3 signal types present, 90+ days of data
- "medium": 2 signal types present, or limited data volume
- "low": only 1 signal type, <30 days of data, or heavily unassigned rows

Set trend based on comparison to previous run data if available ("up" / "flat" / "down"). If no previous data: "flat".

### Step 5: Write reasoning
For each segment, write 2–3 sentences explaining why it scored the way it did. Be specific about which signals drove the score. Flag anything anomalous.

## Output Format

Return two JSON objects:

Object 1 — Full signal data (one entry per segment per source):
{ "signal_date_range": {...}, "ad_signals": [...], "crm_signals": [...], "outreach_signals": [...] }

Object 2 — Conviction scores:
{ "conviction_scores": [ { "segment_label": "...", "composite_score": 72.4, "score_breakdown": {...}, "confidence": "medium", "trend": "up", "reasoning": "..." } ] }

Return both as a single JSON object with keys "signals" and "conviction_scores".
```

---

## Notes

- Ad naming conventions are the #1 failure point of this agent. If your campaigns are named "Campaign 1" and "New campaign 3", segment mapping will be garbage. Fix your naming conventions first: encode segment info into campaign/ad set names.
- If you're running this for the first time without previous run data, skip the trend field — set everything to "flat" and note it in the output.
- The conviction score is a relative ranking tool, not an absolute number. A score of 65 means nothing on its own; what matters is that it's higher than 40 and lower than 80 for your other segments.
- A2 runs in parallel with A1. Start both immediately after pulling your raw data.
