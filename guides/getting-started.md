# Getting Started with JBOT Protocol

**From zero to first agent in 30 minutes**

---

## Prerequisites

Before you start, you need:

- [ ] A clear understanding of one division you want to augment
- [ ] Access to at least one business system (CRM, ERP, e-commerce, etc.)
- [ ] A Claude account (Pro, Team, or Enterprise)
- [ ] 30 uninterrupted minutes

## The 30-Minute Quick Start

### Step 1: Read the Framework (5 minutes)

Skim the [README](../README.md) and understand the Six Pillars:

1. **Division Architecture** — Where agents fit in your org
2. **Knowledge Capture** — Documenting what your team knows
3. **Tool Integration** — Connecting agents to your systems
4. **Governance** — Rules and guardrails
5. **Change Management** — Getting your team on board
6. **Measurement & ROI** — Proving the value

You don't need to master all six. Start with the first three.

### Step 2: Fill Out a Division Template (10 minutes)

Copy [templates/DIVISION.md](../templates/DIVISION.md) and fill it out for your target division.

**Focus on these sections only:**

1. **Overview** — Mission and 3-5 key metrics
2. **Core Processes** — List the top 5 recurring processes (don't document them in detail yet)
3. **Systems & Tools** — What software does this division use daily?

**Example (Fulfillment Division):**

| Field | Value |
|-------|-------|
| Mission | Ensure orders ship on time and inventory stays stocked |
| Key Metrics | Orders shipped/day, days to fulfill, late order rate |
| Top Processes | Order fulfillment, inventory monitoring, late order escalation, returns processing, carrier selection |
| Systems | Shopify, NetSuite, ShipStation, Slack |

### Step 3: Document Your Top 3 Processes (10 minutes)

Don't use the full [PROCESSES.md](../templates/PROCESSES.md) template yet. Instead, write a simple version of your 3 most repetitive processes:

```markdown
## Process: [Name]

**Trigger:** [What starts this process]
**Steps:**
1. [First thing you do]
2. [Second thing you do]
3. [Continue until done]

**Exceptions:**
- If [this happens], then [do this instead]

**Escalation:**
- If [this condition], notify [this person]
```

**Tip:** The exceptions and escalation rules are the most valuable part. Agents can figure out the happy path — it's the edge cases where they need your expertise.

### Step 4: Choose Your Implementation Path (3 minutes)

| Path | Best For | Guide |
|------|----------|-------|
| **Claude Code** | Teams already using Claude Code CLI; want fast setup with CLAUDE.md files | [Claude Code Guide](../implementation/claude-code-guide.md) |
| **OpenClaw** | Self-hosted fleet; need Telegram/phone integration; multiple persistent agents | [OpenClaw Guide](../implementation/openclaw-fleet.md) |
| **Claude Projects** | Simplest start; no infrastructure; single-division pilot | Create a Claude Project with your Division + Process docs as context |

**Recommendation for first-timers:** Start with a Claude Project. Paste your division overview and top 3 processes into a Claude Project's custom instructions. You'll have a working agent in minutes.

### Step 5: Start Read-Only (2 minutes)

Whatever path you choose, your first agent action should be **reading data, not writing it**.

Good first tasks:
- "Summarize today's unfulfilled orders"
- "What are our top 5 selling products this month?"
- "List support tickets opened in the last 24 hours"

Bad first tasks:
- "Send an email to all customers with late orders"
- "Update the CRM with these new lead scores"
- "Post the daily summary to Slack"

Write access comes after you've verified the agent understands your data correctly.

---

## Week-by-Week Expansion

### Week 1: Single Division Pilot

- [ ] Division template filled out
- [ ] Top 3 processes documented
- [ ] Agent deployed (read-only)
- [ ] Daily check: Is the agent's output accurate?

### Week 2: Add Governance

- [ ] Define which actions need human approval
- [ ] Set up audit logging (even if it's just saving chat transcripts)
- [ ] Promote one read-only integration to assisted actions (e.g., drafting Slack messages for your review)
- [ ] Document the first exception the agent couldn't handle

### Week 3: Measure

- [ ] Establish baseline metrics (time spent before vs. after)
- [ ] Track agent task completion rate
- [ ] Survey team members: Is this helpful?
- [ ] Calculate first-month ROI estimate using the [ROI framework](../framework/06-measurement-roi.md)

### Week 4: Decide to Expand or Iterate

- [ ] Review lessons learned
- [ ] If successful: pick the next division
- [ ] If struggling: refine process documentation and governance rules
- [ ] Share results with stakeholders

---

## Common First-Week Mistakes

| Mistake | Why It Happens | What to Do Instead |
|---------|---------------|-------------------|
| Trying to automate everything at once | Excitement about the technology | Pick 1 division, 3 processes |
| Skipping knowledge capture | "The agent is smart, it'll figure it out" | Document exceptions and edge cases — that's where value lives |
| Giving write access immediately | "I trust AI" | Start read-only. Promote after 2 weeks of accurate reads |
| No baseline metrics | "We'll measure later" | Record how long tasks take *before* the agent starts |
| Building for the org, not with the org | IT-driven rollout | Division owners must own their agents |

---

## Next Steps

Once you've completed the 30-minute quick start:

1. Read the full [Division Architecture](../framework/01-division-architecture.md) pillar to understand organizational patterns
2. Deep-dive into [Knowledge Capture](../framework/02-knowledge-capture.md) to systematically document your team's expertise
3. Review the [Case Study](../case-studies/anonymized-implementation.md) to see how a real company implemented this
4. Set up the [Measurement & ROI](../framework/06-measurement-roi.md) framework to prove value to your stakeholders

---

*Questions? Open an issue on the [GitHub repository](https://github.com/jabondano/jbot-protocol/issues).*
