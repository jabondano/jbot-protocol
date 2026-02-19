# Industry Templates

**Pre-built JBOT Protocol configurations for common verticals**

---

## What Are Industry Templates?

Industry templates are ready-to-customize starting points for deploying the JBOT Protocol in specific verticals. Each template provides:

- **Industry-specific pain points** and how AI agents address them
- **Bot fleet design** with model-tier recommendations (Haiku/Sonnet/Opus)
- **System integration maps** showing MCP connections to common industry tools
- **Quick wins** achievable in the first 30 days
- **ROI estimates** based on typical operational benchmarks
- **Implementation phases** with realistic timelines

These are **consulting deliverables** — practical, actionable blueprints that a team can start executing in week one. They are not academic frameworks or theoretical exercises.

---

## Available Templates

| Template | Industry | Key Challenge | Bot Count | Timeline |
|----------|----------|---------------|:---------:|:--------:|
| [Real Estate Brokerage](real-estate-brokerage.md) | Residential real estate | Speed-to-lead response, agent coordination, transaction management | 5 | 90 days |
| [Import/Export Distribution](import-export-distribution.md) | Logistics & distribution | Cross-timezone supplier coordination, field team management, multi-warehouse inventory | 4 | 90 days |
| [Manufacturing Operations](manufacturing-operations.md) | Vertically integrated manufacturing | Cross-layer data synthesis, production monitoring, capital expenditure justification | 5 | 120 days |

---

## How Templates Map to the 6 Pillars

Every template is structured around the core JBOT Protocol pillars, with industry-specific adaptations:

| Pillar | How It Appears in Templates |
|--------|----------------------------|
| **1. Division Architecture** | Bot fleet design — which agents cover which functions, model tier selection |
| **2. Knowledge Capture** | Industry context section — the domain knowledge each bot needs to encode |
| **3. Tool Integration** | System integration map — MCP connections to industry-standard tools |
| **4. Governance** | Built into implementation phases — read-only first, write access earned |
| **5. Change Management** | Quick wins section — early victories that build organizational buy-in |
| **6. Measurement & ROI** | ROI estimate section — quantified value with conservative assumptions |

---

## Customization Guidance

These templates are **starting points, not rigid prescriptions**. Every organization is different. Here is how to adapt them:

### What to Keep As-Is
- **Bot role definitions** — The functional splits (lead management, marketing, operations) are universal within each industry
- **Implementation sequencing** — The phased approach (read-only first, quick wins early) is proven across deployments
- **Governance patterns** — Human-in-the-loop requirements are non-negotiable in early phases

### What to Customize
- **Bot count** — Smaller organizations may combine roles (e.g., merge LeadBot and ClientBot in a 5-agent real estate brokerage). Larger ones may split them further
- **Model tiers** — The Haiku/Sonnet/Opus recommendations balance cost and capability. Adjust based on task complexity and budget
- **System integrations** — Swap in whatever CRM, ERP, or project tool your client uses. The MCP patterns are system-agnostic
- **Quick wins** — Prioritize based on the client's loudest pain point. The template order is a suggestion, not a mandate
- **Timeline** — The 90/120-day timelines assume a mid-size organization with moderate technical maturity. Adjust up or down accordingly

### Common Modifications

| Scenario | Modification |
|----------|-------------|
| Client has no CRM | Add CRM selection and setup to Phase 1; push bot deployment to Phase 2 |
| Client has existing automation | Audit current automations first; integrate rather than replace |
| Client is risk-averse | Extend the read-only phase; add more human review checkpoints |
| Client has strong technical team | Compress timelines; let their team handle integrations directly |
| Budget constraints | Start with 2 bots (highest-ROI functions); expand after proving value |

---

## Creating New Templates

To build a template for an industry not covered here, follow this structure:

1. **Industry Context & Pain Points** — What keeps operators up at night?
2. **Bot Fleet Design** — Which agents, what model tier, what schedules?
3. **System Integration Map** — What tools does this industry use? How do they connect?
4. **Quick Wins (First 30 Days)** — What can we deliver fast to build trust?
5. **ROI Estimate** — Conservative math that justifies the investment
6. **Implementation Phases** — Realistic timeline with clear milestones
7. **Key Customizations vs Core Framework** — What is unique about this vertical?

Include a Mermaid diagram for the bot fleet architecture. Keep the total length between 200-300 lines. Write for a reader who understands their industry but may be new to AI agent deployment.

---

## Related Resources

- [JBOT Protocol — The 6 Pillars](../framework/)
- [Getting Started Guide](../guides/getting-started.md)
- [Division Template](../templates/DIVISION.md)
- [Agent Skill Template](../templates/AGENT-SKILL.md)
- [Case Studies](../case-studies/)
