# Deployment Example: B2B Wholesale Pipeline at a Consumer Hardware Company

> This example is drawn from a real deployment at a multi-channel consumer products company, anonymized. Figures are illustrative of a representative deployment of this system — they are not audited results and should not be attributed to any specific company.

---

## Deployment Context

**Company profile:** Consumer hardware manufacturer selling DTC and B2B wholesale  
**Product Lines:** Optical (eyewear), Safety/PPE (industrial eyewear)  
**Team Size:** 3 (2 sales directors + 1 wholesale associate)  
**Channel role:** B2B wholesale was the smaller but strategically important channel

**Business context:**
- Small team managing 80+ active deals across a seven-figure pipeline
- Sales directors also handle product development, supply chain, customer support
- The COO oversees sales but isn't the day-to-day closer

---

## The Problem (Before)

### Manual Pipeline Tracking

**Daily routine:**
1. Login to CRM, filter deals by stage (3-5 min)
2. Check for stale deals manually (5-7 min)
3. Identify deals closing this week, review top deals by value (5-8 min)
4. Check data quality (missing amounts, owners, close dates) (3-5 min)
5. Copy-paste into Slack/email for team (2-3 min)

**Total time:** 15-20 minutes/day × 6 days/week = **90-120 min/week**

**Pain points:**
- Reactive (someone has to remember to check the CRM daily)
- Falls on the COO, not the sales reps
- Inconsistent (weekends/travel = no check-ins)
- Error-prone (manual filters miss edge cases)
- No stale deal tracking (deals went 30-60 days without updates)

### Follow-Up Gaps

**Before automation:**
- **20-30%** of deals went >14 days without CRM update
- No systematic follow-up cadence (reps relied on memory/calendar)
- High-value deals ($25K+) sometimes forgotten for weeks
- The COO discovered stale deals during weekly meetings (too late to recover)

**Impact:**
- A handful of deals lost per quarter to follow-up failures — a five-figure quarterly revenue risk
- Sales directors spent 10-15 hrs/week on admin vs selling

### Data Quality Issues

15-20% of deals were missing a close date, 10-15% missing an amount, and 5-10% unassigned (no owner). Consequence: inaccurate pipeline forecasts and unclear priorities.

---

## The Solution (After)

### Automated Daily Pipeline Monitor

**What runs:** Python script (`daily_pipeline.py`) pulling the CRM via API  
**When:** 9 AM local, Mon-Sat (excludes Sunday)  
**Delivery:** Telegram direct message to the COO  
**Time to consume:** 2-3 minutes (vs 15-20 min manual check)

**Report sections:**
1. **Action Items** — Urgent deals, overdue close dates, follow-ups needed
2. **Closing This Week** — Deals with close date within 7 days, sorted by date
3. **Top 5 Deals** — Highest value deals in motion, current stage, owner
4. **Stale Deal Alerts** — Deals >14 days no update (flagged with days count)
5. **Pipeline Snapshot** — Total active deals, total value, vs annual channel target
6. **Data Quality Checks** — Missing amounts, owners, close dates

**Example output (anonymized, figures illustrative):**
```
📊 DAILY B2B PIPELINE MONITOR

🔔 ACTION ITEMS (3)
  • [Company A] - Overdue close date (3 days past)
  • [Company B] - Stale 18 days (no updates since Feb 24)
  • [Company C] - High-priority ($50K) in Proposal Sent for 12 days

📅 CLOSING THIS WEEK (5 deals, $215K)
  1. 04/14 | [Company D] | $75K | Negotiation | Rep A
  2. 04/15 | [Company E] | $50K | Contract Review | Rep B
  ...

🏆 TOP 5 DEALS IN MOTION ($485K)
  1. $100K | [Company F] | Discovery Complete | Rep A
  2. $85K | [Company G] | Proposal Sent | Rep B
  ...

⚠️ STALE DEALS (0)
  • All deals updated within 14 days ✅

📊 PIPELINE SNAPSHOT
  • Active Deals: 80+
  • Total Value: seven figures
  • vs Target: pipeline coverage >2x annual B2B target

🔍 DATA QUALITY (2 issues)
  • 2 deals missing close date (Contacted stage)
  • 0 deals missing amount ✅
```

### Weekly B2B Pipeline Report

**What runs:** Comprehensive pipeline analysis (`generate_pipeline_report.py`)  
**When:** Monday 9 AM local  
**Delivery:** Telegram  
**Time to consume:** 5-7 minutes

**Additional insights:**
- Team breakdown (Rep A vs Rep B pipeline)
- Stage distribution analysis (where deals are bottlenecking)
- Conversion rate by stage (Contacted → Qualification → etc.)
- Weighted forecast (probability × deal size)
- Win/loss trends

---

## Representative Results (Illustrative Model)

The figures below model what the first 8 weeks of a deployment like this looks like, based on the operator's observed time allocation before and after. They are estimates, not audited measurements.

### Time Savings

| Task | Before (hrs/week) | After (hrs/week) | Savings |
|------|-------------------|------------------|---------|
| Pipeline monitoring | 1.5 | 0.2 | 1.3 hrs |
| Stale deal detection | 2.0 | 0 | 2.0 hrs |
| Follow-up tracking | 3.5 | 0.5 | 3.0 hrs |
| Data quality checks | 1.5 | 0.2 | 1.3 hrs |
| Team coordination | 2.5 | 1.0 | 1.5 hrs |
| **Total** | **11.0** | **1.9** | **9.1 hrs** |

**Per-person impact:**
- COO: ~6 hrs/week saved (was doing most pipeline admin)
- Sales directors: ~3 hrs/week saved (less manual reporting)

**Illustrative annual value:** 9.1 hrs/week × 50 weeks × $150/hr (blended rate) = **~$68K/year**

### Pipeline Health

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Stale deals (>14 days) | 20-30% | <5% | ✅ 80-90% reduction |
| Deals >30 days no update | 10-15% | 0% | ✅ Eliminated |
| Missing close dates | 15-20% | <5% | ✅ ~75% improvement |
| Missing amounts | 10-15% | <3% | ✅ ~80% improvement |
| Follow-up consistency | Inconsistent | Daily alerts | ✅ Full coverage |

**Revenue effect (illustrative):** stale-deal alerts plausibly rescue a five-figure amount of at-risk revenue per quarter, and faster follow-up supports a modest win-rate lift. Treat these as directionally representative, not measured.

### Pipeline Visibility

**Before:** COO checked the pipeline 3-4 times/week; sales directors checked when they remembered; no weekend coverage.

**After:** Daily brief 6 days/week to the COO, owner-specific alerts pushed to sales directors, weekend activity flagged Monday morning, and shared pipeline context for the whole team.

---

## Illustrative ROI Model

### Implementation Cost

**Initial setup (Week 1):**
- CRM API integration: 2 hours
- Daily report script development: 4 hours
- Weekly report script development: 3 hours
- Config tuning (thresholds, sections): 2 hours
- Cron job setup: 1 hour
- **Total:** 12 hours × $150/hr = **$1,800**

**Ongoing cost:**
- Server/hosting: $5/month ($60/year)
- CRM API (included in existing plan): $0
- Maintenance/updates: 2 hours/quarter × $150 = $300/year
- **Total annual:** **$360**

### Financial Return (Illustrative)

- Time savings value: ~$68K/year (9.1 hrs/week at a $150/hr blended rate)
- Revenue protection: five-figure annual upside from rescued stale deals (estimate)

Even counting time savings alone, the model returns the $1,800 setup cost in well under a month. The point isn't the precise multiple — it's that a 12-hour build pays for itself almost immediately at any reasonable blended rate.

---

## What Worked

1. **Proactive push vs reactive pull.** Daily Telegram brief (pushed) beats CRM dashboard (pulled). The COO reads the report over coffee, no login required.
2. **Action items first.** Report starts with "what needs attention today" (not vanity metrics). Drives immediate action.
3. **Stale deal detection.** 14-day threshold was the sweet spot (7 days too aggressive, 30 days too late). Caught deals before they went cold.
4. **Team visibility.** Footer with all team names reinforced shared accountability ("everyone sees this report, not just me").
5. **Data quality automation.** Flagging missing fields daily drove compliance. Within 3 weeks, data quality improved sharply.
6. **Consistent delivery.** 6 days/week (Mon-Sat) meant no deal went >24 hours unmonitored. Weekends excluded (no action taken Sunday).

---

## What Didn't Work (Lessons Learned)

1. **Too many sections initially.** First version had 10 sections (overwhelming). Trimmed to 6 core sections. Less is more.
2. **Wrong stale threshold.** Started with 7-day threshold (too many false positives). Adjusted to 14 days.
3. **Missing owner filter.** Early reports showed all 80+ deals (information overload). Added owner-specific filtering for sales directors.
4. **No weekend exclusion.** Tried 7-day delivery initially. Sunday reports went unread. Cut to 6 days.
5. **Overly detailed weekly report.** First weekly report was 50+ lines (nobody read it). Trimmed to top-line metrics + team breakdown.

---

## Configuration Details

**CRM:** HubSpot (B2B sales pipeline)  
**Pipeline Stages:** 9 (New Lead → Contacted → Qualification → Discovery → Proposal → Negotiation → Contract Review → Won/Lost)  
**Team:** 3 (Rep A — Optical, Rep B — PPE, one wholesale associate — support)  
**Stale Threshold:** 14 days  
**High-Priority Threshold:** $25K+  
**Follow-Up Cadence:** 3-day, 7-day, 14-day (manual, not automated yet)  
**Delivery:** Telegram (COO's chat)  
**Schedule:** 9 AM local, Mon-Sat

---

## Technical Implementation

### Stack
- **Language:** Python 3.9+
- **CRM API:** HubSpot REST API v3
- **Scheduler:** OpenClaw cron (fallback to system cron)
- **Delivery:** Telegram Bot API
- **Hosting:** Small VPS

### Key Scripts
- `scripts/daily_pipeline.py` — Daily monitoring report
- `generate_pipeline_report.py` — Weekly comprehensive report
- `scripts/hubspot/query_deals.py` — CRM queries

### Data Flow
1. Cron triggers agent turn (9 AM local)
2. Agent invokes `daily_pipeline.py` script
3. Script queries CRM API (deals, contacts, properties)
4. Script calculates metrics (stale deals, closing this week, top 5)
5. Script formats Telegram-optimized text
6. Script sends via Telegram Bot API
7. COO receives push notification on phone

### API Rate Limits
- HubSpot: 100 requests/10 sec, 40K requests/day
- Daily report uses ~15 API calls (well under limit)
- Weekly report uses ~25 API calls

---

## Operator Perspective

> "Before, the morning routine was logging into the CRM, filtering deals, and copying data into Slack — 15-20 minutes every day. Now it's a 2-minute brief that arrives on its own. The stale deal alerts alone rescued deals that would've gone cold, and the reps say it's like having a sales manager tracking their pipeline for them."

---

## Replicability

**This configuration can be replicated for:**
- ✅ Other B2B wholesale companies (optical, PPE, distribution)
- ✅ B2B manufacturers selling through dealers/distributors
- ✅ Professional services firms with proposal-based sales
- ✅ Any business with 20-100 active B2B deals, 1-10 rep team

**Configuration adjustments needed:**
- Stage names (map to your process)
- Stale thresholds (based on your sales velocity)
- High-priority amounts (based on your average deal size)
- Team structure (solo vs multi-rep)

**Time to deploy:** 1-2 weeks (including pilot)

---

## Lessons for Other Businesses

1. **Start simple, add later.** Deploy the daily report first (highest ROI). Add the weekly report after the team is trained; add scoring/automation after 4-6 weeks.
2. **Tune thresholds to your business.** Don't copy the 14-day stale threshold blindly. Test 7, 14, 21 days; watch the false positive rate; adjust.
3. **Push beats pull.** Telegram/Slack delivery (proactive) beats a dashboard (reactive). Make pipeline visibility a push, not a pull.
4. **Action items first.** Report structure matters. Start with "what needs attention today," not "here's all your data."
5. **Measure time saved, not features.** ROI comes from time saved and deals rescued (stale detection), not feature count.
6. **Team buy-in through transparency.** A shared footer listing every recipient drove accountability — everyone knew everyone saw the same data.

---

**Status:** Pattern extracted from a production deployment (8 weeks live at time of writing, April 2026).
