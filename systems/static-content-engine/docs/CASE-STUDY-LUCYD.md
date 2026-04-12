# Case Study: Static Content Engine at Lucyd
*How a NASDAQ-listed eyewear company automated creative production at scale*

---

## Company Context

**Company:** Innovative Eyewear Inc (NASDAQ: LUCY)  
**Industry:** Smart eyewear (DTC + B2B)  
**Deployment:** January - March 2026  
**Team:** COO, Creative Director, CTO, 10-person operations team

### The Challenge

**Pre-deployment state (December 2025):**
- **4 brands:** Lucyd Lyte (flagship), Lucyd Armor (safety), Reebok Eyewear (licensed), AERO (premium, launching 2026)
- **5 channels:** Meta Ads, Google Ads, Email (Klaviyo), Wholesale/PR, Retail POS
- **Production method:** Mix of external agency ($200-300/asset, 2-3 week turnaround) + in-house designer
- **Volume:** 10-15 assets/month
- **Creative director time:** 20-25 hours/week
- **Pain points:**
  - Agency too slow and expensive for A/B testing
  - In-house designer bottleneck (can't scale past 15 assets/month)
  - No systematic creative refresh (running same ads until they burn out)
  - Manual QA (creative director reviews every single asset)
  - No performance feedback loop (don't know which creative patterns win until after the fact)

**The breaking point:** Reebok Eyewear launch (Q1 2026) required 40+ new assets across all formats. Agency quoted $12K and 6-week timeline. That would blow the quarterly creative budget and miss the launch window.

---

## The Solution: Static Content Engine

### Implementation Timeline

**Week 1-2: Discovery & Planning**
- Interviewed creative director (Lilia) to understand current workflow
- Mapped production resources (photo shoots, 3D renders, product photography, designer capacity)
- Defined tier structure (Hero → Campaign → Evergreen → Operational)
- Set QA dimensions (brand voice, product accuracy, hook clarity, visual quality, legal compliance, channel fit)

**Week 3-4: Schema & Workflow**
- Built Supabase `content_jobs` table (brief → approval → production → QA → review → live)
- Created brief templates for each brand + channel combination
- Configured production method decision tree
- Set up Telegram integration for approval workflow

**Week 5-6: AI Pipeline**
- Deployed BiRefNet + Nano Banana 2 + Pillow composite pipeline
- Built Claude Sonnet vision QA scoring (6 dimensions, 1-10 scale each)
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
    {"id": "lyte", "name": "Lucyd Lyte", "voice": ["casual", "direct", "aspirational"]},
    {"id": "armor", "name": "Lucyd Armor", "voice": ["direct", "technical", "bold"]},
    {"id": "reebok", "name": "Reebok Eyewear", "voice": ["casual", "bold", "aspirational"]},
    {"id": "aero", "name": "AERO", "voice": ["aspirational", "formal", "direct"]}
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
| **1 - Hero** | Photo shoot | Campaign launches, PR, wholesale, Reebok co-brand | $50-200 (amortized) |
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
- **Brief approver:** Lilia (Creative Director)
- **Channel:** Telegram
- **SLA:** <24 hours
- **Auto-approve scenarios:** None (all briefs reviewed)
- **Risk tolerance:** Moderate

---

## Results (After 60 Days)

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

### Economics

**Before (monthly):**
- Agency: $2,000-4,000 (8-15 assets × $200-300)
- Designer: $2,500 (salary allocation)
- Photography: $1,000-3,000 (shoots amortized)
- **Total:** $5,500-9,500/month

**After (monthly):**
- AI pipeline: $8-22 (30-80 Tier 3 assets × $0.28)
- 3D renders: $600-1,200 (20-40 assets × $30-50)
- Designer: $1,500 (reduced allocation, focus on Tier 4)
- Photography: $1,000-3,000 (shoots unchanged, more reuse)
- **Total:** $3,100-5,700/month

**Savings:** $2,400-3,800/month (40-60% reduction)

### Creative Performance

**Meta Ads creative testing (March 2026):**
- **Before:** 1-2 variants per concept, refresh every 30-45 days
- **After:** 3-5 variants per concept, refresh every 10-14 days
- **Impact:** 
  - Fatigue detection automated (frequency + days > threshold → flag for refresh)
  - Creative refresh cadence 2-3x faster
  - Winning ad identification faster (more variants = better signal)

**Example:** One AI composite ad (Armor safety glasses, problem-aware hook) hit 8.34x ROAS at $52/day spend. Scaled to $200/day within 48 hours. Total production cost: $0.28.

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

---

### 2. QA Thresholds Need Real Data

**Initial thresholds:** ≥8 auto-approve, 6-7 flag, ≤5 reject

**Problem:** Only 35% auto-routing (way below 80% target). Creative director still reviewing 65% of assets.

**Root cause:** Thresholds set based on intuition, not real data. "8/10 visual quality" was too strict for social organic.

**Fix:** Ran 50 assets through QA, reviewed Lilia's actual approvals, adjusted thresholds to match her real decision pattern.

**New thresholds:** ≥7 auto-approve, 5-6 flag, ≤4 reject

**Result:** 80% auto-routing achieved. Lilia only sees edge cases.

---

### 3. Approval Gates BEFORE Production Costs

**Early mistake:** Generate asset → QA score → Lilia reviews → approve/reject

**Problem:** If Lilia rejects, we already spent $0.28-$50 producing it.

**Fix:** Brief approval BEFORE production. Lilia approves the creative direction (hook, visual, tier) before we spend money.

**Result:** Rejections at brief stage cost $0.001 (LLM call). Rejections after production cost $0.28-$50. 100x cost difference.

---

### 4. Performance Feedback Loop Creates Compound Value

**Initial system:** Brief → Approve → Produce → QA → Review → Live

**Missing:** What happens after "Live"? Does the asset perform well? Should we make more like it?

**Added:** Performance tracking (Meta Ads API fatigue detection) → feeds back into brief generation

**How it works:**
- mktgbot monitors all live ads for frequency + days + impressions
- When ad crosses fatigue threshold → writes signal to Supabase
- Next brief generation run → reads signals → "Refresh ad XYZ" becomes a brief
- Winning ad patterns → inform future briefs ("Hook type A outperforms B by 3x")

**Result:** System gets smarter over time. Learns which creative patterns work.

---

### 5. Human Judgment Gates Are Non-Negotiable

**Temptation:** "80% auto-routing is good. Can we get to 95%? 100%?"

**Answer:** No. And you shouldn't want to.

**Why:** The 20% that need human review are:
- Edge cases QA scoring can't evaluate (e.g., "Does this scene feel authentic?")
- High-stakes assets (campaign launches, wholesale hero shots)
- Brand moments (new product debuts, seasonal themes)

**Lilia's role:** Define the taste system. QA enforces consistency within that system, but Lilia defines what "good" means.

**If you automate away human judgment:** You get consistent mediocrity at scale.

**The goal:** Free creative director from routine review (80% auto-routed) so they can focus on high-value judgment (20% edge cases + tier 1 direction).

---

## Challenges & Solutions

### Challenge 1: Creative Director Resistance

**Problem:** Lilia initially skeptical. "AI can't match my judgment. I need to review everything."

**Solution:**
1. **Pilot with low stakes:** Started with Tier 3 organic social (not paid ads)
2. **Showed QA scoring:** "Here's how the system evaluates. Does this match your criteria?"
3. **Tuned thresholds to her pattern:** Adjusted based on her real approvals
4. **Proved time savings:** After 2 weeks, she'd reviewed 20% instead of 100% of assets
5. **Emphasized her role:** "You define taste. QA enforces consistency. You still own the brand."

**Outcome:** Full buy-in after 3-week pilot. Now advocates for the system.

---

### Challenge 2: Brand Consistency Across 4 Brands

**Problem:** Each brand has different voice (Lyte = casual, Armor = technical, Reebok = bold, AERO = aspirational). How does QA know which is which?

**Solution:**
1. **Brand-specific QA rubrics:** Lilia defined "brand voice" criteria per brand
2. **Example assets:** Tagged 10 "perfect" assets per brand as references
3. **Weighted scoring:** Brand voice weighted 20% for all brands, but definition varies
4. **Continuous tuning:** After each batch, Lilia reviews rejected assets → refines criteria

**Outcome:** QA can differentiate Lyte vs Armor vs Reebok voice with 85% accuracy (measured by Lilia agreement rate).

---

### Challenge 3: Reebok Co-Brand Compliance

**Problem:** Reebok has strict brand guidelines (logo usage, color palette, athlete imagery rules). Can't risk non-compliant assets going live.

**Solution:**
1. **Tier 1 only:** All Reebok assets require photo shoot or designer (no AI tier)
2. **Additional QA dimension:** "Reebok compliance" (30% weight, blocking if <7)
3. **Lilia manual review:** All Reebok assets flagged for final approval (no auto-route)
4. **Partnership clause:** Reebok approves all hero assets before launch

**Outcome:** Zero compliance issues in Q1 2026 launch.

---

## ROI Calculation

### Time Savings

**Creative director time:**
- Before: 20-25 hrs/week
- After: <6 hrs/week
- Savings: 14-19 hrs/week

**Hourly rate equivalent:** $75/hr (creative director market rate)

**Annual savings:** 14 hrs/week × 52 weeks × $75/hr = **$54,600/year**

### Cost Savings

**Production cost reduction:**
- Before: $5,500-9,500/month
- After: $3,100-5,700/month
- Savings: $2,400-3,800/month

**Annual savings:** $2,400-3,800/month × 12 = **$28,800-45,600/year**

### Revenue Impact (Harder to Measure)

**Faster creative testing:**
- More A/B variants per concept (3-5 vs 1-2)
- Faster refresh cadence (10-14 days vs 30-45 days)
- **Estimated impact:** 10-15% improvement in blended ROAS (from 2.8x to 3.1-3.2x)
- **At $15K/month ad spend:** $1,500-2,250/month additional revenue = **$18,000-27,000/year**

### Total ROI

**Annual benefit:** $54,600 (time) + $28,800-45,600 (cost) + $18,000-27,000 (revenue) = **$101,400-127,200/year**

**System cost:** 
- Deployment: $0 (built in-house via existing team)
- Operating: ~$100-200/month (API costs: Claude Sonnet QA, image generation)
- **Annual operating cost:** $1,200-2,400/year

**ROI:** $101,400-127,200 / $1,200-2,400 = **42-106x**

---

## What Would We Do Differently

### 1. Start with Intake First

We built the system through discovery (weeks 1-6) then retrofitted it into a configurable framework.

**Better approach:** Run the intake questionnaire FIRST, generate config, deploy from config.

**Why:** Faster deployment (days vs weeks), fewer rework cycles, clearer expectations.

---

### 2. Pilot with Multiple Tiers Simultaneously

We piloted Tier 3 (evergreen) only, then expanded to Tier 4, then Tier 2.

**Problem:** Tier 3 alone doesn't prove the full system value. Creative director saw "AI can do organic social" but not "AI can handle volume across all use cases."

**Better approach:** Pilot with 2-3 tiers at once (e.g., Tier 3 evergreen + Tier 4 operational + one Tier 2 campaign).

**Why:** Proves broader value faster, builds confidence in full system.

---

### 3. Set Performance Feedback Loop from Day 1

We added fatigue detection + performance tracking in Week 9-10.

**Problem:** First 8 weeks of briefs weren't informed by performance data. Missed opportunity to learn faster.

**Better approach:** Wire performance APIs (Meta, Google, Klaviyo) during Week 3-4 schema setup. Even if brief generation doesn't use it yet, start collecting data immediately.

**Why:** By Week 9-10, you'd have 6-8 weeks of historical data to inform brief improvements.

---

## Quotes from the Team

**Lilia (Creative Director):**
> "I was skeptical at first. But after two weeks, I realized I was spending 80% of my time on routine approvals and only 20% on creative strategy. Now it's flipped. I focus on the 20% that actually needs my judgment, and the system handles the rest. I can finally do the job I was hired for."

**Joaquin (COO):**
> "The Reebok launch would have cost us $12K and 6 weeks with the agency. We did it in 10 days for under $2K, and the creative performed better because we could A/B test 3x more variants. That's when I knew this wasn't just a cost-saving tool — it's a competitive advantage."

**Eric (CTO):**
> "The performance feedback loop is where it gets interesting. The system isn't just executing a workflow — it's learning which creative patterns work and feeding that back into future briefs. That's not automation. That's intelligence."

---

## Scalability

**Current state (April 2026):**
- 40-80 assets/month across 4 brands
- <6 hrs/week creative director time
- 80% QA auto-routing

**Capacity headroom:**
- Could scale to 150-200 assets/month with no additional human time
- Bottleneck is Tier 1 (photo shoots) and Tier 2 (3D agency capacity)
- Tier 3 + 4 (AI + templates) are effectively unlimited

**Next expansion:**
- AERO launch (Summer 2026): New brand, new positioning, new creative direction
- International markets: Localized copy (Spanish, French, German)
- Video variants: Extend to short-form video (9:16 Reels, TikTok) using same workflow

---

## Conclusion

The Static Content Engine transformed creative production from a **bottleneck** into a **competitive advantage**.

**Before:** Creative was slow, expensive, and limited our ability to test.  
**After:** Creative is fast, cheap, and we can test more aggressively.

**The unlock wasn't the AI.** It was the **system**:
- Tiered production (right method for right job)
- Automated QA (free creative director for high-value work)
- Performance feedback loop (system learns what works)
- Human judgment gates (taste can't be automated)

**This isn't about replacing creative directors.** It's about **amplifying them**.

Lilia went from executing 15 assets/month herself to **directing a system** that produces 40-80 assets/month — and she's spending less time doing it.

That's the future of creative operations.

---

**System deployed:** January - March 2026  
**Company:** Innovative Eyewear Inc (NASDAQ: LUCY)  
**Extracted for JBOT Protocol:** April 2026  
**Available at:** github.com/jabondano/jbot-protocol

*Wispr · jabondano · 🦞*
