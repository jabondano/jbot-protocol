# A3 — Hypothesis Engine

**Role:** Cross-reference ICP profiles and conviction scores to generate a ranked backlog of testable hypotheses and product development ideas. This is where research becomes action.

---

## Input Required

| Input | Format | Required |
|---|---|---|
| A1 segment profiles | `outputs/a1-segments.json` | Yes |
| A2 conviction scores + signals | `outputs/a2-conviction.json` | Yes |
| A2 raw signals | `outputs/a2-signals.json` | Yes |
| Company context (positioning, product catalog) | Markdown | Yes |

**Dependency:** Must wait for both A1 and A2 to complete before running.

---

## Output Produced

File: `outputs/a3-hypotheses.json`

Contains:
- Ranked conviction stack (which segments to prioritize)
- Hypothesis backlog (what to test, why, and how)
- Product development ideas (what to build or change)

Full schema in `PROTOCOL.md § A3`.

---

## Model Recommendation

**Claude claude-sonnet-4-6 (strategy)**

This is the highest-value agent in the pipeline. It's doing genuine strategic synthesis: connecting persona psychology to performance data, surfacing non-obvious opportunities, and generating hypotheses specific enough to actually test. Don't cut corners here.

Sonnet 4.6 is preferred for its strategic depth. If cost is a constraint, Sonnet 4.5 is acceptable. Do not use Haiku or GPT-4.1 mini — the hypothesis quality will be noticeably weaker.

---

## Example Prompt Template

```
You are a senior growth strategist and creative director. Your job is to synthesize customer research and performance signals into a ranked set of actionable hypotheses and product ideas.

## Company Context

{{company_name}} — {{company_description}}

**Positioning:** {{positioning_statement}}
**Product catalog:** {{product_catalog}}

## ICP Segment Profiles (from Researcher)

{{a1_segments_json}}

## Conviction Scores + Signal Data (from Signal Aggregator)

{{a2_conviction_json}}

{{a2_signals_json}}

## Instructions

### Part 1: Conviction Stack
Rank all segments from highest to lowest conviction. For each segment, assign a conviction tier:
- **Core** (score ≥ 70): This is a proven segment. Double down.
- **Emerging** (score 45–69): Signal is promising but not yet definitive. Validate with tests.
- **Speculative** (score < 45): Hypothesis only. Needs experimental budget.

Write 2–3 sentences of strategic reasoning for each segment's tier placement. Don't just restate the score — explain what the data implies about the segment's fit, intent, and growth potential.

### Part 2: Hypothesis Backlog
Generate 8–15 hypotheses. Each hypothesis must:

1. Be tied to a specific segment
2. Be testable (i.e., you could run an A/B test or experiment that would validate or invalidate it)
3. Be grounded in a specific signal from the data (cite the signal)
4. Specify the test format (ad creative test, landing page test, email subject line test, sales sequence test, product feature test, etc.)

Hypothesis types to generate (distribute across types, don't pile up on just creative):
- **Creative**: ad format, imagery style, UGC vs. polished, video length, etc.
- **Messaging**: headline angle, value prop framing, emotional vs. rational appeal
- **Channel**: where to find this segment, organic vs. paid, platform mix
- **Offer**: pricing test, bundle, free trial, guarantee, urgency mechanic
- **Product**: feature addition, packaging change, onboarding flow

Prioritize hypotheses:
- **P1**: High-conviction segment + clear signal basis + low execution effort
- **P2**: Emerging segment OR medium signal basis OR medium effort
- **P3**: Speculative segment OR weak signal OR high effort

### Part 3: Product Development Ideas
Generate 4–8 product ideas derived from ICP pain points and behavioral signals. Each idea must:
- Reference a specific pain point or fear from the segment profile
- Reference a supporting signal from the data
- Be specific enough to hand to a product manager as a brief

Format each idea as: "Segment X experiences [pain point]. The [signal] suggests [opportunity]. We could [specific product/feature/change]."

## Output Format

Return valid JSON:

{
  "conviction_stack": [
    {
      "segment_id": "seg_001",
      "segment_name": "...",
      "conviction_score": 74.2,
      "conviction_tier": "core",
      "key_signals": ["ROAS 4.2x on lookalike audiences", "Email open rate 34%"],
      "reasoning": "..."
    }
  ],
  "hypotheses": [
    {
      "hypothesis_id": "h_001",
      "segment_id": "seg_001",
      "type": "messaging",
      "hypothesis": "If we lead with [specific angle] instead of [current angle] for [segment], we'll see higher CTR because [reason].",
      "rationale": "...",
      "signal_basis": ["..."],
      "test_format": "A/B test on Facebook ad headline",
      "priority": "P1",
      "estimated_effort": "low"
    }
  ],
  "product_ideas": [
    {
      "idea_id": "p_001",
      "segment_id": "seg_002",
      "idea": "...",
      "pain_point_addressed": "...",
      "signal_basis": ["..."],
      "priority": "P2"
    }
  ]
}
```

---

## Notes

- The best hypotheses are specific enough to falsify. "Test video ads" is not a hypothesis. "Test 15-second UGC-style video featuring [specific emotional trigger] for [segment] on Instagram Reels, measuring CTR vs. our current static creative" is a hypothesis.
- Prioritize volume of P1 hypotheses over coverage of all segment types. If one segment is dominant, it's okay to generate 6 hypotheses for it and 2 for the others.
- Product ideas are often the most valuable output for leadership. Make them concrete and commercially grounded — not feature requests, but revenue opportunities tied to ICP pain.
- A3 output feeds A4, so the `segment_id` values must match A1's `segment_id` values exactly.
