# A1 — Researcher

**Role:** Build deep, evidence-backed persona profiles for each ICP segment your company serves.

---

## Input Required

| Input | Format | Required |
|---|---|---|
| Brand voice + positioning doc | Markdown or plaintext | Yes |
| Product/service catalog | Markdown or PDF | Yes |
| Existing ICP definitions (if any) | Markdown, doc, or JSON | No |
| Customer review data | Plaintext (scraped or exported) | No |
| Customer interview notes | Plaintext or transcript | No |
| NPS/CSAT open-text responses | CSV or plaintext | No |
| Competitor positioning notes | Markdown | No |

**Minimum to run:** Brand context + product catalog. Everything else enriches the output.

---

## Output Produced

A structured JSON file matching the schema in `PROTOCOL.md § A1`. One entry per segment.  
File: `outputs/a1-segments.json`

---

## Model Recommendation

**Claude claude-sonnet-4-5 (synthesis/creative)**

Persona construction requires nuanced synthesis — connecting demographic facts to emotional reality, finding the language patterns that resonate, identifying fears that aren't always stated directly. Haiku is too thin for this. GPT-4.1 works. Sonnet 4.5 is the sweet spot: deep reasoning + narrative quality without the cost of the frontier models.

Use `claude-sonnet-4-6` if you have access and this is a quarterly deep review where precision matters more than cost.

---

## Example Prompt Template

Adapt this to your company. Replace `{{placeholders}}` with your actual content.

```
You are an expert market researcher and customer psychologist. Your job is to build precise, evidence-backed ICP (Ideal Customer Profile) profiles for {{company_name}}, a company that {{company_description}}.

## Context

**Product/Service:**
{{product_catalog}}

**Brand Positioning:**
{{positioning_statement}}

**Brand Voice:**
{{brand_voice_notes}}

{% if existing_icps %}
**Existing ICP definitions to validate/refine:**
{{existing_icps}}
{% endif %}

{% if review_data %}
**Customer review data:**
{{review_data}}
{% endif %}

{% if interview_notes %}
**Customer interview notes:**
{{interview_notes}}
{% endif %}

## Instructions

1. Identify 3–6 distinct customer segments. Name them in a way that's memorable and specific (not "Persona A" — give them a real archetype label like "Efficiency-Obsessed Ops Manager" or "Budget-Constrained Founder").

2. For each segment, produce a complete X-ray covering:
   - Demographics: age range, gender skew if relevant, income/budget range, geography, job title or life stage
   - Psychographics: core values, lifestyle markers, media habits, identity signals
   - Pain points: what problems are they actively trying to solve?
   - Fears: what outcomes are they trying to avoid? What keeps them from buying?
   - Motivators: what does success look like for them?
   - Emotional triggers: what makes them lean in? What makes them scroll past?
   - Primary touchpoints: where do they discover products like this? Where do they research?
   - Language patterns: actual words and phrases this person uses (not marketing speak)
   - Common objections: what do they say when they don't buy?

3. Rate your confidence in each segment profile: low / medium / high. Be honest.

4. Note which sources informed each profile.

## Output Format

Return valid JSON matching this schema exactly:

{
  "segments": [
    {
      "segment_id": "seg_001",
      "segment_name": "...",
      "demographics": { ... },
      "psychographics": { ... },
      "pain_points": [...],
      "fears": [...],
      "motivators": [...],
      "emotional_triggers": [...],
      "primary_touchpoints": [...],
      "language_patterns": [...],
      "objections": [...],
      "data_confidence": "low | medium | high",
      "sources_used": [...]
    }
  ]
}

Be specific. Avoid generalities that could describe any customer for any company. If you're unsure about a field, say so inside the field rather than hallucinating specifics.
```

---

## Notes

- If you have review data (Amazon, G2, App Store, Trustpilot), mine it hard. Reviews are the most honest customer language you'll ever get — they write the way they think, not the way you want them to.
- Competitor reviews are equally valuable. What do people complain about with competitors? That's your differentiation signal.
- On the first run, aim for 4–5 segments. You can always add more later. Fewer sharp segments beat many vague ones.
- A1 output is used by A3 (Hypothesis Engine), so the `segment_id` field must be consistent and stable across runs.
