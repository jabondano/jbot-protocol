# Deployment Example: Content Engine at a Multi-Brand Consumer Company

> This example is drawn from a real deployment at a multi-channel consumer products company, anonymized. Figures are illustrative of a representative deployment of this system — they are not audited results and should not be attributed to any specific company.

---

## Deployment Context

**Company profile:** Multi-brand consumer products (DTC + B2B)
**Deployment window:** 10 weeks
**Team:** COO, Creative Director, CTO, ~10-person operations team

### The Challenge

**Pre-deployment state:**
- **4 brands:** Brand A (flagship), Brand B (safety line), Brand C (licensed third-party brand), Brand D (premium, pre-launch)
- **5 channels:** Meta Ads, Google Ads, Email, Wholesale/PR, Retail POS
- **Production method:** Mix of external agency ($200-300/asset, 2-3 week turnaround) + in-house designer
- **Volume:** 10-15 assets/month
- **Creative director time:** 20-25 hours/week
- **Pain points:**
  - Agency too slow and expensive for A/B testing
  - In-house designer bottleneck (can't scale past 15 assets/month)
  - No systematic creative refresh (running same ads until they burn out)
  - Manual QA (creative director reviews every single asset)
  - No performance feedback loop (which creative patterns win is only discovered after the fact)

**The breaking point:** A licensed-brand launch (Brand C) required 40+ new assets across all formats. The agency quote — roughly $12K and a 6-week timeline — would have blown the quarterly creative budget and missed the launch window.

---

## The Solution: Static Content Engine

### Implementation Timeline

**Week 1-2: Discovery & Planning**
- Interviewed the creative director to understand current workflow
- Mapped production resources (photo shoots, 3D renders, product photography, designer capacity)
- Defined tier structure (Hero → Campaign → Evergreen → Operational)
- Set QA dimensions (brand voice, product accuracy, hook clarity, visual quality, legal compliance, channel fit)

**Week 3-4: Schema & Workflow**
- Built Supabase `content_jobs` table (brief → approval → production → QA → review → live)
- Created brief templates for each brand + channel combination
- Configured production method decision tree
- Set up Telegram integration for approval workflow

**Week 5-6: AI Pipeline**
- Deployed background-removal + image-generation + compositing pipeline
- Built vision-model QA scoring (6 dimensions, 1-10 scale each)
- Set thresholds: ≥7 auto-approve, 5-6 flag for review, ≤4 auto-reject
- Pilot with Tier 3 (evergreen organic content, lowest risk)

**Week 7-8: Production & Iteration**
- First batch: 20 AI composites for organic social
- QA auto-route rate: 65% (lower than target, thresholds too strict)
- Adjusted thresholds based on real data
- Second batch: 25 assets, 82% auto-route (within target range)

**Week 9-10: Scale & Expansion**
- Expanded to Tier 4 (operational/promo resizes)
- Connected to 3D render pipeline (external agency API)
- Wired performance feedback loop (Meta Ads fatigue detection → refresh triggers)
- Full system live across all 4 brands

---

## The Configuration

### Brand Architecture
```json
{
  "brands": [
    {"id": "brand-a", "name": "Brand A (flagship)", "voice": ["casual", "direct", "aspirational"]},
    {"id": "brand-b", "name": "Brand B (safety)", "voice": ["direct", "technical", "bold"]},
    {"id": "brand-c", "name": "Brand C (licensed)", "voice": ["casual", "bold", "aspirational"]},
    {"id": "brand-d", "name": "Brand D (premium)", "voice": ["aspirational", "formal", "direct"]}
  ]
}
```

### Channel Strategy
- **Paid:** Meta (primary), Google (secondary), TikTok (testing)
- **Organic:** Instagram, TikTok, LinkedIn, Email
- **B2B/Wholesale:** Yes (requires Tier 1 hero assets)
- **Retail POS:** Yes (requires Tier 1 hero assets)

### Production Resources
- **Photo shoots:** Available ($3,500-$20,000/campaign, quarterly)
- **3D rendering:** External agency (all SKUs, all colorways)
- **Product photography:** Professional 4K (white background + lifestyle)
- **In-house designer:** 35 hrs/week
- **External agency:** Backup ($250/asset, 14-day turnaround)

### Tier Definitions
| Tier | Method | Use Cases | Cost/Asset |
|------|--------|-----------|------------|
| **1 - Hero** | Photo shoot | Campaign launches, PR, wholesale, licensed co-brand | $50-200 (amortized) |
| **2 - Campaign** | 3D render | All SKUs, colorway variants, product heroes | $30-50 |
| **3 - Evergreen** | AI composite | Social organic, A/B tests, email, background variants | $0.28 |
| **4 - Operational** | Designer/templates | Format resizes, copy swaps, promo badges | $15-30 |

### QA Framework
**Dimensions (weighted):**
1. Brand voice (20%) — Copy matches tone and messaging
2. Product accuracy (25%) — Product shown correctly, no misrepresentation
3. Hook clarity (15%) — Hook is clear, compelling, awareness-level matched
4. Visual quality (20%) — Lighting, composition, resolution meet standards
5. Legal compliance (10%) — No misleading claims, trademark issues
6. Channel fit (10%) — Format, aspect ratio, style appropriate

**Thresholds:**
- Auto-approve: All dimensions ≥7/10
- Flag for review: Any dimension 5-6/10
- Auto-reject: Any dimension ≤4/10

**Auto-route target:** 80%

### Approval Governance
- **Brief approver:** Creative Director
- **Channel:** Telegram
- **SLA:** <24 hours
- **Auto-approve scenarios:** None (all briefs reviewed)
- **Risk tolerance:** Moderate

---

## Illustrative Results

The ranges below represent what a deployment of this scale can expect after roughly 60 days. They are illustrative, not audited.

### Volume & Efficiency

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| **Assets/month** | 10-15 | 40-80 | 4-8x |
| **Cost/asset (AI tier)** | $150-300 | $0.28 | 500-1000x |
| **Cost/asset (blended)** | $150-300 | $5-50 | 3-60x |
| **Turnaround (AI tier)** | 2-3 weeks | <24 hours | 14-21x faster |
| **Creative director time** | 20-25 hrs/week | <6 hrs/week | 70-80% reduction |
| **QA auto-route rate** | 0% (manual) | 80% | Target achieved |
| **A/B test variants** | 1-2 per concept | 3-5 per concept | 3-5x |

### Economics (Representative Monthly)

**Before:**
- Agency: $2,000-4,000 (8-15 assets × $200-300)
- Designer: $2,500 (salary allocation)
- Photography: $1,000-3,000 (shoots amortized)
- **Total:** $5,500-9,500/month

**After:**
- AI pipeline: $8-22 (30-80 Tier 3 assets × $0.28)
- 3D renders: $600-1,200 (20-40 assets × $30-50)
- Designer: $1,500 (reduced allocation, focus on Tier 4)
- Photography: $1,000-3,000 (shoots unchanged, more reuse)
- **Total:** $3,100-5,700/month

**Savings:** $2,400-3,800/month (40-60% reduction)

### Creative Testing Impact

- **Before:** 1-2 variants per concept, refresh every 30-45 days
- **After:** 3-5 variants per concept, refresh every 10-14 days
- Fatigue detection automated (frequency + days > threshold → flag for refresh)
- Winning ad identification faster (more variants = better signal)

**Illustrative example:** One AI composite ad (safety-line product, problem-aware hook) hit a high single-digit ROAS at modest daily spend and was scaled within 48 hours. Total production cost: $0.28.

---

## Key Learnings

### 1. Tier Discipline Matters

**Early mistake:** Tried to use AI (Tier 3) for everything, including hero campaign assets.

**Problem:** AI composites can't match photo shoot quality for high-stakes launches (PR, wholesale, co-brand partnerships).

**Fix:** Defined tier use cases explicitly:
- Tier 1 = hero moments only (quarterly campaigns, PR, wholesale)
- Tier 2 = product catalog (3D renders for all SKUs)
- Tier 3 = volume (social organic, tests, email)
- Tier 4 = operational (resizes, promos)

**Result:** Right production method for the right job. No more forcing AI where it doesn't belong.

### 2. QA Thresholds Need Real Data

**Initial thresholds:** ≥8 auto-approve, 6-7 flag, ≤5 reject

**Problem:** Only 35% auto-routing (way below the 80% target). Creative director still reviewing 65% of assets.

**Root cause:** Thresholds set on intuition, not data. "8/10 visual quality" was too strict for social organic.

**Fix:** Ran 50 assets through QA, reviewed the creative director's actual approvals, adjusted thresholds to match her real decision pattern.

**New thresholds:** ≥7 auto-approve, 5-6 flag, ≤4 reject

**Result:** 80% auto-routing achieved. The creative director only sees edge cases.

### 3. Approval Gates BEFORE Production Costs

**Early mistake:** Generate asset → QA score → human review → approve/reject

**Problem:** A rejection after production has already spent $0.28-$50 on the asset.

**Fix:** Brief approval BEFORE production. The creative director approves the direction (hook, visual, tier) before money is spent.

**Result:** Rejections at brief stage cost $0.001 (LLM call). Rejections after production cost $0.28-$50. A 100x cost difference.

### 4. Performance Feedback Loop Creates Compound Value

**Initial system:** Brief → Approve → Produce → QA → Review → Live

**Missing:** What happens after "Live"? Does the asset perform? Should the system make more like it?

**Added:** Performance tracking (ad-platform fatigue detection) feeding back into brief generation:
- A monitor watches all live ads for frequency + days + impressions
- When an ad crosses the fatigue threshold → writes a signal to the database
- The next brief generation run reads signals → "Refresh ad XYZ" becomes a brief
- Winning ad patterns inform future briefs ("Hook type A outperforms B by 3x")

**Result:** The system gets smarter over time. It learns which creative patterns work.

### 5. Human Judgment Gates Are Non-Negotiable

**Temptation:** "80% auto-routing is good. Can we get to 95%? 100%?"

**Answer:** No. And you shouldn't want to.

**Why:** The 20% that need human review are:
- Edge cases QA scoring can't evaluate (e.g., "Does this scene feel authentic?")
- High-stakes assets (campaign launches, wholesale hero shots)
- Brand moments (new product debuts, seasonal themes)

**The creative director's role:** Define the taste system. QA enforces consistency within that system, but a human defines what "good" means.

**If you automate away human judgment:** You get consistent mediocrity at scale.

---

## Challenges & Solutions

### Challenge 1: Creative Director Resistance

**Problem:** Initial skepticism — "AI can't match my judgment. I need to review everything."

**Solution:**
1. **Pilot with low stakes:** Started with Tier 3 organic social (not paid ads)
2. **Showed QA scoring:** "Here's how the system evaluates. Does this match your criteria?"
3. **Tuned thresholds to her pattern:** Adjusted based on real approvals
4. **Proved time savings:** After 2 weeks, she'd reviewed 20% instead of 100% of assets
5. **Emphasized her role:** "You define taste. QA enforces consistency. You still own the brand."

**Outcome:** Full buy-in after a 3-week pilot.

### Challenge 2: Brand Consistency Across 4 Brands

**Problem:** Each brand has a different voice (Brand A = casual, Brand B = technical, Brand C = bold, Brand D = aspirational). How does QA know which is which?

**Solution:**
1. **Brand-specific QA rubrics:** The creative director defined "brand voice" criteria per brand
2. **Example assets:** Tagged 10 "perfect" assets per brand as references
3. **Weighted scoring:** Brand voice weighted 20% for all brands, but the definition varies
4. **Continuous tuning:** After each batch, rejected assets are reviewed and criteria refined

**Outcome:** QA differentiates brand voices at roughly 85% accuracy (measured by creative-director agreement rate).

### Challenge 3: Licensed-Brand Compliance (Brand C)

**Problem:** The licensed partner has strict brand guidelines (logo usage, color palette, imagery rules). Non-compliant assets going live is not an acceptable risk.

**Solution:**
1. **Tier 1 only:** All licensed-brand assets require photo shoot or designer (no AI tier)
2. **Additional QA dimension:** "Licensor compliance" (30% weight, blocking if <7)
3. **Manual review:** All licensed-brand assets flagged for final approval (no auto-route)
4. **Partnership clause:** The licensor approves all hero assets before launch

**Outcome:** Zero compliance issues in the launch quarter.

---

## ROI Calculation (Illustrative)

### Time Savings

**Creative director time:**
- Before: 20-25 hrs/week → After: <6 hrs/week → Savings: 14-19 hrs/week

At a representative $75/hr creative-director rate:
14 hrs/week × 52 weeks × $75/hr = **~$54,600/year**

### Cost Savings

**Production cost reduction:** $2,400-3,800/month

**Annual savings:** **$28,800-45,600/year**

### Revenue Impact (Harder to Measure)

Faster creative testing (more variants, faster refresh cadence) can plausibly improve blended ROAS by 10-15%. At a representative $15K/month ad spend, that's roughly **$18,000-27,000/year** in additional revenue. Treat this as directional, not measured.

### Total

**Illustrative annual benefit:** $101,400-127,200/year
**Operating cost:** ~$100-200/month in API costs (vision QA, image generation) = $1,200-2,400/year
**Illustrative ROI:** **42-106x**

---

## What Would We Do Differently

### 1. Start with Intake First

The system was built through discovery (weeks 1-6), then retrofitted into a configurable framework.

**Better approach:** Run the intake questionnaire FIRST, generate config, deploy from config. Faster deployment (days vs weeks), fewer rework cycles, clearer expectations.

### 2. Pilot with Multiple Tiers Simultaneously

Piloting Tier 3 alone proved "AI can do organic social" but not "AI can handle volume across all use cases."

**Better approach:** Pilot 2-3 tiers at once (e.g., Tier 3 evergreen + Tier 4 operational + one Tier 2 campaign). Proves broader value faster, builds confidence in the full system.

### 3. Set the Performance Feedback Loop from Day 1

Fatigue detection and performance tracking were added in week 9-10, so the first 8 weeks of briefs weren't informed by performance data.

**Better approach:** Wire performance APIs during week 3-4 schema setup. Even if brief generation doesn't use the data yet, start collecting immediately — by week 9-10 you'd have 6-8 weeks of history to inform brief improvements.

---

## Operator Perspective

> "The creative director was spending 80% of her time on routine approvals and 20% on creative strategy. Within weeks, that flipped — she focuses on the 20% that actually needs her judgment, and the system handles the rest. The licensed-brand launch that would have cost five figures and six weeks with an agency shipped in 10 days at a fraction of the cost, and the creative performed better because we could A/B test 3x more variants. And the feedback loop is where it gets interesting: the system isn't just executing a workflow — it's learning which creative patterns work and feeding that back into future briefs."

---

## Scalability

**Steady state:**
- 40-80 assets/month across 4 brands
- <6 hrs/week creative director time
- 80% QA auto-routing

**Capacity headroom:**
- Could scale to 150-200 assets/month with no additional human time
- Bottleneck is Tier 1 (photo shoots) and Tier 2 (3D agency capacity)
- Tier 3 + 4 (AI + templates) are effectively unlimited

**Natural expansions:**
- New brand launches (new positioning, new creative direction, same workflow)
- International markets: localized copy
- Video variants: extend to short-form video (9:16) using the same workflow

---

## Conclusion

The Static Content Engine transformed creative production from a **bottleneck** into a **competitive advantage**.

**Before:** Creative was slow, expensive, and limited the ability to test.
**After:** Creative is fast, cheap, and testing can be aggressive.

**The unlock wasn't the AI.** It was the **system**:
- Tiered production (right method for right job)
- Automated QA (free the creative director for high-value work)
- Performance feedback loop (system learns what works)
- Human judgment gates (taste can't be automated)

**This isn't about replacing creative directors.** It's about **amplifying them**. The creative director went from executing 15 assets/month herself to directing a system that produces 40-80 assets/month — in less time.

---

**Deployment window:** 10 weeks
**Extracted for JBOT Protocol:** April 2026
**Available at:** github.com/jabondano/jbot-protocol
