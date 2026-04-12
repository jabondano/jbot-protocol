# Configuration Comparison
*How three different businesses configure the same Sales Pipeline Engine*

---

## Overview

The Sales Pipeline Engine is **universal in structure** but **business-specific in configuration**. This document compares three real/hypothetical configurations to illustrate what's constant vs what adapts to your business.

**Compared:**
1. **Acme Wholesale Co** — B2B wholesale, HubSpot CRM, 2-rep team
2. **CloudMetrics SaaS** — B2B SaaS, Pipedrive CRM, 5-rep team
3. **Bootstrapped Consulting** — Solo founder, Google Sheets, manual process

---

## Company Context

| Dimension | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|-----------|----------------|-------------------|-------------------------|
| **Industry** | B2B Wholesale (Eyewear/PPE) | B2B SaaS | Professional Services |
| **Sales Motion** | Hybrid (inbound + outbound) | Inbound (product-led) | Outbound (manual prospecting) |
| **Avg Deal Size** | $15K | $28K | $8.5K |
| **Sales Cycle** | 45 days | 60 days | 30 days |
| **Annual Target** | $722K | $2.4M | $350K |
| **Team Size** | 3 (2 AEs + 1 support) | 5 (1 VP + 3 AEs + 1 SDR) | 1 (founder) |

**What's universal:** All three have a sales cycle, deal stages, and need pipeline visibility.

**What's different:** Volume, complexity, team structure, and urgency.

---

## CRM Integration

| Dimension | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|-----------|----------------|-------------------|-------------------------|
| **CRM Type** | HubSpot | Pipedrive | Google Sheets |
| **Access Level** | Admin (full API) | Admin (full API) | Admin (OAuth) |
| **Sync Frequency** | Hourly | Real-time webhook | Daily batch |
| **Custom Fields** | 3 (product_line, lead_source, icp_score) | 4 (company_size, use_case, seats, plan_tier) | 3 (industry, contact_method, proposal_link) |

**Why this matters:**
- **Acme:** Hourly sync is sufficient (not high-velocity)
- **CloudMetrics:** Real-time webhook needed (fast-moving inbound leads)
- **Bootstrapped:** Daily batch acceptable (solo founder, low volume)

**Configuration decision:** Sync frequency depends on deal velocity, not CRM choice.

---

## Pipeline Stages

| Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|----------------|-------------------|-------------------------|
| 1. New Lead (10%) | 1. Lead (5%) | 1. Prospect (10%) |
| 2. Contacted (15%) | 2. Qualified (15%) | 2. Contacted (20%) |
| 3. Qualification (25%) | 3. Demo Scheduled (30%) | 3. Meeting Scheduled (40%) |
| 4. Discovery Complete (40%) | 4. Demo Completed (50%) | 4. Proposal Sent (60%) |
| 5. Proposal Sent (60%) | 5. Proposal Sent (70%) | 5. Negotiation (80%) |
| 6. Negotiation (75%) | 6. Negotiation (85%) | 6. Won (100%) / Lost (0%) |
| 7. Contract Review (90%) | 7. Won (100%) / Lost (0%) | — |
| 8. Won (100%) / Lost (0%) | — | — |

**Observations:**
- **Acme:** 9 stages (most granular) — wholesale requires contract review, compliance
- **CloudMetrics:** 8 stages (standard B2B SaaS) — demo-centric process
- **Bootstrapped:** 7 stages (minimal) — solo founder, simpler process

**Why this matters:** More stages = better visibility but more admin overhead. Choose based on team size and deal complexity.

**Configuration decision:** Stage count and probability weights depend on your actual conversion funnel, not industry.

---

## Follow-Up Cadences

| Dimension | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|-----------|----------------|-------------------|-------------------------|
| **First Touch** | 3 days | 1 day | 2 days |
| **Second Touch** | 7 days | 3 days | 5 days |
| **Third Touch** | 14 days | 7 days | 10 days |
| **Cold Threshold** | 30 days, 3 attempts | 21 days, 4 attempts | 21 days, 3 attempts |
| **Automation** | Enabled (templates) | Enabled (templates) | Disabled (manual) |

**Why this matters:**
- **Acme:** Longer cadence (B2B buyers slower to respond, institutional)
- **CloudMetrics:** Faster cadence (SaaS buyers expect fast response, competitive market)
- **Bootstrapped:** Mid-range (solo founder can't handle aggressive cadence)

**Configuration decision:** Cadence speed depends on buyer urgency and your capacity, not CRM.

---

## Deal Scoring Weights

| Dimension | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|-----------|----------------|-------------------|-------------------------|
| **Deal Size** | 30% | 25% | 20% |
| **Engagement** | 25% | 35% | 40% |
| **Stage** | 25% | 20% | 30% |
| **Timeline** | 20% | 20% | 10% |

**Why this matters:**
- **Acme:** Deal size prioritized (wholesale = big orders, high variance)
- **CloudMetrics:** Engagement prioritized (SaaS = responsive buyers close faster)
- **Bootstrapped:** Engagement prioritized (solo founder needs warm leads, can't chase cold)

**Configuration decision:** Scoring weights reflect what predicts wins in YOUR sales process, not a generic formula.

---

## Stale Deal Thresholds

| Dimension | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|-----------|----------------|-------------------|-------------------------|
| **Warning** | 7 days | 5 days | 7 days |
| **Urgent** | 14 days | 10 days | 14 days |
| **Critical** | 30 days | 21 days | 21 days |

**Why this matters:**
- **Acme:** Wholesale buyers slower (14-day urgent threshold acceptable)
- **CloudMetrics:** SaaS buyers fast (10-day urgent threshold needed)
- **Bootstrapped:** Solo founder tolerates longer gaps (14 days acceptable)

**Configuration decision:** Stale thresholds depend on sales velocity and buyer behavior, not team size.

---

## Reporting Structure

| Dimension | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|-----------|----------------|-------------------|-------------------------|
| **Frequency** | Daily (Mon-Sat) | Daily (Mon-Fri) | Daily (Mon-Fri) |
| **Time** | 9:00 AM EST | 8:00 AM PST | 8:30 AM CST |
| **Channel** | Telegram | Slack | Telegram |
| **Detail Level** | Detailed | Summary | Summary |
| **Sections** | All 8 sections | 7 sections (no data quality) | 5 sections (action items, closing, top deals, stale, snapshot) |

**Why this matters:**
- **Acme:** Detailed report needed (COO wants full pipeline view)
- **CloudMetrics:** Summary preferred (team of 5, don't need every detail daily)
- **Bootstrapped:** Summary only (solo founder, just needs action items)

**Configuration decision:** Report detail depends on recipient role and time budget, not company size.

---

## Alert Configuration

| Alert Type | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|------------|----------------|-------------------|-------------------------|
| **Stale Deals** | 14 days | 10 days | 14 days |
| **High-Priority** | $25K+ | $50K+ | $10K+ |
| **Overdue Close** | 3 days before | 1 day before | 3 days before |
| **Missing Data** | amount, owner, close_date | amount, owner, close_date, stage | amount, close_date |
| **Conversion Drop** | Disabled | Enabled (15% drop) | Disabled |

**Why this matters:**
- **Acme:** High-priority at $25K (medium deal size)
- **CloudMetrics:** High-priority at $50K (larger average deal)
- **Bootstrapped:** High-priority at $10K (smaller deals, every one matters)

**Configuration decision:** Thresholds are relative to YOUR average deal size, not absolute numbers.

---

## Team Structure

| Dimension | Acme Wholesale | CloudMetrics SaaS | Bootstrapped Consulting |
|-----------|----------------|-------------------|-------------------------|
| **Assignment Logic** | Product-based | Territory-based | Manual |
| **Team Reports** | 2 (Optical, PPE) | 3 (Enterprise, Mid-Market, SMB) | None |
| **Notification Routing** | Telegram (all 3 members) | Slack (#sales-pipeline) | Telegram (solo) |

**Why this matters:**
- **Acme:** Product-based split (Optical vs PPE require different expertise)
- **CloudMetrics:** Territory-based split (deal size determines owner)
- **Bootstrapped:** No split (solo founder handles all)

**Configuration decision:** Team structure drives assignment logic and reporting breakdown.

---

## What's Universal vs Business-Specific

### ✅ Universal (Same Across All Three)

**Core Workflow:**
- Lead → Qualified → Contact → Demo/Discovery → Proposal → Negotiation → Closed
- Stale deal detection (days inactive)
- Follow-up cadences (first, second, third touch)
- Deal scoring (size + engagement + stage + timeline)
- Daily pipeline reports

**System Components:**
- CRM integration (API or manual sync)
- Daily monitoring cron job
- Alert routing (Telegram/Slack/Email)
- Stage-based automation triggers

### ❌ Business-Specific (Different For Each)

**Configuration Values:**
- Stage names and count (7-9 stages)
- Stage probability weights (10%-90%)
- Follow-up cadence timing (1-14 days)
- Stale deal thresholds (7-30 days)
- High-priority deal size ($10K-$50K)
- Scoring weights (20%-40% per dimension)
- Team assignment logic (product/territory/manual)
- Report detail level (summary vs detailed)

**Process Differences:**
- Demo-first (SaaS) vs proposal-first (wholesale) vs meeting-first (consulting)
- Fast cadence (1-3 days) vs slow cadence (7-14 days)
- Automated follow-up vs manual touch
- Team-based vs solo operation

---

## Key Takeaways

### 1. **Start with fewer stages, add later**
Bootstrapped (7 stages) is easier to pilot than Acme (9 stages). You can always split stages later when you need more granularity.

### 2. **Sync frequency follows deal velocity, not CRM choice**
Real-time webhook (CloudMetrics) is overkill if you close 5 deals/month. Daily batch (Bootstrapped) is fine until you scale.

### 3. **Scoring weights should reflect YOUR win patterns**
If engagement predicts wins better than deal size in your process, weight it higher. Run historical analysis to calibrate.

### 4. **Stale thresholds depend on buyer urgency, not your capacity**
Even solo founders need aggressive stale detection if buyers expect fast responses. Threshold is about buyer behavior, not team size.

### 5. **Report detail should match recipient role**
VP Sales needs detailed (pipeline health, team breakdown, stage distribution). Founder needs summary (action items, top deals, stale).

### 6. **Team structure determines automation complexity**
Solo (manual assignment) < Product-based (2 segments) < Territory-based (3+ segments). Start simple, add segmentation when needed.

---

## Migration Paths

**From Bootstrapped → CloudMetrics (scaling up):**
1. Move from Google Sheets → Pipedrive/HubSpot (CRM adoption)
2. Enable real-time sync (replace daily batch)
3. Add team assignment logic (territory-based)
4. Enable conversion rate drop alerts (team accountability)
5. Split into team-specific reports

**From CloudMetrics → Acme (adding complexity):**
1. Add more stages (split Discovery into Discovery + Proposal Prep)
2. Add contract review stage (enterprise/compliance requirements)
3. Enable legal/compliance review gates
4. Extend sales cycle assumptions (60d → 90d)
5. Add custom scoring criteria (referral bonus, competitor mentions)

**From Acme → CloudMetrics (simplifying):**
1. Collapse stages (Contract Review → Negotiation)
2. Remove compliance gates (B2B SaaS is lighter than wholesale)
3. Shorten cadence (7d → 3d)
4. Increase automation (more templates, fewer manual touches)

---

## Choosing Your Configuration

**Ask yourself:**

1. **How many stages do buyers actually experience?**
   - Don't invent stages. Map real touchpoints: first call, demo, proposal, negotiation, contract.

2. **How fast do buyers respond?**
   - SaaS buyers expect <24h response. Wholesale buyers expect 3-5 days. Set cadence accordingly.

3. **What predicts wins in your process?**
   - Run historical analysis: do big deals close more often? Or engaged deals? Weight scoring accordingly.

4. **How much admin time can you afford?**
   - Solo founder: summary reports, minimal data quality checks
   - Team of 5: detailed reports, strict data quality, team accountability

5. **What breaks when you scale?**
   - Manual entry breaks first (move to CRM)
   - Manual follow-up breaks next (enable automation)
   - Founder-as-only-reviewer breaks last (delegate approval gates)

---

**Next:** Read `CASE-STUDY-LUCYD.md` for real implementation metrics and ROI data.
