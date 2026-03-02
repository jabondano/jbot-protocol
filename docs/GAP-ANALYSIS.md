# Gap Analysis: jbot-protocol v2.x vs. Production-Proven Patterns

**Audience:** Protocol authors, senior contributors  
**Purpose:** Identify what's missing, what needs updating, and what's holding strong after 2 months of live fleet operations at a 10-bot scale.

---

## Executive Summary

The 6-pillar framework is conceptually sound and operationally useful. The core ideas — embedded agents, knowledge capture before deployment, governance by oversight pattern, change management investment ratio — hold up under production conditions.

What's missing is the *before* and the *beneath*:

- **Before deployment:** There's no structured methodology for researching the sector or understanding the operator's reality before writing a single line of agent code. Discovery is reduced to a single interview template, which isn't enough.
- **Beneath the agent:** There's no explicit architecture for what makes an individual bot a reliable, persistent, character-consistent unit. The three-file pattern (SOUL + MEMORY + schedule) was discovered in production and isn't in the protocol.

Additionally, two governance subsystems — cost discipline and kill hierarchy — exist in fragments across the framework but need to be elevated to first-class concepts.

---

## Gap-by-Gap Analysis

### GAP 1 (Critical): No Pre-Deployment Research Phase

**Current state:** The framework begins with Division Architecture (Pillar 1). There is a `discovery-interview.md` template with good questions, but no structured methodology for *sector-level* research before the operator interview, and no output format for capturing what was learned.

**What's missing:**
- External research methodology: industry benchmarks, public company analysis, vertical-specific tooling patterns
- Internal research methodology: structured observation, tool mapping, org connection mapping
- A formal output — the "Gap Map" — that classifies which processes are AI-ready vs. which will break it

**Impact:** Deployments that skip this phase tend to automate the wrong things first. Without external benchmarks, there's no way to know if the operator is ahead of or behind their peer group.

**Recommended fix:** Add Phase 0 as a formal pre-pillar research stage.

---

### GAP 2 (Critical): No Individual Bot Identity Architecture

**Current state:** SOUL.md is mentioned in Pillar 1 (Section 1.3) as a pattern — roughly 150 words. MEMORY.md doesn't exist in the framework. Cron scheduling is mentioned in passing in implementation guides.

**What's missing:** The three-file architecture as an explicit, mandatory, first-class pattern:
1. **SOUL.md** — operating constitution: purpose, domain, what it owns, what it never touches, personality, escalation rules
2. **MEMORY.md** — persistent context: contacts, historical signals, what the bot learned from previous runs
3. **jobs.json / schedule** — cron schedule: recurring tasks that run without being prompted

Without SOUL.md, bots drift. Without MEMORY.md, every session starts from zero. Without a schedule, the bot isn't operational — it's a chat interface.

**Also missing:** Character design principles — how to land in the "lovable machine" zone between robotic and human-mimicking.

**Recommended fix:** Add Pillar 7 — Agent Identity & Human-Machine Trust.

---

### GAP 3 (High): Cost Discipline as Governance, Not Optimization

**Current state:** Model selection is an implementation guide and a brief table in Pillar 1. It's framed as optimization.

**What's missing:** The reframing. Cost discipline is *governance*. When every cron job has an explicit model assignment, you are making a governance decision: this task is not allowed to use the most expensive model. Without that assignment, costs compound and the fleet becomes unsustainable.

**Recommended fix:** Elevate model assignment to a policy requirement in Pillar 4 (Governance). Document it in jobs.json alongside the cron schedule. Add token budget as a governance KPI.

---

### GAP 4 (High): Kill Hierarchy Is Incomplete

**Current state:** Pillar 4 has a solid escalation framework and Fleet Watchdog pattern. No kill procedures.

**What's missing:**
- **Level 1 Kill:** Isolate a single bot (stop service, revoke credentials, preserve logs)
- **Level 2 Kill:** Stop scheduled execution fleet-wide without tearing down infrastructure
- **Level 3 Kill (Nuclear):** Stop everything, rotate all credentials, preserve state for post-mortem

Kill procedures are trust infrastructure. Teams that haven't rehearsed a kill will hesitate when something goes wrong.

**Recommended fix:** Add "Kill Hierarchy" section to Pillar 4 with per-level checklist and decision criteria.

---

### GAP 5 (Medium): The Lattice (Multi-Agent Signal Sharing) Is Underdocumented

**Current state:** The shared database is described as "the nervous system" in Pillar 6. Agent-to-agent arrows appear in Pillar 1 diagrams. The specific signal-sharing pattern is never made explicit.

**What's missing:**
- The `bot_signals` table pattern (schema, signal types, consumption model)
- How a fleet monitor reads health signals from all bots
- How operational bots write signals that compound organizational intelligence over time

**Recommended fix:** Add "The Lattice" section to Pillar 1 or Pillar 6. Describe the signal table pattern and compound intelligence model.

---

### GAP 6 (Medium): Sector Research Depth Is Near-Zero

**Current state:** Three industry templates exist — deployment blueprints. Useful for "how to deploy" but not "what to research before starting."

**What's missing:** Per-sector research starting points — benchmark companies, typical AI-ready workflows, common tool stacks, relevant research sources, and readiness red flags.

**Recommended fix:** Add `sector-research-starter.md` covering 8 sectors.

---

## What's Strong — Don't Touch

| Element | Why It Holds |
|---|---|
| Embedded agents over centralized AI dept | Confirmed. Division ownership drives adoption. |
| Integration spectrum (read-only → full autonomy) | Confirmed. Starting read-only is the single most important trust-building move. |
| 3:1 change management investment ratio | Confirmed. Underinvestment here is why pilots stall. |
| "Silent if Nothing" principle | Confirmed. Alert fatigue kills operator trust within weeks. |
| Fleet Health Score (5-factor composite) | Strong. Recommend adding "cost tier compliance" as a 6th factor. |
| Data freshness pre-check | Confirmed. Stale data in reports is worse than no reports. |
| Pizza team sizing / ~20-task split threshold | Confirmed. Split early. |
| Human-Agent Ratio (HAR) | Good metric. Pair with "cost per output" for sustainability picture. |

---

## Recommended Protocol Update Roadmap

| Priority | Change | Location |
|---|---|---|
| P0 | Add Phase 0 — Research Methodology (pre-pillar) | New file |
| P0 | Add Pillar 7 — Agent Identity & Human-Machine Trust | New file |
| P1 | Expand Pillar 4: Kill Hierarchy | `04-governance.md` |
| P1 | Elevate cost discipline to governance policy in Pillar 4 | `04-governance.md` |
| P2 | Add Lattice signal-sharing pattern to Pillar 1 or 6 | `01-division-architecture.md` |
| P2 | Add MEMORY.md and jobs.json templates | `/templates/` |
| P3 | Add sector research starter pack | New file |
| P3 | Expand industry templates to 8 sectors | `industry-templates/` |

---

*Last updated: 2026-03-01 | Protocol version targeted: v2.2.x*
