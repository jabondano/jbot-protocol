# Model Selection Guide

**Matching model capability to task complexity**

---

## Overview

Not every agent task needs the most powerful model. Routing tasks to the right model tier reduces costs by 60-70% while maintaining quality where it matters. This guide provides a decision framework for model selection across JBOT Protocol deployments.

## The Three Tiers

### Tier 1: Haiku — High Volume, Low Complexity

**Cost:** ~$0.25 / 1M input tokens, ~$1.25 / 1M output tokens

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

**Cost:** ~$3 / 1M input tokens, ~$15 / 1M output tokens

**Best for:**
- Report drafting and narrative generation
- Data analysis with interpretation
- Multi-step workflow execution
- Content creation (marketing, communications)
- Process documentation

**Characteristics:**
- Response time: 2-5 seconds
- Context window: 200K tokens
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

**Cost:** ~$15 / 1M input tokens, ~$75 / 1M output tokens

**Best for:**
- Cross-division synthesis and strategic analysis
- Board report compilation
- Complex problem diagnosis
- Multi-agent coordination (orchestrator role)
- Decision support for high-stakes scenarios

**Characteristics:**
- Response time: 5-15 seconds
- Context window: 200K tokens (1M beta)
- Output: up to 128K tokens
- Strengths: Deep reasoning, nuanced judgment, multi-source synthesis
- Limitations: Highest cost, slower response

**Example tasks:**

| Task | Input | Output |
|------|-------|--------|
| Morning ops briefing | Data from 4 division agents | Synthesized executive summary |
| Board metrics package | Monthly data across all systems | Complete board report |
| Root cause analysis | Incident data + system logs | Diagnosis with action plan |
| Strategic recommendation | Market data + internal metrics | Options analysis with tradeoffs |

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
| Haiku | 150 | 5K in + 2K out | ~$0.56 |
| Sonnet | 40 | 10K in + 5K out | ~$4.20 |
| Opus | 10 | 20K in + 10K out | ~$10.50 |
| **Total** | **200** | | **~$15/mo** |

### Mid-Size Team (4-6 divisions, 20-40 agent tasks/day)

| Model | Tasks/Month | Avg Tokens/Task | Monthly Cost |
|-------|:-----------:|:---------------:|:------------:|
| Haiku | 600 | 5K in + 2K out | ~$2.25 |
| Sonnet | 150 | 10K in + 5K out | ~$15.75 |
| Opus | 30 | 30K in + 15K out | ~$47.25 |
| **Total** | **780** | | **~$65/mo** |

### Enterprise (8+ divisions, 100+ agent tasks/day)

| Model | Tasks/Month | Avg Tokens/Task | Monthly Cost |
|-------|:-----------:|:---------------:|:------------:|
| Haiku | 2,000 | 5K in + 2K out | ~$7.50 |
| Sonnet | 500 | 10K in + 5K out | ~$52.50 |
| Opus | 100 | 40K in + 20K out | ~$210.00 |
| **Total** | **2,600** | | **~$270/mo** |

*Note: Costs are estimates based on published pricing as of early 2026. Actual costs vary with prompt caching, batch API discounts, and token efficiency.*

## Optimization Tips

1. **Start with Haiku everywhere.** Only promote to a higher tier when quality demonstrably suffers.
2. **Use prompt caching.** Repeated system prompts and CLAUDE.md content get cached, reducing input token costs significantly.
3. **Batch related queries.** Instead of 5 separate Haiku calls, batch into 1 Sonnet call with structured output.
4. **Monitor cost per task.** Track `tokens_used / tasks_completed` weekly. If a Haiku task consistently needs retries, promote it to Sonnet — the retries cost more than the upgrade.
5. **Cache MCP results.** Within a session, avoid re-querying the same data. Structure workflows to query once, analyze multiple ways.

---

## Related Resources

- [Claude Code Guide](./claude-code-guide.md) — Multi-model routing in Claude Code
- [Measurement & ROI](../framework/06-measurement-roi.md) — Cost tracking as part of ROI
