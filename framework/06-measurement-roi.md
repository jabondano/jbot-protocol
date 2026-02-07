# Pillar 6: Measurement & ROI

**What gets measured gets funded**

---

## Overview

Measurement & ROI is the pillar that sustains everything else. Without clear metrics tied to business outcomes, AI agent programs lose executive sponsorship and wither. This pillar provides the framework for quantifying value, tracking adoption, and making the business case for continued investment.

## Core Principles

### 6.1 The Three Value Categories

Every agent deployment creates value in one or more of these categories:

| Category | Definition | Example |
|----------|-----------|---------|
| **Time Saved** | Hours freed from manual, repetitive work | Agent generates daily reports in 2 minutes vs. 45 minutes manual |
| **Errors Reduced** | Fewer mistakes in data entry, routing, compliance | Ticket categorization accuracy improves from 72% to 95% |
| **Capacity Unlocked** | New capabilities that weren't possible before | Cross-division synthesis that no single person had time to do |

**The order matters.** Time Saved is easiest to measure and most compelling to executives. Start there.

### 6.2 ROI Calculation Framework

```
ROI = (Value Created - Cost of Implementation) / Cost of Implementation x 100

Where:
  Value Created = (Hours Saved x Hourly Rate) + (Errors Avoided x Cost per Error) + Capacity Value
  Cost = API spend + setup time + ongoing maintenance + change management
```

#### Practical Example

| Line Item | Calculation | Monthly Value |
|-----------|-------------|:------------:|
| Daily reports automated (3 divisions) | 3 reports x 45 min x 22 days x $50/hr | $4,950 |
| Order monitoring (manual → automated) | 2 hrs/day x 22 days x $35/hr | $1,540 |
| Ticket categorization errors reduced | 50 errors/mo x 15 min fix x $35/hr | $437 |
| Cross-division synthesis (new capability) | 4 hrs/week x $75/hr | $1,300 |
| **Total monthly value** | | **$8,227** |
| **Monthly cost** (API + maintenance) | | **($800)** |
| **Net monthly ROI** | | **$7,427** |
| **Annual ROI** | ($8,227 x 12 - $9,600) / $9,600 | **928%** |

### 6.3 Metrics Taxonomy

#### Leading Indicators (predict future success)

| Metric | Definition | Target |
|--------|-----------|--------|
| **Adoption Rate** | Active users / Total eligible users | >80% by month 3 |
| **Tasks Completed** | Agent tasks successfully executed per week | Increasing week-over-week |
| **Time to First Value** | Days from agent deployment to first useful output | <5 business days |
| **User Satisfaction** | Weekly pulse survey (1-5 scale) | >4.0 |

#### Lagging Indicators (confirm realized value)

| Metric | Definition | Target |
|--------|-----------|--------|
| **Cost Savings** | Documented reduction in operational costs | Measurable by month 3 |
| **Revenue Impact** | Revenue attributed to agent-enabled activities | Track from month 6 |
| **Error Rate** | Mistakes in agent-handled processes vs. baseline | <50% of pre-agent baseline |
| **Throughput** | Volume of work processed per unit time | >150% of pre-agent baseline |

### 6.4 Human-Agent Ratio (HAR)

The HAR metric tracks how deeply agents are embedded in each division's operations.

```
HAR = Number of Active Agent Tasks / Number of Human FTEs in Division
```

| HAR Range | Interpretation | Maturity Stage |
|-----------|---------------|----------------|
| 0.1 - 0.5 | Agent assists with a few tasks | Pilot |
| 0.5 - 2.0 | Agent handles significant portion of routine work | Adoption |
| 2.0 - 5.0 | Agent is a core team member | Scaled |
| 5.0+ | Division is AI-native | Transformed |

**Tracking methodology:**
1. Count distinct recurring agent tasks per division (daily alerts, weekly reports, etc.)
2. Count human FTEs in the same division
3. Calculate ratio monthly
4. Track trend over time — the trajectory matters more than the absolute number

### 6.5 Dashboarding

Make metrics visible and automatic. A dashboard that requires manual updates won't be updated.

**Recommended stack:**
- **Data store:** Supabase (PostgreSQL) — agent outputs and metrics logged via API
- **Frontend:** Astro + React — lightweight, fast, deployable to Cloudflare Pages
- **Refresh:** Agents write their own metrics as part of task completion (self-reporting)

**Dashboard sections:**

| Section | Metrics | Refresh |
|---------|---------|---------|
| Executive Summary | Total ROI, cost savings, HAR by division | Weekly |
| Adoption | Active users, tasks completed, satisfaction scores | Daily |
| Performance | Response quality, error rates, throughput | Daily |
| Cost | API spend by agent, cost per task, budget vs. actual | Weekly |

## Implementation Checklist

- [ ] Establish baseline metrics before agent deployment
- [ ] Define target KPIs per division
- [ ] Instrument agent tasks to log outcomes automatically
- [ ] Build or configure dashboard
- [ ] Set up weekly metric review cadence
- [ ] Create monthly ROI summary for executive sponsors
- [ ] Review and adjust targets quarterly

## The Sustainability Benchmark

Research from Deloitte's AI scaling studies suggests that organizations need to achieve **45% operational sustainability** — meaning AI-augmented processes must maintain effectiveness for 3+ years without constant rebuilding. This requires:

1. **Feedback loops** — Instrument every agent decision; log human overrides
2. **Continuous improvement** — Monthly review of agent performance; quarterly skill updates
3. **Knowledge maintenance** — SOPs and agent instructions reviewed when processes change
4. **Cost discipline** — Track API spend per task; optimize model selection (use Haiku for simple tasks, Opus for complex)

## Deliverables

1. **ROI Model** — Spreadsheet with baseline, targets, and actuals
2. **Metrics Dashboard** — Automated tracking of leading and lagging indicators
3. **Monthly ROI Report** — Executive summary with trends and recommendations
4. **HAR Tracking** — Division-level human-agent ratios over time

---

## Related Resources

- [Getting Started Guide](../guides/getting-started.md) — Includes metric setup in Week 3
- [Model Selection Guide](../implementation/model-selection-guide.md) — Cost optimization
