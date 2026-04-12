# Configuration Comparison
*How the same system adapts to different businesses*

---

## Overview

This document shows how three different businesses configure the Static Content Engine for their specific needs.

**Same universal workflow:**
```
BRIEF → APPROVE BRIEF → PRODUCE → QA → REVIEW → LIVE
```

**Different configurations:**
- Production methods (shoots vs AI vs designer vs templates)
- QA dimensions (what matters for brand quality)
- Approval governance (who approves, how fast)
- Volume targets (10/month vs 60/month)
- Cost thresholds ($0.28 vs $25 vs $100 per asset)

---

## The Three Companies

| Company | Industry | Brands | Monthly Volume | Primary Channels |
|---------|----------|--------|----------------|------------------|
| **Eyewear E-commerce** | DTC, B2B | 4 brands | 60 assets | Meta, Google, Email, Wholesale, Retail |
| **Apparel DTC** | DTC | 1 brand | 25 assets | Meta, TikTok, Pinterest, Email |
| **B2B SaaS** | B2B | 1 brand | 15 assets | LinkedIn, Google, Blog, Email |

---

## Production Capabilities Comparison

### Eyewear E-commerce
**Resources:**
- ✅ Photo shoots ($15K/shoot, campaign-only)
- ✅ Professional product photography (4K quality)
- ✅ 3D rendering (external agency)
- ✅ In-house designer (35 hrs/week)
- ✅ External agency (backup, $250/asset)

**Tier Strategy:**
- **Tier 1 (Hero):** Photo shoots — $50-200/asset
- **Tier 2 (Campaign):** 3D renders — $30-50/asset
- **Tier 3 (Evergreen):** AI composites — $0.28/asset
- **Tier 4 (Operational):** Designer/templates — $15-30/asset

**Why:** Multi-brand, multi-channel requires flexibility. Shoots for hero assets (PR/wholesale), 3D for scale (all SKUs), AI for volume (social/tests), designer for operational.

---

### Apparel DTC
**Resources:**
- ❌ No photo shoots (can't afford)
- ✅ Product photography (1080p quality)
- ❌ No 3D rendering
- ❌ No in-house designer
- ❌ No agency

**Tier Strategy:**
- **Tier 1 (Hero):** UGC creators — $5-10/asset
- **Tier 2 (Campaign):** AI composites — $0.50/asset
- **Tier 3 (Evergreen):** AI composites — $0.28/asset
- **Tier 4 (Operational):** Automated templates — $0/asset

**Why:** Bootstrapped, founder-run, no design team. AI-first strategy + UGC for authenticity. All four tiers use AI or automation.

**Creative Director Hours:** 3 hrs/week (vs 6 for eyewear, 8 for SaaS)

---

### B2B SaaS
**Resources:**
- ❌ No photo shoots
- ✅ Product screenshots (4K quality)
- ❌ No 3D rendering
- ✅ In-house designer (20 hrs/week)
- ❌ No agency

**Tier Strategy:**
- **Tier 1 (Hero):** Designer — $50-100/asset
- **Tier 2 (Campaign):** Designer — $25-50/asset
- **Tier 3 (Evergreen):** Designer — $10-25/asset
- **Tier 4 (Operational):** Automated templates — $0/asset

**Why:** B2B professional context, technical accuracy matters more than volume. Designer creates all assets (no AI trust for technical claims), automation only for resizes.

**Creative Director Hours:** 8 hrs/week (more review time, lower volume)

---

## QA Dimensions Comparison

### Eyewear E-commerce
1. **Brand voice** (20%) — Copy matches tone
2. **Product accuracy** (25%) — Product shown correctly
3. **Hook clarity** (15%) — Awareness-level match
4. **Visual quality** (20%) — Lighting/composition
5. **Legal compliance** (10%) — No misleading claims
6. **Channel fit** (10%) — Format/style appropriate

**Thresholds:** ≥7 approve, 6 flag, ≤4 reject  
**Auto-route target:** 80%

**Why:** Mix of technical (product accuracy) and creative (hook, visual). Legal matters (health claims prohibited). High volume needs high auto-route rate.

---

### Apparel DTC
1. **Brand voice** (20%) — Playful, casual tone
2. **Lifestyle fit** (25%) — Model/scene matches customer
3. **Hook clarity** (15%) — Awareness-level match
4. **Visual quality** (20%) — Social media standards
5. **Trend relevance** (10%) — Aligns with current trends
6. **Channel fit** (10%) — Platform-appropriate style

**Thresholds:** ≥6 approve, 5 flag, ≤3 reject  
**Auto-route target:** 85%

**Why:** Lower bar (6 vs 7) because bootstrapped, aggressive testing. "Lifestyle fit" and "trend relevance" replace "product accuracy" and "legal compliance" (apparel is less regulated). Higher auto-route target (85% vs 80%) to save founder time.

---

### B2B SaaS
1. **Technical accuracy** (30%) — Features correctly represented
2. **Value prop clarity** (25%) — Clear benefit statement
3. **CTA strength** (15%) — Strong call-to-action
4. **Professional tone** (15%) — Trustworthy voice
5. **Visual quality** (10%) — Clean, readable design
6. **Channel fit** (5%) — LinkedIn-appropriate

**Thresholds:** ≥8 approve, 7 flag, ≤5 reject  
**Auto-route target:** 60%

**Why:** Higher bar (8 vs 7 vs 6) because B2B trust matters. "Technical accuracy" weighted 30% (highest of all dimensions across all examples). Lower auto-route target (60% vs 80% vs 85%) because manual review matters more than speed.

**Unique validation:** "No technical claims without proof/citation" is a blocker (not just warning).

---

## Approval Governance Comparison

### Eyewear E-commerce
**Brief Approver:** Creative Director (Lilia)  
**Channel:** Telegram  
**SLA:** <24 hours  
**Auto-approve:** None (all briefs reviewed)  
**Risk tolerance:** Moderate

**Why:** Professional creative director role exists. Telegram for speed. 24h SLA balances speed + quality. No auto-approvals for paid ads (risk management).

---

### Apparel DTC
**Brief Approver:** Founder  
**Channel:** Slack  
**SLA:** <48 hours  
**Auto-approve:** Format resizes, tier-4 operational  
**Risk tolerance:** Aggressive

**Why:** Founder wears all hats. 48h SLA (slower, but acceptable for bootstrapped pace). Auto-approves low-risk operational work to save time. Aggressive risk tolerance (test fast, learn fast).

---

### B2B SaaS
**Brief Approver:** Marketing Director  
**Channel:** Email  
**SLA:** <72 hours  
**Auto-approve:** Format resizes, tier-4 operational  
**Risk tolerance:** Conservative

**Why:** Corporate structure (Director, not founder). Email for formal documentation. 72h SLA (weekly batch reviews acceptable). Conservative risk tolerance (B2B trust takes years to build, seconds to lose).

---

## Volume & Economics Comparison

| Metric | Eyewear | Apparel | B2B SaaS |
|--------|---------|---------|----------|
| **Monthly volume** | 60 assets | 25 assets | 15 assets |
| **Target cost/asset** | $5 | $3 | $25 |
| **Monthly budget** | $7,000 | $1,200 | $3,000 |
| **Creative director hrs/week** | 6 | 3 | 8 |
| **Auto-route %** | 80% | 85% | 60% |
| **Turnaround target** | 24 hours | 48 hours | 72 hours |
| **Fatigue threshold (days)** | 10 | 7 | 14 |

**Observations:**

**Eyewear** = high volume, mid cost, tight turnaround
- Multi-brand, multi-channel drives volume
- Mix of tiers (shoots + AI) drives mid-range cost
- Creative director balances 6 hrs/week across 60 assets (6 min/asset)

**Apparel** = mid volume, low cost, fast iteration
- Bootstrapped forces low cost ($3/asset target)
- AI-first strategy enables fast turnaround
- Founder spends 3 hrs/week (lowest absolute time)

**B2B SaaS** = low volume, high cost, quality-first
- B2B sales cycles are long, less creative needed
- Designer-made assets cost more, but higher trust
- Marketing Director spends 8 hrs/week (highest per-asset review time)

---

## Audience Strategy Comparison

### Eyewear E-commerce
**Primary Awareness:** Level 2 (Problem-Aware)  
**Secondary:** Level 3 (Solution-Aware)  
**ICPs:** 7 distinct personas  
**Funnel:** 4 stages (awareness → consideration → decision → retention)

**Why:** Diverse customer base (DTC consumer, B2B safety manager, prescription upgrader). Most are problem-aware (know they want hands-free audio) but haven't discovered smart glasses yet.

---

### Apparel DTC
**Primary Awareness:** Level 1 (Problem-Unaware)  
**Secondary:** Level 2 (Problem-Aware)  
**ICPs:** 3 distinct personas  
**Funnel:** 3 stages (awareness → consideration → decision)

**Why:** Fashion discovery is impulse-driven. Level 1 (problem-unaware) means hooks lead with emotion/desire, not product features. Simpler funnel (no retention stage yet, too early-stage).

---

### B2B SaaS
**Primary Awareness:** Level 3 (Solution-Aware)  
**Secondary:** Level 4 (Product-Aware)  
**ICPs:** 3 distinct personas (all decision-makers)  
**Funnel:** 3 stages (awareness → consideration → decision)

**Why:** B2B buyers actively research solutions (Level 3). They know the category, comparing vendors. Higher awareness level = more technical/feature-focused creative, less emotional.

---

## Technical Integration Comparison

| Integration | Eyewear | Apparel | B2B SaaS |
|-------------|---------|---------|----------|
| **Storage** | Supabase | Cloudflare R2 | AWS S3 |
| **Delivery** | Meta API, Klaviyo, Manual | Meta API, Manual | LinkedIn API, Google API, HubSpot, Manual |
| **Data sources** | Meta, Shopify, Klaviyo | Meta, Shopify | LinkedIn, Google, HubSpot, GA4 |
| **OpenClaw deployed** | ✅ Yes | ✅ Yes | ✅ Yes |

**Why different storage:**
- Eyewear: Already on Supabase for bot coordination → reuse
- Apparel: Cloudflare Pages site → R2 makes sense
- B2B SaaS: Enterprise infra on AWS → S3 natural fit

**Why different delivery:**
- Eyewear: Agency handles Meta/Google delivery (Lilo Social) → API writes to agency platform
- Apparel: Founder manually uploads to TikTok/Pinterest (no APIs yet)
- B2B SaaS: HubSpot integration required (CRM attribution), LinkedIn + Google ads APIs

---

## Key Takeaways

### 1. Same System, Different Configuration

The 6-stage workflow is identical:
```
BRIEF → APPROVE → PRODUCE → QA → REVIEW → LIVE
```

But every parameter adapts:
- Eyewear uses all 4 production tiers
- Apparel collapses to AI + templates
- SaaS uses designer for everything except operational

### 2. QA Dimensions Reflect Business Priorities

**Eyewear:** Product accuracy (25%) — physical product must match
**Apparel:** Lifestyle fit (25%) — scene/model must resonate
**B2B SaaS:** Technical accuracy (30%) — claims must be provable

### 3. Risk Tolerance Drives Automation

**Aggressive (Apparel):** 85% auto-route, 6/10 approval threshold, founder approves in 48h  
**Moderate (Eyewear):** 80% auto-route, 7/10 threshold, creative director in 24h  
**Conservative (SaaS):** 60% auto-route, 8/10 threshold, marketing director in 72h

### 4. Volume ≠ Budget

**Eyewear:** 60 assets/month, $7K budget = $117/asset blended  
**Apparel:** 25 assets/month, $1.2K budget = $48/asset blended  
**B2B SaaS:** 15 assets/month, $3K budget = $200/asset blended

Eyewear has 4x the volume of SaaS but spends 2.3x the budget (economies of scale via AI tiers).

### 5. Creative Director Time Per Asset

**Apparel:** 3 hrs/week ÷ 25 assets = 7.2 min/asset  
**Eyewear:** 6 hrs/week ÷ 60 assets = 6 min/asset  
**B2B SaaS:** 8 hrs/week ÷ 15 assets = 32 min/asset

B2B spends 5x more review time per asset (manual validation of technical claims).

---

## What This Means For You

**Don't copy a configuration. Build yours.**

If you're:
- **DTC e-commerce (physical products)** → Start with eyewear config, adjust for your brand count
- **DTC e-commerce (apparel/fashion)** → Start with apparel config, adjust for your resources
- **B2B SaaS** → Start with SaaS config, adjust for your review process

**The intake questionnaire will guide you** through the configuration decisions.

**But the system is the same** — you're just dressing the skeleton in your specific clothes.

---

**Next:** Complete the intake questionnaire in `config/intake.md` to generate your configuration.
