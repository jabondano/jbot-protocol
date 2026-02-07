# Claude Code Implementation Guide

**Deploying Division-Based AI Agents with Claude Code**

---

## Overview

This guide documents implementing the JBOT Protocol using [Claude Code](https://claude.ai/claude-code), Anthropic's CLI-based AI coding and operations tool. Where the [OpenClaw guide](./openclaw-fleet.md) covers a self-hosted multi-agent fleet, this guide covers the Claude-native approach — leveraging CLAUDE.md configuration files, MCP servers, hooks, skills, and subagents.

## Architecture

### The CLAUDE.md Hierarchy

Claude Code uses a four-level configuration hierarchy. Each level inherits from the one above and can override or extend it.

| Level | Location | Purpose | Scope |
|-------|----------|---------|-------|
| **Enterprise** | `/Library/Application Support/ClaudeCode/CLAUDE.md` | Org-wide policies, compliance rules | All users on machine |
| **Project** | `./CLAUDE.md` or `./.claude/CLAUDE.md` | Team instructions, project conventions | Team via source control |
| **User** | `~/.claude/CLAUDE.md` | Personal preferences, role context | Individual across projects |
| **Local** | `./CLAUDE.local.md` | Personal project-specific overrides | Individual, current project |

#### Mapping to JBOT Protocol

| JBOT Concept | CLAUDE.md Level | What Goes Here |
|-------------|-----------------|----------------|
| **Governance Policy** | Enterprise | Action restrictions, prohibited operations, compliance rules |
| **Division Knowledge** | Project | Division-specific SOPs, system access, agent behavior |
| **Personal Context** | User | Role, team directory, personal preferences |
| **Agent Customization** | Local | Experimental behaviors, local-only overrides |

#### Example: Division-Level CLAUDE.md

```markdown
# CLAUDE.md — Fulfillment Division

## Role
You are the fulfillment operations agent for [Company].

## Systems Access
- Shopify (read orders, inventory) via MCP
- NetSuite (read fulfillment records) via MCP
- Slack (post to #fulfillment channel) via MCP

## Key Processes
- Late order alert: Flag orders unfulfilled >3 business days
- Inventory warning: Alert when any SKU drops below 50 units
- Daily summary: Post fulfillment metrics to Slack at 9am ET

## Governance
- NEVER modify orders or inventory directly
- ALWAYS post alerts to Slack; never email customers directly
- Escalate orders >$500 or >7 days late to COO
```

### MCP Server Configuration

Claude Code connects to business systems through MCP servers, configured in the project or user settings:

```json
{
  "mcpServers": {
    "shopify": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-shopify"],
      "env": { "SHOPIFY_ACCESS_TOKEN": "shpat_..." }
    },
    "netsuite": {
      "command": "node",
      "args": ["/path/to/mcp-netsuite/index.js"],
      "env": {
        "NS_ACCOUNT_ID": "...",
        "NS_CONSUMER_KEY": "...",
        "NS_TOKEN_ID": "..."
      }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-slack"],
      "env": { "SLACK_BOT_TOKEN": "xoxb-..." }
    }
  }
}
```

### Claude Code Hooks

Hooks are shell commands that execute in response to agent events. Use them for governance enforcement:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__shopify__update_order",
        "command": "echo 'BLOCKED: Order modifications require manual approval' && exit 1"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "mcp__slack__send_channel_message",
        "command": "echo 'Message sent' >> /var/log/agent-audit.log"
      }
    ]
  }
}
```

**Hook types and governance mapping:**

| Hook | When It Fires | Governance Use |
|------|--------------|----------------|
| `PreToolUse` | Before any tool call | Block prohibited actions, enforce read-only |
| `PostToolUse` | After tool call completes | Audit logging, notification triggers |
| `Notification` | When agent produces output | Alert human supervisors |

### Skills and Subagents

Claude Code skills are reusable prompt templates that encode institutional knowledge — the same concept as OpenClaw skills but stored as markdown files.

**Skill structure:**

```
~/.claude/skills/
├── late-orders-alert/
│   └── SKILL.md
├── daily-ops-briefing/
│   └── SKILL.md
└── inventory-watchdog/
│   └── SKILL.md
```

**Subagents** allow Claude Code to delegate work to specialized agents running in parallel:

```markdown
## Subagent Configuration in CLAUDE.md

Use Task tool with these specialized agents:
- `shipbot`: Fulfillment queries (Shopify, NetSuite MCP)
- `mktgbot`: Marketing data (Airtable, campaign MCP)
- `financebot`: Financial reports (NetSuite MCP, Supabase)
```

## Opus 4.6 Capabilities

### Agent Teams (Research Preview)

Opus 4.6 introduced **Agent Teams** — parallel multi-agent coordination where multiple Claude instances work simultaneously on different aspects of a task.

**Implications for JBOT Protocol:**
- A single Claude Code session can spawn parallel subagents for each division
- Morning briefing can query fulfillment, marketing, sales, and finance simultaneously
- Cross-division synthesis happens in the coordinator agent after parallel data collection

**Example: Parallel Morning Briefing**

```
Coordinator (Opus) ──┬── shipbot subagent (Haiku): "Get late orders"
                     ├── mktgbot subagent (Haiku): "Get campaign metrics"
                     ├── salesbot subagent (Haiku): "Get pipeline updates"
                     └── financebot subagent (Sonnet): "Get revenue summary"

All return → Coordinator synthesizes into unified briefing
```

### Extended Context (1M tokens, beta)

For knowledge-heavy divisions, the extended context window means:
- Entire SOP libraries can be loaded into a single session
- Quarterly financial data can be analyzed without chunking
- Complex cross-division analysis with full context preservation

### Extended Output (128K tokens)

Enables generation of:
- Complete SOP drafts in a single pass
- Full board report compilations
- Comprehensive audit reports with detailed findings

### Adaptive Thinking

Claude dynamically decides how much reasoning to apply based on task complexity:
- Simple data lookups: minimal thinking, fast response
- Complex analysis: extended reasoning, higher quality
- This maps naturally to the Integration Spectrum: routine tasks need less reasoning, strategic decisions need more

### Inference Geography

US-only inference option (`anthropic-inference-geo: us`) for:
- Financial data processing (SOX compliance)
- Healthcare-adjacent operations
- Government contracting contexts

## Multi-Model Strategy

Not every agent task requires Opus. Route by complexity and stakes:

| Task Type | Model | Rationale |
|-----------|-------|-----------|
| Ticket categorization | Haiku | High volume, low complexity, fast |
| Data extraction | Haiku | Structured input/output, cost-efficient |
| Report drafting | Sonnet | Moderate complexity, good writing quality |
| Data analysis | Sonnet | Balanced reasoning and cost |
| Cross-division synthesis | Opus | Complex reasoning, multiple data sources |
| Strategic recommendations | Opus | High stakes, nuanced judgment |
| Coordinator/orchestrator | Opus | Needs to route and synthesize across agents |

See the [Model Selection Guide](./model-selection-guide.md) for detailed cost analysis and decision framework.

## Template Mapping

JBOT Protocol templates map directly to CLAUDE.md structure:

| JBOT Template | Claude Code Equivalent |
|--------------|----------------------|
| `DIVISION.md` | Project-level CLAUDE.md (division context) |
| `PROCESSES.md` | Skills (SKILL.md files encoding SOPs) |
| `SYSTEMS.md` | MCP server configuration + tool documentation |
| `CUSTOM-INSTRUCTIONS.md` | CLAUDE.md role and behavior section |
| `discovery-interview.md` | Prompt template for discovery sessions |

## Cost Optimization Patterns

### Pattern 1: Model Routing
Use Haiku for 80% of tasks, Sonnet for 15%, Opus for 5%. This can reduce API spend by 60-70% vs. using Opus for everything.

### Pattern 2: Caching
Claude Code caches MCP tool results within a session. Structure workflows to batch related queries rather than making individual calls.

### Pattern 3: Compaction
For long-running sessions, Claude Code's automatic compaction summarizes conversation history. Design agent workflows to complete discrete tasks within a session rather than maintaining indefinite state.

### Pattern 4: Skill Reuse
Encode common operations as skills. A well-written skill is invoked with a short trigger rather than re-explaining the full workflow each time.

## Implementation Checklist

- [ ] Set up CLAUDE.md hierarchy (enterprise → project → user)
- [ ] Configure MCP servers for each business system
- [ ] Create skill files for top 5 recurring tasks per division
- [ ] Define hooks for governance enforcement
- [ ] Set up audit logging via PostToolUse hooks
- [ ] Configure subagent routing for parallel operations
- [ ] Establish model selection defaults per task type
- [ ] Test escalation paths (agent → human)
- [ ] Document recovery procedures for failures

---

## Related Resources

- [OpenClaw Fleet Guide](./openclaw-fleet.md) — Alternative: self-hosted multi-agent deployment
- [Model Selection Guide](./model-selection-guide.md) — Detailed model routing and cost analysis
- [Division Architecture](../framework/01-division-architecture.md) — Organizational patterns
- [Governance](../framework/04-governance.md) — Guardrails and oversight
