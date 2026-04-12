# Static Content Engine
*Automated creative production from brief → approval → production → QA → delivery*

---

## What This Is

A configurable system that automates creative asset production at scale, used by a NASDAQ-listed company to produce 40-80 assets/month with <6 hours/week of creative director time.

**Not a tool. A complete operating system.**

---

## The Problem

Every company running paid ads, email campaigns, or content marketing faces the same bottleneck: **creative production can't keep up with distribution.**

You need:
- 20-40 new ad creatives per month to avoid fatigue
- A/B test variants (3-5 versions per concept)
- Multi-format (4:5, 9:16, 1:1, 16:9 for different channels)
- Brand-consistent (can't just ship whatever AI generates)
- Fast turnaround (campaigns launch weekly, not quarterly)

**Traditional solutions:**
- **Agency:** $200-500/asset, 2-3 week turnaround, expensive at scale
- **In-house designer:** $50-150/asset, faster but still bottlenecks on one person
- **Founder in Canva:** "Free" but costs 10-20 hrs/week, inconsistent quality
- **AI tools alone:** Fast and cheap, but no quality control → ships garbage

**All hit the same wall:** As volume increases, quality drops OR costs explode OR creative director burns out.

---

## The Solution

A **6-stage automated pipeline** with human judgment gates at the right points:

```
BRIEF → APPROVE BRIEF → PRODUCE → QA → REVIEW → LIVE
```

**What makes it work:**

1. **Tiered production** — Right method for the right job (photo shoot for hero, AI for variants)
2. **Automated QA** — 80% of assets auto-routed, only edge cases need human review
3. **Configured to your business** — Your brands, your channels, your approval process, your budget

**Result:** 10-40x cost reduction, 4-10x speed increase, 80% reduction in creative director time

---

## Real Numbers (Reference Implementation)

**Company:** Eyewear e-commerce (4 brands, 5 channels, DTC + B2B)

**Before:**
- Asset production: Agency + in-house designer
- Cost: $150-300/asset (agency), $50/asset (designer)
- Turnaround: 2-3 weeks (agency), 2-3 days (designer)
- Volume: 10-15 assets/month
- Creative director time: 20-25 hrs/week

**After:**
- Asset production: 4-tier system (shoots + 3D + AI + templates)
- Cost: $0.28/asset (AI), $15-30/asset (designer touch-ups), $50-200/asset (shoot amortized)
- Turnaround: <24 hours (AI), 2-3 days (designer), 1-2 weeks (shoots planned in advance)
- Volume: 40-80 assets/month
- Creative director time: <6 hrs/week

**ROI:** 4-8x volume increase, 10-50x cost reduction per asset, 75% time savings

---

## How It Works

### The 6-Stage Workflow

#### Stage 1: BRIEF
**What:** Define what to make, for whom, and why  
**Input:** Campaign need (launch, A/B test, refresh fatigued ad, seasonal)  
**Output:** Structured brief with hook options, visual direction, production method recommendation  
**Time:** 90 seconds (automated via `static-brief` skill)

#### Stage 2: APPROVE BRIEF
**What:** Human creative judgment BEFORE production costs  
**Who:** Creative director or delegated approver  
**Decision:** On-brand? Right hook for awareness level? Achievable?  
**Time:** 5-15 minutes (batched daily)

**Why this matters:** Catching bad creative direction at the brief stage costs $0.001 (LLM call). Catching it after production costs $0.28-$200 (wasted asset).

#### Stage 3: PRODUCE
**What:** Execute using the appropriate production method  
**Methods:** Photo shoot, 3D render, AI composite, Figma template  
**Decision tree:** Tier (hero/campaign/evergreen/operational) + time + budget + channel requirements  
**Time:** 5 minutes (AI) to 2 weeks (shoot) depending on method

#### Stage 4: QA
**What:** Automated quality scoring across 6 dimensions  
**Dimensions:** Brand voice, product accuracy, hook clarity, visual quality, legal compliance, channel fit  
**Routing logic:**
- All scores ≥7/10 → auto-approve, ship to delivery
- Any score 5-6/10 → flag for human review
- Any score ≤4/10 → auto-reject, regenerate

**Why this matters:** 80% of AI assets auto-route (score ≥7). Creative director only sees the 20% that need judgment.

#### Stage 5: REVIEW
**What:** Human final approval for flagged assets or high-stakes campaigns  
**Who:** Creative director  
**Frequency:** Batch reviews (not every single asset)  
**Time:** 1-2 hrs/week total

#### Stage 6: LIVE
**What:** Deliver to channel (Meta Ads, email platform, social scheduler, etc.)  
**Tracking:** Asset performance fed back into system for future brief generation  
**Learning:** High-performing hooks/visuals inform next batch of briefs

---

## Configuration: Your Business, Your System

The system is **universal** (6-stage workflow, tier hierarchy, QA framework), but **configured** to your specific context.

### What Gets Configured

**Brand Architecture:**
- How many brands? (1 or 20)
- Brand hierarchy (umbrella → sub-brands → product lines)
- Brand voice (formal, casual, technical, aspirational, etc.)
- Visual identity rules (colors, fonts, logo usage)

**Channel Strategy:**
- Which paid channels? (Meta, Google, TikTok, LinkedIn, etc.)
- Which organic channels? (IG, email, blog, etc.)
- Format priorities (4:5, 9:16, 1:1, 16:9)
- B2B/wholesale/retail needs

**Production Resources:**
- Can you do photo shoots? (yes/no/occasionally)
- Do you have product photography? (yes/no)
- 3D rendering capability? (yes/no)
- Budget constraints ($X/month, $Y/asset)

**Approval Governance:**
- Who approves briefs? (name, role, channel)
- Who approves finals? (same person or different?)
- Turnaround SLA (<24h, 1-3 days, weekly batch)
- Legal/compliance review needed? (yes/no)

**QA Framework:**
- Which dimensions matter? (brand voice, product accuracy, legal, etc.)
- Scoring thresholds (what's "good enough" vs "needs review")
- Validation methods (automated vision scoring, manual checklist, etc.)

**Performance Targets:**
- Volume needed (10/month, 40/month, 100/month)
- Cost targets ($X/asset)
- Speed targets (same-day, 3-day, 1-week)
- Fatigue thresholds (refresh ads after X days/impressions)

### How Configuration Works

**Step 1:** Complete the intake questionnaire (3 hours, one-time)  
**Step 2:** Generate your configuration file (automated)  
**Step 3:** Deploy the system with your config  
**Step 4:** Pilot with low-risk tier (e.g., organic social, not paid ads)  
**Step 5:** Iterate and expand to other tiers

---

## Repository Structure

```
static-content-engine/
├── README.md (this file)
├── docs/
│   ├── UNIVERSAL-WORKFLOW.md (the 6-stage pipeline)
│   ├── TIER-HIERARCHY.md (hero → campaign → evergreen → operational)
│   ├── QA-FRAMEWORK.md (scoring dimensions, routing logic)
│   ├── DEPLOYMENT-GUIDE.md (step-by-step setup)
│   └── CASE-STUDY.md (real implementation, anonymized)
├── config/
│   ├── schema.json (configuration schema definition)
│   ├── intake.md (questionnaire to generate config)
│   └── validation.js (config validator)
├── examples/
│   ├── eyewear-ecommerce.json (DTC, 4 brands, 5 channels)
│   ├── apparel-dtc.json (DTC, 1 brand, 3 channels)
│   └── b2b-saas.json (B2B, 1 brand, 4 channels)
├── schema/
│   ├── supabase-content_jobs.sql (database table template)
│   ├── brief-template.md (configurable brief structure)
│   └── qa-rubric.md (configurable QA dimensions)
└── skills/
    ├── static-brief/ (OpenClaw skill: brief generation)
    ├── static-produce/ (OpenClaw skill: AI production pipeline)
    └── static-qa/ (OpenClaw skill: automated QA scoring)
```

---

## Quick Start

### 1. Review Example Configurations

Start by looking at the example configs to understand how different businesses configure the same system:

- **`examples/eyewear-ecommerce.json`** — 4 brands, 5 channels, mix of shoots + 3D + AI
- **`examples/apparel-dtc.json`** — 1 brand, 3 channels, UGC + AI + templates
- **`examples/b2b-saas.json`** — 1 brand, 4 channels, designer + templates only

### 2. Complete the Intake

Read `config/intake.md` and answer the questions. This generates your configuration.

**Time required:** ~3 hours (can be done in 3 sessions)

### 3. Generate Configuration

Run the config generator:
```bash
node config/generate.js --intake your-intake-responses.json
```

Outputs: `your-company.config.json`

### 4. Deploy Database Schema

Apply the Supabase schema with your configuration:
```bash
psql $DATABASE_URL -f schema/supabase-content_jobs.sql \
  --set config_file=your-company.config.json
```

Creates: `content_jobs` table configured for your brands, channels, and tier structure

### 5. Install Skills

If using OpenClaw:
```bash
# Install the 3 skills
openclaw skills install ./skills/static-brief
openclaw skills install ./skills/static-produce
openclaw skills install ./skills/static-qa

# Configure with your company config
openclaw skills config static-brief --config your-company.config.json
```

### 6. Pilot One Tier

**Start with the lowest-risk, highest-volume tier** (usually Tier 3: Evergreen organic content)

**Why:** More forgiving (organic vs paid), higher volume (more data to learn from), lower stakes (easy to iterate)

**Run 10-20 assets through the full pipeline:**
- Generate briefs
- Approve briefs (batch)
- Produce assets
- QA scoring
- Review flagged items
- Deliver to channel

**Measure:**
- Time saved vs manual
- Cost per asset vs previous method
- Creative director hours vs previous
- QA accuracy (auto-approve rate)

### 7. Iterate & Expand

**After first 10-20 assets:**
- Refine QA thresholds (too many flagged? Lower threshold. Not enough? Raise it.)
- Tune brief templates (hooks not resonating? Adjust awareness level mapping.)
- Adjust tier assignments (assets going to wrong production method? Fix decision tree.)

**Then expand to next tier** (Tier 2: Campaign assets, or Tier 4: Operational/promo)

**Hold Tier 1 (Hero assets)** until the system is proven — those are highest stakes

---

## Who This Is For

### ✅ Good Fit

**DTC e-commerce brands:**
- Running paid ads (Meta, Google, TikTok)
- Need 20-40+ new creatives/month
- In-house designer or agency bottleneck
- Budget-conscious (can't afford $200/asset at scale)

**Agencies:**
- Managing creative for multiple clients
- High volume, tight turnaround
- Need to scale without hiring more designers
- Profitability squeezed by production costs

**B2B companies with content marketing:**
- LinkedIn, email, blog, landing pages
- Need consistent brand voice at scale
- Limited design resources
- Long sales cycles (lots of nurture content needed)

### ❌ Not a Fit (Yet)

**Early-stage startups (<$100K ARR):**
- Volume too low to justify automation
- Brand still evolving (configuration will change)
- Better to manual until you have patterns

**Premium/luxury brands:**
- Creative is the differentiator (can't automate taste)
- Shoot-first strategy (AI composites don't match brand bar)
- Better to keep fully manual with top-tier talent

**Video-first brands:**
- This system is static-only (images, not video)
- Video production has different economics and workflow
- Wait for `dynamic-content-engine` (coming later)

---

## What You Need

**Required:**
- OpenClaw runtime (or ability to run the skills independently)
- Supabase account (for `content_jobs` database)
- API access to your channels (Meta, Klaviyo, etc.)
- Product photography or ability to do shoots (for AI tiers)

**Helpful but not required:**
- Existing brand guidelines
- Historical ad performance data
- Design team (for touch-ups, not primary production)

**Nice to have:**
- 3D product renders
- UGC creator network
- Adobe Creative Cloud (for Figma/Photoshop integration)

---

## Roadmap

**Current state:** v1.0 — Proven with one reference implementation (eyewear e-commerce)

**Next:**
- v1.1 — Apparel DTC pilot (validate config portability)
- v1.2 — B2B SaaS pilot (validate non-e-commerce use case)
- v1.3 — Agency pilot (validate multi-client workflow)
- v2.0 — Self-serve deployment (config generator web UI)
- v3.0 — Performance feedback loop (winning assets inform future briefs)

---

## Support & Contribution

**Questions:** Open an issue in this repo  
**Feature requests:** Open an issue with `[Feature]` tag  
**Bug reports:** Open an issue with `[Bug]` tag  
**Consulting:** joaquin@jabondano.co (if you want help deploying this)

**Contributing:**
- Share your configuration (anonymized) as an example
- Document edge cases you hit during deployment
- Improve the intake questionnaire based on your experience
- Build integrations for platforms not yet supported

---

## License

MIT — Use it, fork it, adapt it, sell services around it.

Attribution appreciated but not required.

---

## Credits

Built by Joaquin Abondano as part of the JBOT Protocol.

Reference implementation developed at Innovative Eyewear Inc (NASDAQ: LUCY) over 10 weeks (Jan-Mar 2026).

Extracted and generalized Apr 2026.

---

**Next:** Read `docs/UNIVERSAL-WORKFLOW.md` to understand how the 6-stage pipeline works.
