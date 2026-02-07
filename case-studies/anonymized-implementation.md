# Case Study: AI-Native Operations at a Public Hardware Company

**Anonymized implementation of the JBOT Protocol**

---

## Company Profile

| Attribute | Detail |
|-----------|--------|
| **Industry** | Consumer electronics / wearable technology |
| **Size** | 30-50 employees |
| **Revenue** | ~$3-5M annually |
| **Listing** | Publicly traded (US exchange) |
| **Divisions** | 7 operating divisions |
| **Challenge** | Scale operations without proportional headcount growth |

## The Challenge

The company needed to grow revenue 2-3x while keeping headcount nearly flat. With a lean team spanning customer support, sales, marketing, design, supply chain, apps development, and finance, every person was already stretched thin. Key pain points:

1. **Reporting bottleneck** — The COO spent 2+ hours daily compiling cross-division status updates manually
2. **Late order visibility** — No automated alerting for orders falling behind fulfillment SLAs
3. **Tribal knowledge risk** — Critical processes existed only in the heads of 2-3 key employees
4. **Cross-system blind spots** — Data lived in 8+ systems (ERP, e-commerce, CRM, project tracking) with no unified view
5. **China manufacturing coordination** — 12-13 hour time zone gap meant communication delays compounded daily

## The Approach: JBOT Protocol Implementation

### Phase 1: Division Architecture (Week 1-2)

Mapped all 7 divisions and identified agent deployment opportunities:

| Division | Key Agent Tasks | Priority |
|----------|----------------|:--------:|
| Fulfillment | Late order alerts, inventory monitoring | High |
| Finance | Revenue reporting, cash flow alerts | High |
| Marketing | Campaign performance, content scheduling | Medium |
| Sales | Pipeline tracking, lead scoring | Medium |
| Customer Support | Ticket categorization, response drafting | Medium |
| Supply Chain | PO tracking, vendor communication drafts | Low |
| Design | Brief management, asset organization | Low |

**Architecture chosen:** Hub and Spoke — COO as central coordinator with division-specific agents.

### Phase 2: Knowledge Capture (Week 2-4)

Documented the top 3 processes per high-priority division:

- **Fulfillment:** Late order escalation, inventory reorder triggers, carrier selection criteria
- **Finance:** Revenue reporting filters (specific transaction types, line item rules), board metrics compilation, cash flow monitoring thresholds
- **Customer Support:** Ticket priority matrix, common issue resolution paths, escalation criteria

**Key insight:** The most valuable knowledge captured wasn't in SOPs — it was the **exception handling logic** that experienced employees applied automatically. For example, the fulfillment lead had unwritten rules about which wholesale accounts get expedited shipping on late orders.

### Phase 3: Tool Integration (Week 3-5)

Connected agents to business systems via MCP:

| System | Integration | Access Level |
|--------|------------|:------------:|
| E-commerce platform | MCP server | Read-only |
| ERP | Custom MCP server | Read-only (SQL queries) |
| Project tracking | MCP server | Read-only |
| Team chat | MCP server | Read + Post (specific channels) |
| CRM | Via middleware | Read-only |

**Governance rule:** All integrations started read-only. Write access (posting to chat channels) was added after 2 weeks of successful read operations with human review.

### Phase 4: Deployment (Week 4-6)

Deployed 4 specialized agents using the two-server topology:

| Agent | Division | Primary Tasks |
|-------|----------|--------------|
| Executive assistant | Cross-division | Morning briefing, delegation, synthesis |
| Fulfillment agent | Operations | Late order alerts, inventory monitoring |
| Marketing agent | Marketing | Campaign metrics, content calendar |
| Finance agent | Finance | Revenue dashboards, board report data |

## Results

### Quantitative Outcomes (First 90 Days)

| Metric | Before | After | Change |
|--------|--------|-------|:------:|
| Daily reporting time (COO) | 2.5 hrs | 15 min | **-90%** |
| Late order detection | End of day (manual) | Real-time (automated) | **Same-day alerts** |
| Board report compilation | 3 days | 4 hours | **-83%** |
| Cross-division visibility | Weekly meetings | Daily automated briefings | **Daily** |
| Documented processes | ~15 SOPs | 45+ SOPs | **3x** |
| Monthly API cost | N/A | ~$65 | **Minimal** |

### Human-Agent Ratio (HAR) Progression

| Month | Fulfillment | Finance | Marketing | Sales |
|-------|:-----------:|:-------:|:---------:|:-----:|
| Month 1 | 0.3 | 0.2 | 0.1 | 0.0 |
| Month 2 | 1.2 | 0.8 | 0.5 | 0.3 |
| Month 3 | 2.5 | 1.5 | 1.0 | 0.8 |

### Qualitative Outcomes

- **COO freed up 10+ hours/week** for strategic work instead of manual data compilation
- **Zero missed late orders** in the first 60 days (vs. ~5/month previously discovered after customer complaints)
- **Faster onboarding** — new hires could read documented SOPs instead of shadowing for weeks
- **Better board reporting** — consistent, data-backed reports delivered on schedule every month

## Lessons Learned

### What Worked

1. **Starting read-only was critical.** The team built trust in the agents by seeing accurate data retrieval before granting any write access.
2. **Division ownership, not IT ownership.** Each division leader "owned" their agent and could request changes directly. This drove adoption faster than a top-down IT rollout.
3. **The "split the bot" moment.** The initial single operations bot was replaced with 4 specialized agents within the first month. Quality improved immediately.
4. **Exception documentation was the gold mine.** The 20% of processes that handled edge cases delivered 80% of the value.

### What We'd Do Differently

1. **Capture knowledge earlier.** We started building agents before fully documenting processes. This caused rework when the agents had incomplete instructions.
2. **Set baseline metrics on day one.** We didn't measure "before" carefully enough, making it harder to quantify ROI precisely for the board.
3. **Invest in dashboarding sooner.** Agents generated insights that initially lived in chat messages. Building a proper dashboard took an extra month but should have been parallel.
4. **Plan for the China time zone.** Supply chain agents needed scheduling aligned with manufacturing hours (9pm-6am ET), which wasn't in the initial design.

### Unexpected Benefits

- **Documentation culture shift** — The team started proactively documenting processes because they saw how it directly improved agent quality
- **Cross-division awareness** — The daily briefing revealed dependencies that the team hadn't explicitly acknowledged
- **Vendor communication quality** — Drafting tools helped standardize and improve communications with overseas manufacturing partners

---

## Key Takeaway

> The biggest ROI didn't come from the AI technology itself — it came from the **knowledge capture process**. Forcing the organization to document its tribal knowledge created value that would persist even if the agents were turned off.

---

## Related Resources

- [Division Architecture](../framework/01-division-architecture.md) — Hub and Spoke pattern used here
- [Knowledge Capture](../framework/02-knowledge-capture.md) — Techniques applied
- [Measurement & ROI](../framework/06-measurement-roi.md) — How results were tracked
