# ICP Intelligence Engine

A 6-agent pipeline that builds and maintains a living Ideal Customer Profile system — automatically updated, conviction-scored, and published as a live internal dashboard.

---

## The Problem

Most companies have stale ICPs. They're written once in a strategy deck, never revisited, and completely disconnected from actual performance signals. Your ad data knows which segments convert. Your CRM knows who churns. Your outreach logs know what messages land. But none of that feeds back into how you think about your customers.

This protocol closes that loop. It pulls live signals from your ad platforms, CRM, and outreach channels, runs them through an analysis pipeline, generates testable hypotheses, and publishes a live dashboard your whole team can use.

---

## Who This Is For

| Company Type | Use Case |
|---|---|
| **Marketing agencies** | Build this once, run it for every client. Deliver conviction-scored ICP audits as a service. |
| **SaaS companies** | Track segment-level conversion, activation, and churn signals. Feed product roadmap with ICP insights. |
| **E-commerce brands** | Profile buyers by cohort. Identify who loves you and who bounces. Score segments by LTV signals. |
| **B2B sales orgs** | Map ICP conviction to outreach conversion data. Generate messaging hypotheses tied to persona-level signals. |

---

## What You Get

- **ICP X-rays** — demographics, psychographics, fears, motivators, and touchpoints for each customer segment
- **Conviction scores** — each segment scored by signal strength across ad performance, purchase behavior, and outreach results
- **Hypothesis backlog** — ranked list of what to test next, each tied to a specific ICP signal
- **Product development ideas** — surfaced from ICP pain points and behavioral signals
- **Live internal dashboard** — HTML hub, auto-updated weekly, accessible to your whole team
- **External agency/partner reference page** — shareable version for external collaborators

---

## What You Need to Get Started

### Required
- At least one ad platform with 90+ days of data (Meta Ads, Google Ads, LinkedIn Ads, TikTok Ads)
- A CRM or email platform with segment/contact data (HubSpot, Klaviyo, Salesforce, ActiveCampaign, etc.)
- A product catalog or service offering list
- Brand voice and positioning notes (even a single-page doc works)

### Optional but valuable
- B2B outreach logs (reply rates, response sentiment, objection patterns)
- Customer interview notes or NPS/CSAT data
- Cohort analysis or LTV data from your analytics platform

### Infrastructure
- A place to host the output (Cloudflare Pages, Vercel, or Netlify — all free tiers work)
- A data layer to store agent outputs (Supabase, Airtable, or Notion)
- An LLM API key (Anthropic or OpenAI)
- A scheduler for weekly refresh (cron, GitHub Actions, or any task runner)

---

## Estimated Setup Time

| Phase | Time |
|---|---|
| Data prep (pull exports, format inputs) | 2–4 hours |
| Agent configuration | 1–2 hours |
| First run + QA | 1–2 hours |
| Dashboard deployment | 30–60 minutes |
| Cron/automation setup | 30 minutes |
| **Total** | **5–9 hours** |

After initial setup, the system runs itself. Weekly refresh takes ~15–30 minutes of compute with no human intervention.

---

## Directory Structure

```
protocols/icp-intelligence-engine/
├── README.md               ← You are here
├── PROTOCOL.md             ← Full technical specification
├── example-config.json     ← Working config for a fictional SaaS company
└── agent-templates/
    ├── A1-researcher.md
    ├── A2-signal-aggregator.md
    ├── A3-hypothesis-engine.md
    ├── A4-ux-designer.md
    ├── A5-builder.md
    └── A6-publisher.md
```

---

## Quick Start

1. Read `PROTOCOL.md` end-to-end before touching anything.
2. Fill out `example-config.json` with your company's data.
3. Set up your data layer (Supabase recommended for production; Airtable for low-code).
4. Run A1 and A2 in parallel on your first data pull.
5. Chain A3 → A4 → A5 → A6 in sequence.
6. Schedule weekly refresh via cron or GitHub Actions.
7. Share the published URL with your team.

---

## Part of the jbot-protocol Framework

This protocol is designed to run standalone or as a component inside a larger agent system. See the root `README.md` for integration patterns with other jbot protocols.
