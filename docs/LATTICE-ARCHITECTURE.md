# The jbot Protocol
### An open architecture for building AI operating systems for businesses

> **Status:** Draft v1 — for review and polishing
> **Author:** jabondano
> **Last updated:** March 2026

---

## 0. Manifesto

Most AI deployments fail the same way. A single assistant. A single thread. A human babysitting it.

That's not an AI system. That's a smart autocomplete.

Real businesses don't run on one person doing everything. They run on teams — specialized, coordinated, with shared context and clear ownership. Your AI layer should work the same way.

The jbot Protocol is a framework for building AI operating systems: fleets of specialized agents that share memory, route signals, escalate intelligently, and run continuously without constant supervision.

Not automation. Organizational awareness.

The biological framing: think of it as a nervous system, not a chatbot. Sensors in every business function. A signal layer connecting them. An orchestrator synthesizing the inputs into action. The human at the top of the loop — not in the middle of it.

This repository is the reference implementation. Fork it. Adapt it. Run your own instance.

---

## 1. Architecture Primitives

### The Orchestrator

One agent serves as the human's primary interface. It doesn't do everything — it routes, escalates, synthesizes, and remembers. It's the only agent the operator talks to directly.

The orchestrator owns:
- Long-term memory (MEMORY.md)
- Heartbeat monitoring across the fleet
- Alert escalation from specialized agents
- Cross-agent task routing
- The operator relationship

### Specialized Agents

Each domain gets its own agent. Domain ownership means:
- One agent per business function (sales, ops, marketing, fulfillment, finance)
- Each agent has its own SOUL.md, MEMORY.md, HEARTBEAT.md
- Agents don't talk to each other directly — they communicate through shared data
- Cron jobs are the primary execution unit, not reactive chat

### The Three-File Anatomy

Every agent in the fleet carries three core files:

**SOUL.md** — Identity and operator philosophy
- Who this agent is
- What operator mental model drives it (Munger, Bezos, Grove, Goldratt, etc.)
- How it makes decisions under uncertainty
- What it never does without human approval

**MEMORY.md** — Curated long-term brain
- Git-persisted intelligence that survives session resets
- Structured: business context, key relationships, active projects, decisions made
- Updated on meaningful events, not on every interaction

**HEARTBEAT.md** — Operating rhythm
- What to check on every wake
- Standing alerts and thresholds
- Periodic task schedule
- Escalation rules

### Skills as Capability Expansion

Skills are modular capability packages that extend agents without retraining. A skill is a SKILL.md file that tells the agent how to use a specific tool, API, or workflow.

The rule: if a capability can be expressed as instructions + examples, it belongs in a skill, not in the agent's base prompt. This keeps agents lean and makes capabilities composable.

Browse: https://skills.sh | Install: `npx skills add <owner/repo@skill>`

---

## 2. The Signal Schema Standard

The most underrated primitive in multi-agent architecture is the shared signal layer.

Agents never talk to each other directly. They write signals. Other agents read them.

This is the `bot_signals` table — a canonical schema you can implement in any database.

### Schema

```sql
CREATE TABLE bot_signals (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  bot_name      TEXT NOT NULL,
  signal_type   TEXT NOT NULL,
  entity_type   TEXT,
  entity_id     TEXT,
  payload       JSONB,
  severity      TEXT NOT NULL DEFAULT 'info',
  read_by       TEXT[],
  created_at    TIMESTAMPTZ DEFAULT NOW()
);
```

### Severity Taxonomy

| Level | Meaning | Action |
|-------|---------|--------|
| `info` | Routine completion, no action needed | Log only |
| `warning` | Anomaly detected, monitor | Log + flag in next digest |
| `alert` | Action needed, not time-critical | Notify operator at next heartbeat |
| `critical` | Immediate attention required | Notify operator immediately |

### Signal Type Conventions

Use `noun_verb` format:

- `heartbeat` — agent is alive and ran successfully
- `completed` — specific job finished
- `creative_fatigue` — ad performance dropping below threshold
- `inventory_low` — SKU below reorder point
- `order_stalled` — fulfillment delayed beyond SLA
- `creator_profile_updated` — influencer data refreshed
- `cost_spike` — AI spend above daily threshold

### The Rule

> Bots never call each other. They write to the signal table. The orchestrator reads alert/critical signals on every heartbeat and escalates to the human if needed.

This creates a nervous system, not a phone tree.

---

## 3. The Three-Tier Memory System

The difference between a reactive agent and a predictive one is memory architecture.

### Tier 1: Daily Context (raw log, 14 days)
- File: `memory/YYYY-MM-DD.md`
- Written automatically at end of significant sessions
- Raw, unstructured, chronological
- Purpose: preserve detail that hasn't been synthesized yet
- Retention: 14 days rolling

### Tier 2: Weekly Digest (synthesized intelligence, 90 days)
- File: `memory/week-NN.md`
- Written by the orchestrator at end of each week
- Structured: decisions made, patterns identified, open threads, next week priorities
- Retention: 90 days

### Tier 3: MEMORY.md (permanent, git-persisted)
- The curated brain
- Updated only when something is worth remembering permanently
- Structure: identity layer, business context, key relationships, active projects, architecture decisions
- Committed to git — this is what persists across VPS rebuilds

### How Memory Makes Agents Predictive

Reactive: "The user asked about shipment delays. I'll look that up."

Predictive: "There's a pre-CNY shipment pattern. Storage fees accrue after LFD. I'll check the three open shipments proactively."

The difference is MEMORY.md. An agent with rich memory doesn't wait to be asked — it knows what matters.

---

## 4. Communication Routing

Two channels. Different audiences. Different content.

### Executive Channel (Telegram)
- Alerts and critical signals
- Synthesized summaries — one paragraph, not a log dump
- Approval requests
- Weekly narrative

### Work Floor Channel (Discord)
- Full cron job output
- Detailed reports
- Team visibility (non-operators can read)
- Bot-to-bot coordination signals (human-readable)

### Routing Policy

The rule isn't "which bot sends where." The rule is "who is the intended audience."

- If only the operator needs to act: Telegram
- If the team needs visibility: Discord
- If it's routine completion: Discord only
- If it's a warning or above: Telegram + Discord

### The Channel Config Pattern

Each cron job declares its delivery target explicitly:

```yaml
delivery:
  mode: announce
  channel: telegram  # or discord
```

Never let the bot decide at runtime. Routing is configuration, not logic.

---

## 5. Access Control Model

### Two Tiers

**Read Tier (anyone authorized):**
Query data, get reports, ask questions. No side effects.

**Write Tier (operator only):**
Trigger crons, approve actions, initiate data writes, schedule external communications.

### Behavioral Enforcement

The wrong way: approval gates on every action.

The right way: SOUL.md encodes the operator's intent so deeply that the agent doesn't need to ask for most things.

```markdown
## Boundaries
- Internal actions (reading, organizing, analyzing): be bold
- External actions (emails, messages to humans): confirm before sending
- Financial actions: always confirm, show amount + recipient
- Irreversible actions: always confirm, show what changes
```

Approval gates create friction. Friction kills adoption. Behavioral enforcement creates trust without killing velocity.

### Why This Matters

An AI system that asks for permission for everything gets turned off after two weeks. One that earns trust through consistent, correct behavior becomes load-bearing infrastructure.

---

## 6. The Model Cost Pyramid

Running AI at scale means making deliberate model choices. The wrong model on the wrong job is the most common cause of runaway costs.

### The Pyramid

```
         [Expensive Model]
          Strategic work only
          Never in cron jobs
         ─────────────────────
        [Default Model — Sonnet]
         Most cron work
         Analysis, drafting, synthesis
        ───────────────────────────────
       [Lightweight Model — Haiku/Mini]
        Watchdogs, routers, intent parsing
        High-frequency, low-complexity jobs
       ────────────────────────────────────────
```

### Rules

1. **Watchdogs run Haiku only.** The fleet monitor, cost watchdog, and heartbeat checker should cost less than $1/day combined.

2. **Default cron work runs Sonnet.** Analysis, report generation, content drafting — this is the workhouse layer.

3. **Expensive models (Opus, o3) never run in automated jobs.** Reserve for high-value, human-initiated work only.

4. **One Haiku bot watches all others.** The cost watchdog reads API spend daily and alerts if any bot exceeds threshold. The watchdog itself costs almost nothing.

### The Cost Watchdog Pattern

```
sysbot (Haiku) checks:
  - Daily spend per bot vs. threshold
  - Unexpected model usage (Opus in a cron = alert)
  - Token counts trending up without output quality change
  → Alerts Telegram if any threshold exceeded
```

This pattern paid for itself on day one.

---

## 7. The Programmatic Access Problem

Your AI operating system is only as intelligent as what it can interface with.

Every tool your business uses falls into one of two categories:

**Tier 1: First-class intelligence (CLI or API exists)**
ERP, e-commerce, CRM, ad platforms, marketplaces — these tools become part of the nervous system. Agents read from them, write to them, monitor them.

**Tier 2: Blind spots (no programmatic access)**
Any tool locked behind a UI-only interface. The agent can't read it, can't monitor it, can't learn from it. It's a dead zone in your intelligence layer.

### How to Route Around Closed APIs

When a tool doesn't have an API, look for the email interface:
- Most SaaS tools send email notifications
- Email is parseable — subject lines, structured bodies, attachments
- Build the agent to read the notification, not query the database

This isn't a hack. It's a durable pattern.

### The Competitive Implication

AI-native businesses will consolidate around tools they can interface with programmatically. Tools that remain UI-only will lose enterprise customers to open alternatives — not because of features, but because they can't be integrated into the intelligence layer.

When evaluating new tools: "Does it have an API?" is now a tier-1 requirement, not a nice-to-have.

---

## 8. Operating Rhythm Integration

A fleet of agents generating output is not an operating system. The output has to connect to human decision-making.

### The Weekly Rhythm

**Sunday 7 PM:** Data capture cron runs — pulls metrics from all systems, seeds the weekly summary.

**Monday morning:** Email sends to leadership team — synthesized narrative, not a data dump. Rock status, open issues, one decision needed, one win from last week.

**Monday meeting:** Leadership reviews the email before they meet. The meeting is a decision forum, not a status update.

**Friday:** Scorecard auto-populated. Bots write metrics to shared table. Leadership can see week-over-week trends without anyone compiling a spreadsheet.

### The Email-as-Interface Pattern

The weekly email isn't just a report. It's an interface.

Leadership replies to the email. The reply monitor cron reads replies and routes action items back into the system:
- "Move this to parked" → updates the item status in the database
- "Escalate the blocker" → creates an urgent signal in bot_signals
- "Schedule a call with the vendor" → queues a draft in the communication agent

The executive's inbox becomes a write interface to the operating system. No new tools. No training. Just reply.

### Async Consensus

The highest-leverage pattern in this stack is async consensus.

Before: need alignment → schedule meeting → wait for meeting → discuss → follow up.

After: agent synthesizes the situation → sends structured summary → each exec replies with position → agent detects consensus → meeting agenda is pre-decided.

The meeting becomes a ratification forum, not a deliberation forum. You recover 30–60 minutes per week per leadership meeting.

---

## 9. Deployment Reference

### Infrastructure

**Minimum viable:**
- 1 VPS (2 vCPU, 4GB RAM) — $20–40/month
- Systemd for process management
- Git for configuration version control

**Production (reference implementation):**
- 2 VPS: orchestration layer + studio layer
- Private network (e.g. Tailscale) for secure agent-to-agent networking
- Supabase for shared signal database
- Cloudflare for dashboard hosting

### Cost Model

| Layer | Monthly cost |
|-------|-------------|
| Infrastructure (2 VPS) | ~$60 |
| AI API (10 agents, 100+ crons) | $350–550 |
| Database (Supabase) | $25 |
| **Total** | **~$450/month** |

Equivalent capability from an enterprise AI platform: $5,000–15,000/month.

### Security Checklist

- [ ] Row-level security enabled on all shared database tables
- [ ] Agent tokens scoped to minimum required permissions
- [ ] No credentials in SOUL.md or public files
- [ ] Write-tier actions require operator authorization
- [ ] Cost watchdog running and alerting
- [ ] Token health check cron running weekly
- [ ] External communication (email, messages) requires confirmation before send

### Fork Instructions

1. Clone this repo
2. Copy `.env.example` to `.env` — fill in your API keys
3. Edit `SOUL.md` — this is your agent's identity, make it yours
4. Edit `MEMORY.md` — seed your business context
5. Edit `HEARTBEAT.md` — configure your operating rhythm
6. Deploy to a VPS with `systemd` or your preferred process manager
7. Add skills for your specific toolset: `npx skills add <skill>`

---

## Appendix: What Changed From v1

The original jbot Protocol (2025) established 6 pillars: memory, skills, voice, security, fleet, and rhythm. Those pillars were right. The implementation has matured significantly.

| Old assumption | New understanding |
|---|---|
| Human approval gates on all external actions | Behavioral enforcement via SOUL.md — fewer gates, more trust |
| Single orchestrator handles everything | Orchestrator + specialized fleet + shared signal layer |
| Skills as optional add-ons | Skills as primary capability expansion path |
| Supabase as "state storage" | Supabase as nervous system — the signal layer everything runs through |
| Memory as a feature | Three-tier memory as a core architectural primitive |
| Voice as a novelty | Voice as a first-class access channel (ElevenLabs + Twilio) |

One early lesson: a misconfigured agent running an expensive model on a high-frequency cron caused a significant cost spike in a single day. The cost watchdog pattern (Section 6) was built directly from that incident. Always run a watchdog. Always.

---

*Built through work, not configuration. Reference implementation: github.com/jabondano/jbot-protocol*
