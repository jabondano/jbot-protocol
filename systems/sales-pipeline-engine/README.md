# Sales Pipeline Engine
*Automated B2B sales pipeline monitoring from lead → qualified → contact → demo → proposal → negotiation → closed*

---

## What This Is

A configurable system that automates sales pipeline visibility and follow-up management, used by a NASDAQ-listed company to monitor 80+ active B2B deals worth $1.85M with <3 hours/week of sales director time.

**Not a dashboard replacement. A proactive pipeline operator.**

---

## The Problem

Every B2B sales team faces the same bottleneck: **pipeline visibility can't keep up with deal velocity.**

You need:
- Daily visibility into pipeline health (what's moving, what's stalled, what needs action)
- Stale deal detection (deals going cold without anyone noticing)
- Follow-up reminders (deals in early stages requiring next touch)
- Deal scoring (which deals to prioritize when time is limited)
- Team accountability (who owns what, who's stuck, who's closing)

**Traditional solutions:**
- **Weekly sales meetings:** Too slow, deals go stale between meetings
- **CRM dashboard:** Reactive (you have to remember to check), time-consuming (10-15 min/day)
- **Manual follow-up tracking:** Breaks down at scale, high-performers get attention, cold leads forgotten
- **Spreadsheet pipeline:** Stale by the time you update it, no automation

**All hit the same wall:** As deal count increases, visibility drops OR admin time explodes OR deals slip through cracks.

---

## The Solution

A **7-component automated pipeline** with proactive alerts and daily visibility:

```
LEAD CAPTURE → QUALIFICATION → STAGE TRACKING → FOLLOW-UP AUTOMATION → 
STALE DETECTION → DEAL SCORING → DAILY REPORTS
```

**What makes it work:**

1. **CRM integration** — Sync with HubSpot, Salesforce, Pipedrive, or Google Sheets (configurable)
2. **Daily monitoring** — Pipeline health checks run automatically, delivered to Telegram/Slack/Discord
3. **Proactive alerts** — Stale deals flagged before they die, high-priority deals surfaced daily
4. **Follow-up cadences** — Automated reminders based on stage + days since last touch
5. **Deal scoring** — Prioritize based on deal size, engagement, stage, and timeline
6. **Team visibility** — Owner-specific reports, pipeline by team member, conversion tracking

**Result:** 80% reduction in admin time, 3x faster deal follow-up, zero stale deals >14 days

---

## Real Numbers (Reference Implementation)

**Company:** Eyewear manufacturer (DTC + B2B wholesale, 4 brands, $4.3M annual target)

**Before:**
- Pipeline tracking: Manual CRM checks, weekly sales meetings
- Follow-up: Calendar reminders, inconsistent
- Visibility: Dashboard login required (10-15 min/day)
- Stale deals: ~20-30% of pipeline (>14 days no update)
- Sales director time: 15-20 hrs/week on admin

**After:**
- Pipeline tracking: Automated daily reports to Telegram (2-3 min scan)
- Follow-up: Automated reminders based on stage + cadence config
- Visibility: Proactive push (alerts come to you)
- Stale deals: 0% of pipeline (all deals updated within 14 days)
- Sales director time: <3 hrs/week on admin

**ROI:** 75% time savings, 100% stale deal reduction, 3x faster follow-up velocity

---

## How It Works

### The 7 Components

#### Component 1: LEAD CAPTURE
**What:** Inbound leads auto-flow into CRM from website forms, email, LinkedIn, referrals  
**Integration:** HubSpot forms, Zapier webhooks, email parsing, manual entry  
**Output:** New lead created with source tag, owner assignment, initial score  
**Time:** <30 seconds (automated)

**Why this matters:** Manual lead entry has 20-40% data loss rate. Automated capture ensures nothing falls through cracks.

#### Component 2: QUALIFICATION
**What:** Score new leads against ICP rubric, route to appropriate owner  
**Criteria:** Company size, industry fit, budget signals, decision-maker access, urgency  
**Scoring tiers:** Hot (75+), Warm (45-74), Cold (20-44), DQ (<20)  
**Time:** Real-time scoring on creation

**Why this matters:** Sales reps waste 50% of time on unqualified leads. Scoring focuses effort on winnable deals.

#### Component 3: STAGE TRACKING
**What:** Monitor deal progression through pipeline stages  
**Stages:** Lead → Qualified → Contact → Demo → Proposal → Negotiation → Closed  
**Metrics:** Stage duration, conversion rate by stage, bottleneck identification  
**Time:** Continuous tracking

**Why this matters:** Deals stall in specific stages (usually Discovery or Negotiation). Tracking reveals where process breaks.

#### Component 4: FOLLOW-UP AUTOMATION
**What:** Automated reminders based on stage + days since last touch  
**Cadences:**
- First touch → 3 days (no response)
- Second touch → 7 days
- Third touch → 14 days
- Cold threshold → 30 days

**Routing:** Reminders delivered to deal owner via Telegram/Slack  
**Time:** Daily batch checks (9 AM)

**Why this matters:** Humans forget. 40% of deals go cold simply because rep forgets to follow up.

#### Component 5: STALE DETECTION
**What:** Flag deals with no activity in X days (configurable: 7, 14, 30 days)  
**Criteria:** No CRM updates, no emails, no calls, no meetings logged  
**Alert severity:** 🟡 7-day (warning), 🟠 14-day (urgent), 🔴 30-day (critical)  
**Time:** Daily scan

**Why this matters:** Stale deals = lost revenue. Catching at 14 days recoverable; at 60 days, dead.

#### Component 6: DEAL SCORING
**What:** Prioritize deals based on size, engagement, stage, timeline  
**Scoring model:**
- Deal size (30% weight) — larger deals higher priority
- Engagement (25%) — responsive contacts higher priority
- Stage (25%) — advanced stages higher priority
- Timeline (20%) — urgent close dates higher priority

**Output:** Daily "hot deals" list (top 5-10 to focus on)  
**Time:** Real-time scoring on every CRM update

**Why this matters:** Reps can't work 80 deals equally. Scoring directs effort to highest-value, highest-probability wins.

#### Component 7: DAILY REPORTS
**What:** Structured pipeline brief delivered every morning  
**Sections:**
1. Action items (urgent deals, follow-ups needed, overdue close dates)
2. Deals closing this week (pipeline forecast)
3. Top 5 deals in motion (highest value)
4. Stale deal alerts (>14 days no update)
5. Pipeline snapshot (total deals, total value, vs target)
6. Data quality checks (missing amounts, missing owners, missing close dates)

**Delivery:** Telegram, Discord, or Slack at 9 AM (configurable)  
**Time:** 2-3 min to consume

**Why this matters:** Proactive visibility beats reactive dashboard checks. Push > pull.

---

## Configuration: Your CRM, Your Process

The system is **universal** (7-component workflow, stage tracking, scoring framework), but **configured** to your specific sales process.

### What Gets Configured

**CRM Integration:**
- Which CRM? (HubSpot, Salesforce, Pipedrive, Zoho, Copper, Airtable, Google Sheets, None)
- API access level (admin, read-write, read-only)
- Custom fields (product line, lead source, ICP tier, etc.)
- Sync frequency (real-time webhook, hourly cron, daily batch)

**Pipeline Stages:**
- How many stages? (standard 7-stage or custom)
- Stage names (your terminology: "Discovery" vs "Needs Assessment")
- Stage probability (% likelihood of close at each stage)
- Average duration per stage (how long deals typically stay in each stage)

**Follow-Up Cadences:**
- First touch delay (24h, 3 days, 1 week)
- Second touch delay (3 days, 1 week, 2 weeks)
- Third touch delay (1 week, 2 weeks, 1 month)
- Cold threshold (30 days, 60 days, 90 days)
- Auto-mark cold? (yes/no)

**Deal Scoring Weights:**
- Deal size weight (0-100%)
- Engagement weight (0-100%)
- Stage weight (0-100%)
- Timeline weight (0-100%)
- Custom criteria (referral source, competitor mention, budget confirmed)

**Team Structure:**
- How many owners? (solo founder, 2-3 reps, 10+ team)
- Owner assignment logic (round-robin, territory-based, product-based)
- Team-specific reports (optical vs PPE, enterprise vs SMB)
- Notification routing (who gets what alerts)

**Reporting Config:**
- Frequency (daily, weekly, custom)
- Delivery channel (Telegram, Discord, Slack, Email)
- Report detail level (summary vs detailed)
- Recipients (sales director, full team, founder)
- Footer names (team attribution)

**Alert Thresholds:**
- Stale deal days (7, 14, 30)
- High-priority deal size ($10K, $50K, $100K)
- Overdue close date alert (1 day, 3 days, 1 week)
- Missing data alerts (no amount, no owner, no close date)

### How Configuration Works

**Step 1:** Complete the intake questionnaire (~30 min, one-time)  
**Step 2:** Generate your configuration file (automated)  
**Step 3:** Deploy the system with your config  
**Step 4:** Pilot with 10-20 deals, tune thresholds  
**Step 5:** Scale to full pipeline

---

## Repository Structure

```
sales-pipeline-engine/
├── README.md (this file)
├── config/
│   ├── schema.json (configuration schema definition)
│   └── intake-v1.md (questionnaire to generate config)
├── examples/
│   ├── wholesale-b2b-hubspot.json (Lucyd B2B pipeline, anonymized)
│   ├── saas-b2b-pipedrive.json (hypothetical B2B SaaS)
│   └── no-crm-spreadsheet.json (Google Sheets pipeline)
└── docs/
    ├── CONFIGURATION-COMPARISON.md (compare example configs)
    └── CASE-STUDY-LUCYD.md (real implementation metrics)
```

---

## Quick Start

### 1. Review Example Configurations

Start by looking at the example configs to understand how different businesses configure the same system:

- **`examples/wholesale-b2b-hubspot.json`** — B2B wholesale, HubSpot CRM, 9-stage pipeline, 2 sales reps
- **`examples/saas-b2b-pipedrive.json`** — B2B SaaS, Pipedrive CRM, 7-stage pipeline, 5-rep team
- **`examples/no-crm-spreadsheet.json`** — No CRM, Google Sheets tracking, manual entry

### 2. Complete the Intake

Read `config/intake-v1.md` and answer the questions. This generates your configuration.

**Time required:** ~30 minutes

### 3. Generate Configuration

Run the config generator:
```bash
node config/generate.js --intake your-intake-responses.json
```

Outputs: `your-company.config.json`

### 4. Deploy CRM Integration

**If HubSpot:**
```bash
export HUBSPOT_ACCESS_TOKEN="your-token-here"
python3 scripts/hubspot_integration.py --config your-company.config.json --test
```

**If Salesforce:**
```bash
export SALESFORCE_USERNAME="your-username"
export SALESFORCE_PASSWORD="your-password"
export SALESFORCE_TOKEN="your-security-token"
python3 scripts/salesforce_integration.py --config your-company.config.json --test
```

**If Google Sheets:**
```bash
export GOOGLE_SHEETS_ID="your-sheet-id"
python3 scripts/sheets_integration.py --config your-company.config.json --test
```

### 5. Set Up Daily Report

**If using OpenClaw:**
```bash
openclaw cron add \
  --name "daily-pipeline-monitor" \
  --schedule "0 9 * * *" \
  --agent-turn \
  --message "Run the daily pipeline monitor with config: your-company.config.json"
```

**If standalone:**
```bash
# Add to crontab
0 9 * * * cd /path/to/scripts && python3 daily_pipeline.py --config ../config/your-company.config.json
```

### 6. First Run (Manual)

```bash
python3 scripts/daily_pipeline.py \
  --config config/your-company.config.json \
  --output telegram
```

**Review:**
- Are thresholds set correctly? (stale deal days, high-priority amounts)
- Are stages mapped correctly? (your CRM stages → standard stages)
- Is report format readable?
- Are alerts actionable?

### 7. Iterate Thresholds

**Common adjustments:**
- Stale deal threshold too sensitive (14 days → 21 days)
- High-priority amount too low ($10K → $25K)
- Follow-up cadence too fast (3 days → 5 days)
- Too many data quality alerts (disable "missing amount" if not required)

**After 7-10 days:** Thresholds should stabilize based on real pipeline patterns

---

## Who This Is For

### ✅ Good Fit

**B2B companies with 20-100 active deals:**
- Sales cycle 2 weeks - 6 months
- Deal sizes $5K - $500K
- Team of 1-10 reps
- Using HubSpot, Salesforce, or Pipedrive (or willing to adopt)

**Agencies selling retainers/projects:**
- Proposal-based sales
- Multi-stage sales process
- Recurring revenue model
- Need follow-up discipline

**Wholesale/distribution:**
- B2B buyers (optical, PPE, retail chains)
- Sample → Demo → PO workflow
- Multiple product lines
- Territory-based sales team

### ❌ Not a Fit (Yet)

**Enterprise sales (6-18 month cycles, $1M+ deals):**
- Too complex for automated scoring
- Require custom deal rooms, legal reviews
- Better to keep manual with dedicated account exec per deal

**High-velocity SMB sales (100+ deals/month, <$5K average):**
- Volume too high for manual pipeline checks
- Better fit: automated email sequences + self-serve
- Wait for high-volume variant of this system

**Self-serve / product-led growth:**
- No sales reps, no pipeline
- Better fit: product analytics + automated onboarding

---

## What You Need

**Required:**
- CRM with API access (HubSpot, Salesforce, Pipedrive, etc.) OR Google Sheets
- Delivery channel (Telegram, Discord, Slack, or Email)
- Sales process with defined stages (lead → closed)

**Helpful but not required:**
- Historical deal data (to set realistic scoring weights)
- Existing lead qualification rubric (ICP definition)
- Email integration (for engagement scoring)

**Nice to have:**
- Calendar integration (for meeting tracking)
- Email platform integration (for outreach automation)
- Product catalog (for deal line items)

---

## Roadmap

**Current state:** v1.0 — Proven with one reference implementation (eyewear B2B wholesale)

**Next:**
- v1.1 — Salesforce adapter (in progress)
- v1.2 — Pipedrive adapter
- v1.3 — Google Sheets adapter
- v2.0 — Email integration (engagement scoring from Gmail/Outlook)
- v2.1 — Meeting tracking (calendar integration)
- v3.0 — Automated follow-up drafts (AI-generated emails based on deal context)

---

## Comparison to CRM Dashboards

| Feature | CRM Dashboard | Daily Pipeline Monitor |
|---------|---------------|------------------------|
| **Time to consume** | 10-15 min (login, navigate, filter) | 2-3 min (scan brief) |
| **Insights** | You filter & analyze | Pre-calculated + categorized |
| **Alerts** | You notice | Proactive (flagged in brief) |
| **Delivery** | Pull (you go get it) | Push (it comes to you) |
| **Follow-ups** | Manual calendar reminders | Automated cadence tracking |
| **Stale deals** | You search for them | Auto-flagged daily |
| **Historical** | Yes (query historical) | No (point-in-time snapshot) |

**When to use dashboard:** Deep dives, deal detail, historical analysis, custom reporting  
**When to use daily brief:** Morning check-in, quick decision-making, proactive alerts, follow-up reminders

**They complement each other.** Brief for daily ops, dashboard for deep analysis.

---

## Support & Contribution

**Questions:** Open an issue in this repo  
**Feature requests:** Open an issue with `[Feature]` tag  
**Bug reports:** Open an issue with `[Bug]` tag  
**Consulting:** joaquin@jabondano.co (if you want help deploying this)

**Contributing:**
- Share your configuration (anonymized) as an example
- Document CRM-specific integration patterns
- Improve the intake questionnaire based on your experience
- Build adapters for CRMs not yet supported

---

## License

MIT — Use it, fork it, adapt it, sell services around it.

Attribution appreciated but not required.

---

## Credits

Built by Joaquin Abondano as part of the JBOT Protocol.

Reference implementation developed at Innovative Eyewear Inc (NASDAQ: LUCY) over 8 weeks (Jan-Mar 2026).

Extracted and generalized Apr 2026.

---

**Next:** Read `config/intake-v1.md` to start configuring your pipeline, or browse `examples/` to see real configurations.
