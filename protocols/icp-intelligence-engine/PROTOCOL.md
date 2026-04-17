# ICP Intelligence Engine — Protocol Specification

Version: 1.0.0  
Status: Production-ready

---

## Overview

This document specifies the full technical design of the ICP Intelligence Engine: data requirements, agent interfaces, output schemas, deployment patterns, and customization guidance. Implement this spec as-written and you get a live, auto-refreshing ICP intelligence system. Deviate with intention; every choice here was made for a reason.

---

## 1. Data Layer Requirements

The system needs four input categories. You don't need all of them on day one, but you need at least two of the first three to get conviction scores worth trusting.

### 1.1 Performance Signals

Pull from your paid channels. These tell you what's actually working with real buyers, not who you think your customer is.

| Field | Source | Notes |
|---|---|---|
| Campaign name | Ad platform | Use naming conventions that encode segment/ICP info |
| Audience segment | Ad platform | Age, interest, lookalike, etc. |
| Impressions, clicks, CTR | Ad platform | Last 90 days minimum |
| Spend, CPA, ROAS | Ad platform | Required for conviction scoring |
| Conversion event type | Ad platform | Purchase, lead, trial, etc. |
| Creative type | Ad platform | Video, static, carousel, UGC |

Supported platforms: Meta Ads, Google Ads, LinkedIn Campaign Manager, TikTok Ads Manager, Pinterest Ads.

### 1.2 CRM / Email Signals

Pull from your customer database. These tell you who your buyers are and how they behave after acquisition.

| Field | Source | Notes |
|---|---|---|
| Contact/customer records | CRM | Include segment tags if available |
| Open rate, CTR by segment | Email platform | Segment-level, not list-level |
| Unsubscribe + complaint rate | Email platform | Strong signal for poor-fit segments |
| LTV or revenue by contact | CRM/ERP | Optional but high-value |
| Churn date + reason | CRM | For SaaS/subscription models |
| NPS score by segment | Survey tool | Optional |

Supported sources: HubSpot, Salesforce, Klaviyo, ActiveCampaign, Mailchimp, Intercom.

### 1.3 Outreach / Sales Signals

Pull from your B2B sales motions. These tell you which personas respond, object, and convert.

| Field | Source | Notes |
|---|---|---|
| Outreach sequence name | Sales tool | Should map to ICP hypothesis |
| Reply rate by sequence | Sales tool | Positive + neutral replies |
| Meeting booked rate | Sales tool | Gold standard for B2B signal |
| Common objections | CRM notes / call logs | Qualitative, but critical |
| Won/lost deal tags | CRM | ICP fit validation |

Supported sources: Apollo, Outreach, Salesloft, Instantly, HubSpot Sequences, manual CSV.

### 1.4 Context Documents

These inform tone, positioning, and persona depth. Feed them into A1 and A3.

| Document | Format | Notes |
|---|---|---|
| Product catalog / service list | Markdown or PDF | Feature set, pricing tiers, key differentiators |
| Brand voice guide | Markdown or doc | Tone, vocabulary, what to avoid |
| Positioning statement | Markdown | Who you serve, what you do, why you win |
| Existing ICPs (if any) | Markdown or doc | Used as baseline; A1 will validate/challenge these |

---

## 2. The 6-Agent Pipeline

### Execution Order

```
[A1 Researcher] ──┐
                   ├──► [A3 Hypothesis Engine] ──► [A4 UX Designer] ──► [A5 Builder] ──► [A6 Publisher]
[A2 Signal Agg] ──┘
```

A1 and A2 run in parallel. Every other agent is sequential. Do not skip steps — each agent produces structured output that the next agent requires.

---

### A1 — Researcher

**Role:** Build deep persona profiles for each ICP segment.

**Input:**
- Context documents (brand voice, positioning, product catalog)
- Existing ICP data (if any) from previous runs or strategy docs
- Optional: Customer interview transcripts, NPS comments, review data (G2, Trustpilot, Amazon reviews, App Store)

**Output schema (JSON):**
```json
{
  "segments": [
    {
      "segment_id": "string",
      "segment_name": "string",
      "demographics": {
        "age_range": "string",
        "gender_skew": "string",
        "income_range": "string",
        "geography": "string",
        "job_title_or_role": "string"
      },
      "psychographics": {
        "values": ["string"],
        "lifestyle_markers": ["string"],
        "media_habits": ["string"],
        "identity_signals": ["string"]
      },
      "pain_points": ["string"],
      "fears": ["string"],
      "motivators": ["string"],
      "emotional_triggers": ["string"],
      "primary_touchpoints": ["string"],
      "language_patterns": ["string"],
      "objections": ["string"],
      "data_confidence": "low | medium | high",
      "sources_used": ["string"]
    }
  ]
}
```

---

### A2 — Signal Aggregator

**Role:** Pull, normalize, and score performance signals from all connected data sources.

**Input:**
- Ad platform exports (CSV or API response)
- CRM/email performance data (CSV or API response)
- Outreach/sales logs (CSV or API response)

**Output schema (JSON):**
```json
{
  "signal_date_range": {
    "start": "YYYY-MM-DD",
    "end": "YYYY-MM-DD"
  },
  "ad_signals": [
    {
      "segment_label": "string",
      "platform": "string",
      "spend": "number",
      "cpa": "number",
      "roas": "number",
      "ctr": "number",
      "conversion_volume": "number",
      "top_creative_type": "string",
      "raw_score": "number"
    }
  ],
  "crm_signals": [
    {
      "segment_label": "string",
      "contact_count": "number",
      "email_open_rate": "number",
      "email_ctr": "number",
      "ltv_avg": "number",
      "churn_rate": "number",
      "raw_score": "number"
    }
  ],
  "outreach_signals": [
    {
      "sequence_name": "string",
      "target_persona": "string",
      "reply_rate": "number",
      "meeting_rate": "number",
      "top_objection": "string",
      "raw_score": "number"
    }
  ],
  "conviction_scores": [
    {
      "segment_label": "string",
      "composite_score": "number",
      "score_breakdown": {
        "ad_performance": "number",
        "crm_health": "number",
        "outreach_conversion": "number"
      },
      "confidence": "low | medium | high",
      "trend": "up | flat | down"
    }
  ]
}
```

**Scoring logic:**  
Composite conviction score = weighted average across available signal types.  
Default weights: ad performance 40%, CRM health 35%, outreach conversion 25%.  
Adjust weights in `example-config.json` based on your business model (e.g., a pure inbound SaaS would weight CRM higher; a direct sales org would weight outreach higher).

---

### A3 — Hypothesis Engine

**Role:** Cross-reference ICP profiles (A1) with conviction scores (A2) to generate ranked, testable hypotheses and product development ideas.

**Input:**
- A1 output (full segments JSON)
- A2 output (full signals + conviction scores JSON)

**Output schema (JSON):**
```json
{
  "conviction_stack": [
    {
      "segment_id": "string",
      "segment_name": "string",
      "conviction_score": "number",
      "conviction_tier": "core | emerging | speculative",
      "key_signals": ["string"],
      "reasoning": "string"
    }
  ],
  "hypotheses": [
    {
      "hypothesis_id": "string",
      "segment_id": "string",
      "type": "creative | messaging | channel | offer | product",
      "hypothesis": "string",
      "rationale": "string",
      "signal_basis": ["string"],
      "test_format": "string",
      "priority": "P1 | P2 | P3",
      "estimated_effort": "low | medium | high"
    }
  ],
  "product_ideas": [
    {
      "idea_id": "string",
      "segment_id": "string",
      "idea": "string",
      "pain_point_addressed": "string",
      "signal_basis": ["string"],
      "priority": "P1 | P2 | P3"
    }
  ]
}
```

---

### A4 — UX/UI Designer

**Role:** Translate A3's structured output into a dashboard specification — layout, components, data hierarchy, and content blocks.

**Input:**
- A1 output (segment profiles)
- A2 output (conviction scores + signals)
- A3 output (hypotheses + product ideas)
- Brand context (colors, fonts, tone from config)

**Output:**  
A Markdown specification file (`dashboard-spec.md`) containing:
- Page structure and navigation
- Component inventory (which data goes where)
- Wireframe descriptions for each section (text-based, no images required)
- Content tone guidelines for the dashboard copy

**Output schema (dashboard-spec.md sections):**
```
# Dashboard Spec

## Page Architecture
[List of pages/sections with purpose]

## Navigation Structure
[How users move through the dashboard]

## Component Inventory
[For each section: component type, data source, display format]

## Section Wireframes
[Text description of each major section layout]

## Copy Guidelines
[Tone, labels, terminology to use/avoid]
```

---

### A5 — Builder

**Role:** Generate the HTML/CSS/JS dashboard from the spec. Ship something that works.

**Input:**
- A4 output (`dashboard-spec.md`)
- A1–A3 outputs (the actual data to populate the dashboard)
- Brand config (colors, fonts, logo URL)

**Output:**
- `dashboard/index.html` — main internal hub
- `dashboard/agency.html` — stripped-down external reference page
- `dashboard/data.json` — structured data snapshot used by the HTML (enables updates without full rebuild)

**Requirements:**
- Zero external dependencies at runtime (inline or self-hosted CSS/JS)
- Must render correctly without a build step (open index.html in browser = working)
- Data-driven: HTML templates read from `data.json` so weekly refresh only requires updating the JSON
- Mobile-readable (responsive layout, not necessarily mobile-first)

---

### A6 — Publisher

**Role:** Deploy the dashboard, configure hosting, and set up the weekly refresh schedule.

**Input:**
- A5 output (`dashboard/` folder)
- Deployment config (platform, project name, domain if custom)
- Cron schedule preference

**Output:**
- Live URL for internal dashboard
- Live URL for external agency page
- Cron/automation config file
- Deployment log

**Supported platforms:**
- **Cloudflare Pages** — `wrangler pages deploy ./dashboard`
- **Vercel** — `vercel --prod ./dashboard`
- **Netlify** — `netlify deploy --prod --dir=./dashboard`

**Weekly refresh pattern:**  
On each scheduled run, only A1 and A2 re-execute with fresh data. A3 re-runs on their output. A4 is skipped (spec doesn't change unless you trigger a redesign). A5 regenerates `data.json` only. A6 re-deploys.

Full pipeline re-run (all 6 agents): trigger manually when you want to change layout, add segments, or do a quarterly deep review.

---

## 3. Deployment Pattern

### Initial Setup
```
Day 1: Full 6-agent run → dashboard live
Week 2+: Refresh run (A1 → A2 → A3 → A5 data.json update → A6 deploy)
Quarter: Full run to update design spec and rebuild
```

### Cron Configuration (GitHub Actions example)
```yaml
name: ICP Intelligence Refresh
on:
  schedule:
    - cron: '0 6 * * 1'  # Every Monday at 6am UTC
  workflow_dispatch:       # Manual trigger

jobs:
  refresh:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run refresh pipeline
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          DATA_SOURCE_CREDENTIALS: ${{ secrets.DATA_SOURCE_CREDENTIALS }}
        run: |
          python run_pipeline.py --mode=refresh
      - name: Deploy updated dashboard
        run: |
          npx wrangler pages deploy ./dashboard --project-name=${{ env.PROJECT_NAME }}
```

### Data Layer Options

| Option | Best For | Setup Time | Cost |
|---|---|---|---|
| **Supabase** | Production teams, multiple users, API access needed | 2–3 hours | Free tier generous |
| **Airtable** | Low-code teams, non-technical stakeholders | 30–60 min | Free tier limited |
| **Notion** | Documentation-heavy orgs, async teams | 30–60 min | Free tier works |
| **Local JSON** | Solo operators, development/testing | 5 min | Free |

For production, use Supabase. It gives you a real database, row-level security, and a REST API that agents can read/write without extra tooling.

---

## 4. Customization Guide

### Adjusting for Industry

**Marketing agencies:**  
Run a separate config per client. Use the `client_id` field in config to namespace all outputs. A6 can deploy to client-specific subdomains. Charge for the initial build; sell weekly refresh as a retainer line item.

**SaaS companies:**  
Add a `plan_tier` dimension to your CRM signals. Track activation rate and churn by ICP — this is where the highest-signal data lives for SaaS. Weight CRM signals at 50%+ in your conviction scoring.

**E-commerce brands:**  
Add purchase cohort data to A2 inputs. Include AOV, repeat purchase rate, and product category affinity by segment. Weight ad performance at 50%+ — e-commerce conviction lives in ROAS and CPA by audience.

**B2B sales orgs:**  
Weight outreach signals at 40%+. Include deal stage velocity and ACV by ICP in A2. A3 should prioritize messaging hypotheses over creative hypotheses.

### Adding Segments

Add new entries to A1's output JSON. A2 will attempt to map incoming signal data to segments using the `segment_label` field. Ensure your ad naming conventions and CRM tags match the segment labels used in config.

### Modifying Conviction Score Weights

In `example-config.json`, update the `scoring_weights` object:
```json
"scoring_weights": {
  "ad_performance": 0.4,
  "crm_health": 0.35,
  "outreach_conversion": 0.25
}
```
Weights must sum to 1.0. If you don't have outreach data, set its weight to 0 and redistribute proportionally.

### Changing Refresh Frequency

Weekly is the default and works for most companies. Adjust the cron expression in your scheduler. For fast-moving e-commerce or agencies running active campaigns, daily refresh of A2 signals (skipping A1) is viable and cheap.

---

## 5. Technology Stack Reference

### Minimum viable stack
- LLM API: Anthropic (Claude) or OpenAI
- Data store: Local JSON files
- Hosting: Cloudflare Pages (free)
- Scheduler: GitHub Actions (free)

### Production stack
- LLM API: Anthropic (primary), OpenAI (fallback)
- Data store: Supabase (PostgreSQL)
- Hosting: Cloudflare Pages or Vercel
- Scheduler: GitHub Actions or a dedicated task runner (Modal, Railway)
- Secrets: GitHub Actions secrets or Doppler

### What this protocol does NOT prescribe
- Your specific API client implementation
- How you authenticate to your data sources
- Your exact folder/file structure beyond the output schema
- Which language to write your orchestration in (Python works; so does TypeScript)

---

## 6. Versioning and Updates

When the protocol changes:
- Patch version: bug fixes, clarifications, no schema changes
- Minor version: new optional fields, new supported platforms
- Major version: breaking schema changes, pipeline restructure

Check `CHANGELOG.md` in the protocol root for version history.
