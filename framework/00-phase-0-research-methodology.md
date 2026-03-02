# Phase 0: Research Methodology

**Before you deploy a single agent, understand the world you're deploying into.**

This phase happens before Pillar 1. It typically takes 1–2 weeks and produces one output: a Gap Map that tells you which processes are ready for AI and which ones will break it.

Skip this phase and you'll automate the wrong things first.

---

## Why Phase 0 Exists

The discovery interview in `/templates/discovery-interview.md` gives you the operator's inside-out view. That's necessary but not sufficient. Without external benchmarks, you don't know if you're ahead of or behind the peer group. Without structured internal observation, you miss the shadow processes — the real workflows that differ from the documented ones.

Phase 0 has two parts. Do them in order.

---

## Part A: External Research

External research answers one question: **What does "good" look like in this sector?**

You're looking for how organizations of similar size and complexity are using AI today — what they've automated, what they haven't, where they've failed, and where the tooling is mature enough to trust.

### A.1 Research Levels

Work from large to small. The largest companies set the ceiling. The 5-person operators tell you what's actually practical.

| Level | Who | What You Learn |
|---|---|---|
| **Enterprise (1,000+ employees)** | Public companies in the sector | What's possible at scale; where the tooling has matured; where AI is already ROI-positive |
| **Mid-market (50–1,000 employees)** | Private companies, trade press case studies | What mid-tier operators are actually doing; common implementation patterns; realistic timelines |
| **SMB (5–50 employees)** | Operators like your client | What's achievable without a dedicated AI team; which off-the-shelf tools are gaining adoption |

### A.2 Research Sources by Type

**Public company filings and earnings calls**
- 10-K and 10-Q filings: look for "automation," "AI," "workforce efficiency," "technology investment"
- Earnings call transcripts: executives often name specific tools and outcomes
- Investor day presentations: forward-looking operational strategy

**Industry research and analyst reports**
- McKinsey Global Institute: sector-specific AI adoption studies
- Deloitte Insights: technology in operations benchmarks
- Gartner: technology adoption curves and vendor landscapes (use for tool maturity, not vendor selection)
- Harvard Business Review: practitioner-focused case studies
- MIT Sloan Management Review: research-backed operational AI coverage

**Trade and vertical press**
- Sector-specific publications (e.g., Supply Chain Dive for logistics, Hotel Management for hospitality)
- LinkedIn articles by operations leaders in the sector
- G2, Capterra, and Trustpilot reviews of tools commonly used in the sector

**Academic sources**
- Google Scholar: search "[sector] + workflow automation + empirical" for peer-reviewed evidence
- SSRN: working papers on operational AI before they're published
- MIT CISR: IT and operations intersection research

### A.3 Sector Research Template

For each organization you benchmark, fill out this template:

| Field | Description |
|---|---|
| **Company name** | Public record or anonymized (e.g., "Regional freight broker, ~$40M revenue") |
| **Company size** | Revenue range, employee count |
| **Industry / sub-vertical** | Be specific (e.g., "B2B SaaS with field sales" not just "software") |
| **Current tools** | ERP, CRM, project management, communication, analytics |
| **Automation maturity** | 1 = manual only, 2 = some RPA/macros, 3 = workflow automation, 4 = AI-assisted, 5 = AI-native |
| **Known AI use cases** | What specifically have they automated? Source your claim. |
| **AI readiness score** | 1–5 composite (data quality + tooling + leadership buy-in + talent) |
| **Source** | Where did you find this? (filing, article, interview, case study) |

**AI Readiness Score rubric:**

| Score | Interpretation |
|---|---|
| 1 | No structured data; all processes manual; no internal AI appetite |
| 2 | Some data in systems but inconsistent; isolated AI experiments; mixed leadership buy-in |
| 3 | Clean data in major systems; at least one successful AI pilot; leadership interested |
| 4 | Integrated data across systems; multiple AI use cases in production; dedicated owner |
| 5 | Data as a strategic asset; AI embedded in core processes; AI literacy across org |

### A.4 Suggested Research Sources by Vertical

| Vertical | Key Research Sources |
|---|---|
| **Retail / E-commerce** | Digital Commerce 360, eMarketer, Shopify Commerce Trends, NRF Retail Tech research |
| **Manufacturing** | IndustryWeek, Manufacturing Today, Deloitte Manufacturing Study, MIT Digital Economy |
| **Logistics / Freight** | Supply Chain Dive, Freightos research, FreightWaves, McKinsey Supply Chain report |
| **Real Estate** | NAR Technology Survey, PropTech Insider, CBRE Technology Outlook |
| **Healthcare** | Health Affairs, NEJM Catalyst, HIMSS Analytics, Definitive Healthcare |
| **Professional Services** | ALM Intelligence (legal), Accounting Today (finance), McKinsey Professional Services |
| **Hospitality** | Hotel Management, Skift Research, American Hotel & Lodging Association Tech Study |
| **Financial Services** | American Banker, Celent Research, Aite-Novarica Group, FINRA AI in Finance |

---

## Part B: Internal Research

Internal research answers a different question: **What is actually happening inside this organization, today?**

You're looking for the real processes — not the documented ones. You're mapping tools, data flows, decision authority, and org connections. You're finding the shadow processes that live in spreadsheets, email threads, and the institutional memory of two or three key people.

### B.1 Operator Interviews

Start with the discovery interview template in `/templates/discovery-interview.md`. It's solid. Add these questions to it:

**Process reality check:**
- "Walk me through the last time this process broke. What went wrong, who noticed, and how was it fixed?"
- "If you had to replace yourself for a month, what would the first week of your handoff notes say?"
- "What do you do on Monday morning before you do anything else?"

**Data and tooling:**
- "When you need to make a decision, what do you look at? Where does that information live?"
- "What data would you love to have that you currently don't?"
- "Which of your tools talk to each other? Which ones should but don't?"

**AI readiness signals:**
- "Has anyone here tried using ChatGPT or similar for work tasks? What did they try? What happened?"
- "What's the most repetitive thing a skilled person on your team does every week?"
- "Is there a report or summary that someone creates manually every week that you rely on?"

### B.2 Workflow Recording

Interview-only research misses what experts do automatically. Supplement with direct observation.

**Screen recording:** Ask 2–3 key operators to record themselves doing a representative task while narrating. Tools like Loom (async) or Scribe (auto-capture steps) work well. You're looking for the clicks that happen automatically — the tabs they switch to without thinking, the fields they always check, the manual copy-paste that bridges two systems.

**Shadow process identification:** Compare documented SOPs against what you observe. Where they diverge, you've found tribal knowledge. That divergence is what the agent needs to learn.

**Time logging:** Ask operators to log where their time goes for one week using a simple spreadsheet (task, time spent, frequency). This gives you quantitative data to prioritize automation targets.

### B.3 Tool Inventory

Document every software system the organization uses. For each tool, capture:

| Field | Description |
|---|---|
| **Tool name** | Product name and category |
| **Primary users** | Who uses it and how often |
| **Data stored** | What lives in this system |
| **Integration status** | Does it have an API? MCP server? Webhook support? |
| **Data quality** | 1 (garbage) to 5 (clean, structured, current) |
| **AI-accessible?** | Can an agent read from or write to this system today? |

### B.4 Org Connection Mapping

Map the informal information flows — not the org chart, but the actual network of who talks to whom, who has context that others don't, and where information gets stuck.

For each key process, draw:
- Who initiates it (the trigger)
- Who does the work (the executor)
- Who reviews or approves (the checkpoint)
- Who receives the output (the consumer)
- Where the output goes next (the downstream dependency)

The gaps in this map — places where information transfers manually, where one person is a bottleneck, where the consumer doesn't know the trigger happened — are your highest-value automation opportunities.

---

## Part C: The Gap Map

The Gap Map is the output of Phase 0. It's a simple prioritization matrix that classifies processes by two dimensions:

1. **AI-readiness:** How structured is the data, how well-defined is the process, how available are the tools?
2. **Business impact:** How much time, money, or risk does this process represent?

### Gap Map Template

| Process | Division | Frequency | Time Spent/Week | AI-Readiness (1–5) | Business Impact (1–5) | Quadrant | Notes |
|---|---|---|---|---|---|---|---|
| Daily sales report | Sales | Daily | 3 hrs | 4 | 5 | **Quick Win** | Clean data in CRM; report format is fixed |
| Customer complaint routing | Support | Continuous | 2 hrs | 3 | 4 | **Quick Win** | Rules-based; needs clear escalation logic |
| Freight rate negotiation | Ops | Weekly | 5 hrs | 2 | 5 | **High Value / Not Yet** | Requires judgment; data is fragmented |
| Vendor payment approval | Finance | Monthly | 1 hr | 5 | 3 | **Quick Win** | Fully structured; low stakes per transaction |
| Strategic account review | Sales | Monthly | 4 hrs | 2 | 5 | **Human-Led** | Requires relationship context; do not automate |

### Quadrant Definitions

```
                    High Impact
                         |
    Human-Led            |        Quick Win
    (high impact,        |        (high impact,
     AI not ready)       |         AI-ready)
                         |
─────────────────────────┼─────────────────────────
                         |
    Deprioritize         |        Automate First
    (low impact,         |        (low impact,
     AI not ready)       |         AI-ready)
                         |
                    Low Impact
```

- **Quick Win:** Start here. High impact, AI-ready. These become your first wave of agents.
- **Automate First:** High AI-readiness, lower impact. Use these to build confidence and establish patterns before tackling higher-stakes processes.
- **Human-Led:** High impact but AI isn't ready or appropriate. Flag these for Phase 0 revisit in 6–12 months.
- **Deprioritize:** Low impact, low readiness. Don't touch these in the first deployment cycle.

### AI-Readiness Criteria (score 1–5)

A process scores higher when:
- Data inputs are structured (not in emails, PDFs, or phone calls)
- The process has clear start and end conditions
- Decisions follow rules that can be documented
- Tools involved have APIs or MCP servers
- The output has a consistent format
- Edge cases are rare and well-understood

A process scores lower when:
- Key inputs are unstructured (conversation, negotiation, intuition)
- The process requires ongoing relationship context
- Exceptions are common and consequential
- The tooling has no integration path
- Output format varies by situation

---

## Phase 0 Deliverables Checklist

- [ ] Sector Research: 5–10 benchmark companies documented in the sector research template
- [ ] Sector Research: Key AI-ready workflows identified for this vertical
- [ ] Internal Research: Discovery interview completed with 2–3 key operators
- [ ] Internal Research: Tool inventory documented (all systems, API status, data quality)
- [ ] Internal Research: Org connection map for top 5 processes
- [ ] Internal Research: One week of time logging data from at least one operator
- [ ] Gap Map: All significant processes classified by quadrant
- [ ] Gap Map: "Quick Wins" ranked and approved by operator for first deployment wave

**Estimated time investment:** 8–16 hours for a single operator / small team. This is the highest-ROI time you'll spend on the engagement.

---

*Phase 0 feeds directly into Pillar 1 (Division Architecture). Your Gap Map's Quick Wins become the first agents. The Human-Led processes inform where human supervision remains mandatory.*
