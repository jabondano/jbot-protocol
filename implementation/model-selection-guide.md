# Model Selection Guide

**Matching model capability to task complexity**

---

## Overview

Not every agent task needs the most powerful model. Routing tasks to the right model tier reduces costs by 60-70% while maintaining quality where it matters. This guide provides a decision framework for model selection across JBOT Protocol deployments.

## The Four Tiers

> Pricing and model IDs current as of June 2026 (Claude Haiku 4.5 / Sonnet 4.6 / Opus 4.8 / Fable 5). Check current pricing before budgeting — tier prices have shifted meaningfully over time (Opus-tier pricing *dropped* 3x between generations).

### Tier 1: Haiku — High Volume, Low Complexity

**Model:** `claude-haiku-4-5` &nbsp;|&nbsp; **Cost:** ~$1 / 1M input tokens, ~$5 / 1M output tokens

**Best for:**
- Ticket categorization and routing
- Data extraction from structured sources
- Status checks and simple lookups
- Notification formatting
- Routine data validation

**Characteristics:**
- Response time: <1 second
- Context window: 200K tokens
- Strengths: Speed, cost efficiency, structured output
- Limitations: Shallow reasoning, may miss nuance

**Example tasks:**

| Task | Input | Output |
|------|-------|--------|
| Categorize support ticket | Ticket text | Category + priority |
| Extract order details | Email body | Structured order data |
| Format daily alert | Raw metrics | Formatted Slack message |
| Validate data entry | Form submission | Pass/fail + errors |

### Tier 2: Sonnet — Moderate Complexity

**Model:** `claude-sonnet-4-6` &nbsp;|&nbsp; **Cost:** ~$3 / 1M input tokens, ~$15 / 1M output tokens

**Best for:**
- Report drafting and narrative generation
- Data analysis with interpretation
- Multi-step workflow execution
- Content creation (marketing, communications)
- Process documentation

**Characteristics:**
- Response time: 2-5 seconds
- Context window: 1M tokens
- Strengths: Good writing, solid reasoning, balanced cost
- Limitations: May struggle with highly complex cross-domain synthesis

**Example tasks:**

| Task | Input | Output |
|------|-------|--------|
| Weekly department summary | Raw metrics + context | Narrative report |
| Campaign performance analysis | Ad platform data | Analysis with recommendations |
| SOP documentation | Interview notes | Structured SOP draft |
| Customer communication draft | Ticket + context | Professional response |

### Tier 3: Opus — Complex Reasoning, High Stakes

**Model:** `claude-opus-4-8` &nbsp;|&nbsp; **Cost:** ~$5 / 1M input tokens, ~$25 / 1M output tokens

**Best for:**
- Cross-division synthesis and strategic analysis
- Board report compilation
- Complex problem diagnosis
- Multi-agent coordination (orchestrator role)
- Decision support for high-stakes scenarios

**Characteristics:**
- Response time: 5-15 seconds
- Context window: 1M tokens
- Output: up to 128K tokens
- Strengths: Deep reasoning, nuanced judgment, multi-source synthesis, long-horizon autonomy
- Limitations: Higher cost than Sonnet, slower response

**Example tasks:**

| Task | Input | Output |
|------|-------|--------|
| Morning ops briefing | Data from 4 division agents | Synthesized executive summary |
| Board metrics package | Monthly data across all systems | Complete board report |
| Root cause analysis | Incident data + system logs | Diagnosis with action plan |
| Strategic recommendation | Market data + internal metrics | Options analysis with tradeoffs |

### Tier 4: Frontier — The Hardest Problems (Use Sparingly)

**Model:** `claude-fable-5` (Claude 5 family) &nbsp;|&nbsp; **Cost:** ~$10 / 1M input tokens, ~$50 / 1M output tokens

**Best for:**
- Long-horizon autonomous work measured in hours, not minutes (overnight builds, large migrations, deep multi-source research)
- Problems your Opus-tier agents have demonstrably failed at
- One-off strategic work where correctness matters far more than cost

**Characteristics:**
- Reasoning is always on; single turns can run many minutes — plan for async patterns, not chat-style latency
- Context window: 1M tokens; output up to 128K
- Strengths: The deepest reasoning available; reliable parallel sub-agent delegation
- Limitations: 2x Opus pricing; overkill for anything a lower tier handles

**The governance rule for this tier:** no recurring cron job gets frontier-tier by default. It's a deliberate, per-task escalation — if a scheduled job seems to "need" it, the job is probably mis-scoped.

## Decision Framework

### The 2x2: Volume x Complexity

```
                    Low Volume              High Volume
                ┌───────────────────┬───────────────────┐
  High          │                   │                   │
  Complexity    │   Opus            │   Sonnet          │
                │   (worth the cost │   (balance cost   │
                │    for quality)   │    and quality)   │
                ├───────────────────┼───────────────────┤
  Low           │                   │                   │
  Complexity    │   Sonnet          │   Haiku           │
                │   (Haiku works    │   (optimize for   │
                │    too)           │    cost)          │
                └───────────────────┴───────────────────┘
```

### Decision Tree

```
Is the task high-stakes (financial, external-facing, strategic)?
├── Yes → Is it a synthesis/coordination task?
│         ├── Yes → Opus
│         └── No → Sonnet
└── No → Is the volume >50 tasks/day?
          ├── Yes → Haiku
          └── No → Is writing quality important?
                    ├── Yes → Sonnet
                    └── No → Haiku
```

## Monthly Budget Examples

### Small Team (1-2 divisions, 5-10 agent tasks/day)

| Model | Tasks/Month | Avg Tokens/Task | Monthly Cost |
|-------|:-----------:|:---------------:|:------------:|
| Haiku | 150 | 5K in + 2K out | ~$2.25 |
| Sonnet | 40 | 10K in + 5K out | ~$4.20 |
| Opus | 10 | 20K in + 10K out | ~$3.50 |
| **Total** | **200** | | **~$10/mo** |

### Mid-Size Team (4-6 divisions, 20-40 agent tasks/day)

| Model | Tasks/Month | Avg Tokens/Task | Monthly Cost |
|-------|:-----------:|:---------------:|:------------:|
| Haiku | 600 | 5K in + 2K out | ~$9.00 |
| Sonnet | 150 | 10K in + 5K out | ~$15.75 |
| Opus | 30 | 30K in + 15K out | ~$15.75 |
| **Total** | **780** | | **~$40/mo** |

### Enterprise (8+ divisions, 100+ agent tasks/day)

| Model | Tasks/Month | Avg Tokens/Task | Monthly Cost |
|-------|:-----------:|:---------------:|:------------:|
| Haiku | 2,000 | 5K in + 2K out | ~$30.00 |
| Sonnet | 500 | 10K in + 5K out | ~$52.50 |
| Opus | 100 | 40K in + 20K out | ~$70.00 |
| **Total** | **2,600** | | **~$155/mo** |

*Note: Costs are estimates based on published pricing as of June 2026. Actual costs vary with prompt caching (cached reads cost ~10% of base input price), batch API discounts (50% off for async workloads), and token efficiency. A notable shift since this guide's first edition: Opus-tier pricing dropped 3x while Haiku rose 4x — the spread between tiers narrowed, which weakens "Haiku by default" as a pure cost argument and strengthens routing by capability.*

## Optimization Tips

0. **Use the effort dial before changing tiers.** Current models expose an `effort` parameter (low / medium / high / max) that controls reasoning depth *within* a tier. A Sonnet task that under-performs at low effort often just needs medium — cheaper than promoting it to Opus. Treat the model assignment AND the effort level as part of every cron job's definition; that's a governance decision, not a tuning afterthought.
1. **Start with Haiku for high-volume mechanical tasks.** Only promote when quality demonstrably suffers — but note the narrowed price spread above: for low-volume tasks, the absolute savings of Haiku over Sonnet are often pennies.
2. **Use prompt caching.** Repeated system prompts and CLAUDE.md content get cached, reducing input token costs significantly.
3. **Batch related queries.** Instead of 5 separate Haiku calls, batch into 1 Sonnet call with structured output.
4. **Monitor cost per task.** Track `tokens_used / tasks_completed` weekly. If a Haiku task consistently needs retries, promote it to Sonnet — the retries cost more than the upgrade.
5. **Cache MCP results.** Within a session, avoid re-querying the same data. Structure workflows to query once, analyze multiple ways.

---

## Related Resources

- [Claude Code Guide](./claude-code-guide.md) — Multi-model routing in Claude Code
- [Measurement & ROI](../framework/06-measurement-roi.md) — Cost tracking as part of ROI
