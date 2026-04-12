# JBOT Protocol
*Configurable AI operations systems that adapt to your business*

---

## What This Is

A collection of **pre-built, production-tested AI systems** that can be configured for any company through an intake methodology.

**Not tools. Not prompts. Complete operating systems.**

Each system has been deployed at a real company, extracted into universal patterns, and packaged with configuration schemas so you can deploy it at yours.

---

## The Problem With "AI Implementations"

Most companies approach AI like they approach software:
1. Buy a tool
2. Configure it once
3. Use it forever

**That's backwards.**

The real value isn't in the tool. It's in the **methodology you develop through repeated deployments** — and how each deployment teaches you something that makes the next one faster, cheaper, and more valuable.

---

## The JBOT Approach

### Four Layers

**Layer 1: JBOT OS (Infrastructure)**
- OpenClaw runtime (or equivalent agent execution engine)
- Specialized bot fleet (sales, marketing, ops, fulfillment, etc.)
- Shared data layer (Supabase, PostgreSQL, etc.)
- Cron scheduling + async execution
- Channel routing (Discord, Telegram, Slack, etc.)

**Layer 2: JBOT Protocol (Methodology)**
- Operator mindset (execute-first, proactive, match energy)
- Deployment pattern (discovery → specialization → coordination)
- Shared brain architecture (bot-to-bot signaling)
- Approval governance (read vs write tiers)
- Safety controls (kill switch, cost watchdog, rate limits)

**Layer 3: Business Context (Configuration)**
- Intake methodology (questions that configure systems)
- Company-specific parameters (brands, channels, approvers, budgets)
- Adaptation logic (universal systems → company-specific behavior)

**Layer 4: JBOT Systems (Implementations)**
- Pre-built solutions (Static Content Engine, Marketing Performance Monitor, etc.)
- Configured via Business Context Layer
- Run on JBOT OS infrastructure
- Follow JBOT Protocol patterns

---

## The Flywheel

```
Deploy at Company A
    ↓
Operational Data + Results
    ↓
Case Study + Patterns Documented
    ↓
Protocol Methodology Improved
    ↓
Intake Questions Refined
    ↓
Deploy at Company B (FASTER & BETTER)
    ↓
More Data + Better Patterns
    ↓
More Deployments
    ↓
[repeat]
```

**Why competitors can't just copy the code:**

The code is open source. Anyone can fork it.

**But they can't copy the methodology.**

The methodology is earned through:
- 50 deployments → you know which systems work for which industries
- 500 incidents → you've built safeguards for failure modes competitors haven't seen yet
- 5,000 intake responses → you know which questions matter and which don't

**By the time a competitor does 50 deployments to catch up, you've done 500.**

That's the moat.

---

## Available Systems

### ✅ Ready to Deploy (Production-Tested)

#### 1. [Static Content Engine](systems/static-content-engine/)
**What it does:** Automates creative production from brief → approval → production → QA → delivery

**Real numbers:**
- 10-40x cost reduction per asset (vs agency)
- 4-10x speed increase (hours vs weeks)
- 80% reduction in creative director time
- Used by a NASDAQ company to produce 40-80 assets/month with <6 hrs/week review time

**Configure for:**
- Number of brands (1-20)
- Channels (Meta, Google, Email, Wholesale, Retail, etc.)
- Production resources (shoots, 3D, AI, templates)
- Approval governance
- QA thresholds

**Examples:** DTC eyewear (4 brands, 5 channels), Apparel (1 brand, AI-first), B2B SaaS (designer-driven)

---

#### 2. [Marketing Performance Monitor](systems/marketing-performance-monitor/)
**What it does:** Daily digest of ad performance, email campaigns, and automated insights

**Real numbers:**
- 15-30 minutes/day saved (no dashboard logins)
- Proactive alerts (pause $30+ spend with zero conversions)
- Used by a NASDAQ company to monitor $15-30K/month in ad spend

**Configure for:**
- Ad platforms (Meta, Google, TikTok, LinkedIn, etc.)
- Email platform (Klaviyo, Mailchimp, etc.)
- ROAS/CPA/CTR thresholds
- Winner/loser/alert categories

**Examples:** DTC e-commerce (Meta + Klaviyo, 3x ROAS target), B2B SaaS (LinkedIn + HubSpot), Agency (multi-platform)

---

### ⚠️ Foundation Exists (Components Available, Needs Extraction)

#### 3. Sales Pipeline Engine
**What it would do:** Automates sales pipeline management from lead → qualification → outreach → follow-up → close

**Available components:**
- HubSpot daily sync (21 crons running)
- Deal pipeline tracking
- Follow-up alerts
- Wholesale account monitoring

**Status:** salesbot has the patterns, needs abstraction for non-HubSpot CRMs

---

#### 4. Supply Chain Intelligence
**What it would do:** Monitors vendors, tracks shipments, flags delays, manages documentation

**Available components:**
- Freight email monitoring (KLN, Flexport, DB Schenker)
- PO tracking (partial)
- Shipment delay detection

**Status:** opsbot has email intelligence, needs structured data layer

---

#### 5. Inventory Watchdog
**What it would do:** Monitors stock levels, forecasts demand, triggers reorders, flags anomalies

**Available components:**
- NetSuite inventory monitoring (15 crons)
- Low stock alerts
- Order pacing forecast

**Status:** shipbot works, needs abstraction for Shopify/WooCommerce/BigCommerce

---

### 🔴 Identified But Not Built (Patterns Documented, No Implementation)

6. **Support Triage Engine** — Monitors support tickets, triages by urgency, drafts responses
7. **Customer Success Monitoring** — Tracks customer health, flags churn risk, triggers retention campaigns
8. **Partnership Pipeline** — Manages influencer/affiliate relationships from discovery → ROI tracking
9. **Product Roadmap Engine** — Aggregates feature requests, prioritizes by impact/effort
10. **Vendor Intelligence** — Monitors vendor performance, negotiates pricing, flags risks
11. **Budget Pacing Engine** — Tracks spend across categories, paces to targets, reallocates
12. **Security Monitoring** — Monitors access logs, flags anomalies, enforces policies
13. **Executive Dashboard** — Aggregates KPIs, generates weekly summaries, tracks OKRs
14. **Presentation Authoring** — Turns Discord/Slack discussions → strategic presentation pages

See [Systems Catalog](docs/SYSTEMS-CATALOG.md) for full list and use cases.

---

## How It Works

### 1. Review Available Systems

Browse `systems/` to see what's been built:
- Read the README (what it does, who it's for)
- Review example configurations (how it adapts)
- Check if your business matches a use case

### 2. Complete the Intake

Each system has an intake questionnaire:
- **Static Content Engine:** 26 questions (3 hours, 7 sections)
- **Marketing Performance Monitor:** 15 questions (20 minutes, 4 sections)

Your answers configure:
- Database schemas
- Workflow logic
- QA thresholds
- Approval routing
- Output formatting

### 3. Deploy with Your Config

**Option A: Use OpenClaw**
```bash
openclaw skills install systems/static-content-engine
openclaw skills config static-content-engine --config your-company.json
```

**Option B: Run standalone scripts**
```bash
python3 systems/static-content-engine/scripts/brief_generator.py \
  --config your-company.json
```

### 4. Pilot & Iterate

**Week 1:** Pilot with lowest-risk tier (e.g., organic content, not paid ads)  
**Week 2:** Refine thresholds based on real data  
**Week 3:** Expand to next tier  
**Week 4:** Full system live

---

## Who This Is For

### ✅ Good Fit

**DTC e-commerce brands:**
- Running paid ads (Meta, Google, TikTok)
- Need 20-40+ new creatives/month
- In-house designer or agency bottleneck
- $500-50K/month ad spend

**B2B SaaS companies:**
- Outbound sales motion
- Long sales cycles
- Content marketing at scale
- Product-led growth with support needs

**Agencies:**
- Managing creative for multiple clients
- High volume, tight turnaround
- Need to scale without hiring more designers

**E-commerce (physical products):**
- Managing inventory
- Complex supply chains
- Multi-channel distribution (DTC + wholesale + retail)

### ❌ Not a Fit (Yet)

**Very early stage (<$100K ARR):**
- Volume too low to justify automation
- Better to manual until you have patterns

**Enterprise with existing BI/DevOps teams:**
- Already have Looker/Tableau dashboards
- Data team builds custom reports
- This might be redundant

---

## What You Need

**Required:**
- OpenClaw runtime (or ability to run Python/Node scripts via cron)
- API access to your tools (ads platforms, CRM, email, etc.)
- Delivery channel (Telegram, Discord, Slack)

**Helpful but not required:**
- Supabase account (for shared data layer)
- Historical performance data (to set realistic thresholds)
- Team buy-in (someone needs to act on the insights)

---

## Roadmap

**Current state:** 2 systems ready to deploy (Static Content, Marketing Performance)

**Next:**
- Extract Sales Pipeline Engine (salesbot 21 crons)
- Extract Inventory Watchdog (shipbot)
- Extract Supply Chain Intelligence (opsbot)
- Build intake methodology training
- Create deployment playbooks per industry

**Future:**
- Self-serve config generator (web UI)
- Predictive intake (based on company profile, recommend systems)
- System marketplace (paid premium systems)

---

## Case Studies

### Company A: Eyewear E-Commerce (NASDAQ-listed)
**Systems deployed:** Static Content Engine, Marketing Performance Monitor, Inventory Watchdog, Supply Chain Intelligence, Fulfillment Engine

**Timeline:** 10 weeks (Jan-Mar 2026)

**Results:**
- 40-80 assets/month (was 10-15)
- $0.28/asset (AI tier, was $150-300 via agency)
- <6 hrs/week creative director time (was 20-25 hrs)
- $15-18/day operating cost for 10-bot fleet
- 15-20 hrs/week of work automated
- 10-15x ROI vs hiring equivalent roles

**Lessons learned:**
- Deployment pattern: discovery (weeks 1-2) → specialization (weeks 2-4) → coordination (weeks 4-6)
- Cost crisis (Feb 24): financebot crash-loop, $350/day → built cost watchdog
- Model policy: ban Opus in crons, Haiku for monitors, Sonnet for real work
- 2-tier access: read (anyone) vs write (exec only)

See [full case study](docs/CASE-STUDY-EYEWEAR-ECOMMERCE.md)

---

## Documentation

**Architecture:**
- [JBOT-ARCHITECTURE.md](docs/JBOT-ARCHITECTURE.md) — Four-layer system (OS/Protocol/Context/Systems)
- [THE-FLYWHEEL.md](docs/THE-FLYWHEEL.md) — Why each deployment makes the next one easier
- [LAYER-INTERFACES.md](docs/LAYER-INTERFACES.md) — How layers communicate

**Systems:**
- [SYSTEMS-CATALOG.md](docs/SYSTEMS-CATALOG.md) — All 19 systems (ready, partial, future)
- Each system has its own `/systems/[name]/` directory with README, config schema, examples

**Methodology:**
- [DEPLOYMENT-PATTERN.md](docs/DEPLOYMENT-PATTERN.md) — Discovery → Specialization → Coordination
- [INTAKE-METHODOLOGY.md](docs/INTAKE-METHODOLOGY.md) — How to gather business context
- [SAFETY-CONTROLS.md](docs/SAFETY-CONTROLS.md) — Kill switch, cost watchdog, approval gates

---

## Contributing

**Share your configuration:**
- Anonymize company-specific data
- Submit as example in `systems/[name]/examples/`
- Document what makes your use case unique

**Document edge cases:**
- Hit a configuration issue? Document it
- Found a better threshold? Share it
- Discovered a new pattern? Add it to docs

**Build integrations:**
- Add platform adapters (new ad platform, new CRM, etc.)
- Improve intake questionnaires based on your deployment
- Create industry-specific playbooks

**Open an issue or PR:** github.com/jabondano/jbot-protocol

---

## Support & Consulting

**Questions:** Open an issue in this repo

**Feature requests:** Open an issue with `[Feature]` tag

**Bug reports:** Open an issue with `[Bug]` tag

**Want help deploying?** joaquin@jabondano.co

**Consulting services:**
- Deployment assistance (we configure + deploy for you)
- Custom system development (build a new system specific to your needs)
- Training (teach your team the JBOT methodology)

---

## License

MIT — Use it, fork it, adapt it, sell services around it.

Attribution appreciated but not required.

---

## Credits

**Created by:** Joaquin Abondano

**Reference deployment:** Innovative Eyewear Inc (NASDAQ: LUCY)

**Timeline:** Jan-Mar 2026 (deployment), Apr 2026 (extraction + generalization)

**Built on:** OpenClaw runtime (openclaw.ai)

**Inspired by:** Real operational needs at a public company

---

*Not a chatbot. Not an assistant. An operator.*

**Wispr · jabondano · 🦞**
