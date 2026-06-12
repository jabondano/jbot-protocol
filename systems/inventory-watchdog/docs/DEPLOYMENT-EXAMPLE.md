# Deployment Example: Multi-Channel Consumer Hardware Company

> This example is drawn from a real deployment at a multi-channel consumer products company, anonymized. Figures are illustrative of a representative deployment of this system — they are not audited results and should not be attributed to any specific company.

---

## Company Profile (Anonymized)

**Industry:** Consumer hardware (DTC + Amazon + B2B wholesale)
**Products:** 4 product lines, ~900 active SKUs
**Team:** ~15 employees (COO, ops manager, warehouse team)

**Inventory complexity:**
- 15 warehouse locations (US, EU, APAC)
- 3 sales channels (DTC Shopify, Amazon FBA US/EU, wholesale)
- 4 suppliers (Asia + US) with 14-45 day lead times
- NetSuite ERP

---

## The Problem (Before Automation)

**Ops manager's daily routine:**
- Morning, midday, and end-of-day manual ERP checks (~30 min each): scan ~900 SKUs across 15 locations
- Weekly late-order pivot table in Shopify (2 hours): export orders, filter unfulfilled, age buckets
- Weekly reorder review (3 hours): check velocity, calculate PO quantities, email suppliers

**Total time:** 15-20 hours per week just on monitoring and manual checks

**Pain points documented:**

1. **Reactive stockout detection** — didn't know a SKU was out until a customer complained; ERP reorder points were static (set years ago, never updated). Example: SKU-1002 (Line A, Black) went to zero with no alert — 12 lost sales over 3 days.
2. **Amazon FBA stockouts while the main warehouse had stock** — FBA was a separate location; if it hit zero, the Amazon listing went inactive. Happened 8 times in one quarter.
3. **Late order blindness** — orders sat unfulfilled 20-30 days before anyone noticed; customer service got complaints BEFORE the ops team knew there was a problem.
4. **Inconsistent lead times** — Supplier A: 21 days (usually) · Supplier B: 30 days (sometimes 45) · Supplier C: 14 days. Too early = overstock, too late = stockout.
5. **Manual work burning out the ops manager** — 15+ hrs/week of checks instead of supplier negotiations or fulfillment optimization.

---

## The Solution

### Deployment Timeline (4 Weeks)

**Week 1 — Configuration + API setup:** business intake questionnaire (2 hrs), NetSuite OAuth credentials (1 hr), SuiteQL query testing (2 hrs), Telegram bot for alerts (30 min).

**Week 2 — Stock level monitoring (Component 1):** deployed watchdog cron (every 4 hours); tuned thresholds (<14 days of supply = warning, <7 days = critical); limited tracking to 4 active product lines, excluding 200+ discontinued variants. First real alert: SKU-2015 (Line B, Grey) at 12 units, 3.8 days of supply.

**Week 3 — Late orders + channel balance (Components 3 + 4):** daily 9:30am late-orders script with age buckets (7-13d monitor, 30-60d critical); excluded Rx/custom orders (expected 2-4 week lab lead time); channel balance alerts (DTC vs Amazon FBA allocation).

**Week 4 — Reorder recommendations + PO tracking (Components 2 + 5):** mapped 4 suppliers with lead times + MOQs; open PO tracking (don't alert if restock is already coming); aged PO detection (>45 days past due date).

**Total deployment time:** 18 hours spread over 4 weeks

---

## How It Works (Technical Implementation)

### Component 1: Stock Level Monitoring
**Cron schedule:** Every 4 hours (10am, 2pm, 6pm, 10pm local)
**Data source:** NetSuite SuiteQL API
**Query logic:**

```sql
SELECT
  i.itemid,
  i.displayname,
  il.location,
  l.name as location_name,
  il.quantityavailable,
  il.reorderpoint
FROM item i
JOIN inventoryitemlocations il ON i.id = il.item
JOIN location l ON il.location = l.id
WHERE (i.itemid LIKE 'SKU-1%' OR i.itemid LIKE 'SKU-2%' OR ...)
  AND i.isinactive = 'F'
  AND il.location IN ({tracked_location_ids})
ORDER BY i.itemid, il.location
```

**Alert logic:**
1. Aggregate inventory by SKU across all locations
2. Calculate 30-day sales velocity from historical order data
3. Compute days-of-supply: `current_stock / (30_day_sales / 30)`
4. Alert if days-of-supply < 14 (warning) or < 7 (critical)

**Example alert:**
```
🔴 CRITICAL: Low Stock Alert

SKU-1002 (Line A, Black) - 8 units remaining
• Main warehouse: 3 units
• Amazon US FBA: 5 units
• Sales velocity: 2.1 units/day (30-day avg)
• Days of supply: 3.8 days ⚠️
• Reorder point: 20 units
• Recommended PO: 60 units from Supplier A (14-day lead time)

Last restock: 23 days ago
Next expected shipment: None
Action: Place PO within 2 days to avoid stockout
```

**Ops manager response time:** ~5 minutes (review alert, place PO in ERP)

---

### Component 2: Late Order Detection
**Cron schedule:** Daily 9:30am
**Data source:** Shopify Admin API
**Query logic:**

```javascript
// Get all unfulfilled paid orders
const orders = await shopify.orders.list({
  fulfillment_status: 'unfulfilled',
  financial_status: 'paid',
  status: 'open',
  limit: 250
});

// Age buckets
orders.forEach(order => {
  const age = Math.floor((now - new Date(order.created_at)) / 86400000);
  const isRx = order.tags.toLowerCase().includes('rx-order');

  if (!isRx) {
    if (age >= 30) bucket.critical.push(order);
    else if (age >= 21) bucket.warning.push(order);
    else if (age >= 14) bucket.attention.push(order);
  }
});
```

**Example report:**
```
📦 Unfulfilled Orders — daily report
Total: 18 unfulfilled paid orders

🟡 7–13 Days — MONITOR (5)
  4 Rx orders, 1 standard

🟠 14–20 Days — ATTENTION (7)
  #8501 | Customer A | 18d | $215.00 [Rx]
  #8529 | Customer B | 19d | $89.00

🔴 30–60 Days — CRITICAL (2)
  #7890 | Customer C | 38d | $512.00 ⚠️ REFUND RISK
  #7921 | Customer D | 42d | $234.00 [Rx]
```

**Action taken:** Ops manager escalated #7890 to the COO, warehouse expedited fulfillment, customer notified same day.

---

### Component 3: Channel Balance Monitoring
**Cron schedule:** Daily 11am (part of inventory watchdog)
**Logic:** Track allocation ratio against example allocation targets (e.g., DTC 40%, Amazon US 35%, Amazon EU 10%, Wholesale 15%)
**Alert trigger:** Any channel >20% over/under target

**Example alert:**
```
⚖️ Channel Imbalance Alert

SKU-3030 (Line C, Blue)
• Total available: 42 units
• DTC allocation: 30 units (target 20) ⚠️ +50%
• Amazon US allocation: 8 units (target 20) ⚠️ -60%

Root cause: DTC flash sale sold 18 units in 48 hours
Recommendation: Ship 12 units from main warehouse to Amazon FBA
Action: Update FBA shipment plan within 3 days
```

**Ops manager action:** Created FBA shipment plan, shipped 12 units to the Amazon US warehouse.

---

## Illustrative 6-Month Impact Model

> The numbers below are modeled from early deployment data and represent what a typical 6-month outcome looks like for this configuration. They are illustrative, not audited results.

### Time Savings
| Activity | Before | After | Savings |
|----------|--------|-------|---------|
| Daily stock checks (3x/day) | 10.5 hrs/week | 0 hrs (automated) | **10.5 hrs/week** |
| Weekly late order review | 2 hrs/week | 15 min/week (review report) | **1.75 hrs/week** |
| Weekly reorder planning | 3 hrs/week | 30 min/week (review PO recs) | **2.5 hrs/week** |
| Ad-hoc stockout firefighting | 2-3 hrs/week | 0.5 hrs/week | **2 hrs/week** |
| **Total** | **17-19 hrs/week** | **~2 hrs/week** | **15-17 hrs/week (~87%)** |

Time is reallocated to supplier negotiations, warehouse efficiency, and new product launch planning.

### Stockout Prevention (Modeled)
**Baseline (4 months pre-deployment):** 31 SKU-stockout events, avg duration 8.2 days, ~$47K estimated lost sales (based on 30-day velocity)
**Modeled 6-month outcome:** 3 SKU-stockout events (90% reduction), avg duration 1.8 days, ~$4.2K estimated lost sales
**Prevented stockouts in the model:** 47 (alerts that triggered POs placed <7 days before projected stockout) ≈ **$127K revenue protected**

**Example prevented stockout:**
- SKU-2008 (Line B, Black) — alert at 8 units remaining, 4.2 days of supply
- Action: PO for 80 units with Supplier B, 21-day lead time
- Bridging inventory: reduced Amazon FBA allocation from 25% to 15%, shifted to DTC
- Result: zero stockout, sales velocity maintained

### Late Order Reduction (Modeled)
| Metric | Baseline | Modeled outcome |
|--------|----------|-----------------|
| Avg age of unfulfilled orders | 31 days | 18 days (-42%) |
| Orders >30 days unfulfilled | 12-18 at any time | 3-5 (-67%) |
| Customer complaints/month | 8-12 | 2-3 (-75%) |

**Root cause surfaced by the 60+ day archive bucket:** 80% of aged orders contained one SKU (SKU-3190) marked "in stock" in the ERP but physically missing from its bin location. A physical count found the stock in the wrong location — 11 orders shipped the same week.

### Overstock Reduction (Modeled)
- Slow-moving inventory (>6 months of supply): $150K → $102K (32% reduction, ~$48K working capital freed)
- Velocity tracking identified 23 SKUs with <1 unit/month sales → clearance sale for slow movers
- Warehouse space freed for new product line inventory

### Channel Balance (Modeled)
- Amazon FBA stockouts: 8 per quarter → 1 in 6 months
- Amazon listing uptime: 94% → 99%+
- Double-digit channel revenue growth in the example model; fewer emergency air freight shipments (~$12K modeled savings)
- Mechanism: alerts when FBA drops below 30% allocation → proactive FBA shipment plans (7-day restock vs 14-21 days reactive)

---

## Illustrative ROI Calculation

> Same caveat: this is the *structure* of the ROI case with representative numbers, not an audited P&L.

### Costs
| Item | Amount |
|------|--------|
| Initial configuration (18 hours @ $75/hr) | $1,350 |
| VPS hosting (6 months @ $20/month) | $120 |
| NetSuite + Shopify API calls (within existing plans) | $0 |
| Telegram bot (free) | $0 |
| **Total 6-month cost** | **$1,470** |

### Benefits
| Item | Amount |
|------|--------|
| Ops manager time saved (15 hrs/week × 26 weeks × $50/hr) | $19,500 |
| Stockout prevention (47 events × $2,700 avg lost revenue) | $126,900 |
| Overstock reduction ($48K freed capital × 5% opportunity cost) | $2,400 |
| Emergency freight savings (3 avoided shipments × $4K) | $12,000 |
| CS time saved (6 complaints/month × 6 months × 2 hrs × $30/hr) | $2,160 |
| **Total 6-month benefit** | **$162,960** |

**Net benefit:** ~$161K · **ROI:** ~110x · **Payback period:** days, not months

---

## Key Learnings

### What Worked

1. **Days-of-supply thresholds > static unit counts**
   - SKU-1005 sells 2 units/day → 28 units = 14 days supply
   - SKU-4012 sells 0.3 units/day → 4 units = 13 days supply
   - Same "low stock" urgency, very different unit counts

2. **Exclude custom/Rx orders from "late" categorization**
   - Optical lab work takes 2-4 weeks
   - Without exclusion, 60% of "late" alerts were false positives
   - After exclusion, only actionable late orders surfaced

3. **Channel balance matters for FBA**
   - Can't "shift stock" from the main warehouse to Amazon FBA instantly
   - Need 7-14 day lead time for FBA shipment plan + inbound
   - Proactive alerts prevented reactive firefighting

4. **In-transit PO tracking prevents duplicate orders**
   - Before: see low stock, place PO, then discover a PO was already placed last week
   - After: system checks open POs, only alerts if no restock is coming

5. **Telegram > Email for critical alerts**
   - Push notification seen in <1 minute; email often buried for 2-6 hours
   - For stockout risk, that gap matters

### What Didn't Work (At First)

1. **Initial thresholds too sensitive**
   - Week 1: <21 days of supply = warning → 40-60 alerts/day, 90% non-urgent
   - Fix: <14 days = warning, <7 days = critical → 3-8 alerts/day, 95% actionable

2. **Tried to track all ~900 SKUs**
   - Week 1: monitored everything, including discontinued variants
   - Result: alerts for SKUs that hadn't sold in 6 months
   - Fix: filter by the 4 active product lines, exclude discontinued → 70% less noise

3. **Forgot to exclude accessory/variant suffixes**
   - SKU-1002 (standard) vs SKU-1002-BT (Bluetooth variant) showed identically in some reports
   - Result: false low-stock alerts (looking at the wrong SKU)
   - Fix: added exclusion pattern `%-BT%` to config

4. **Late order age buckets too tight**
   - Initially: 3-7 days = attention, 7+ days = critical
   - Problem: 50% of Rx orders take 14-21 days (normal)
   - Fix: 7-13 days = monitor, 30+ days = critical, plus the Rx tag exclusion

---

## Operator Perspective

> Paraphrased from the deployment team, unattributed: Before the watchdog, monitoring meant logging into the ERP three times a day and scanning ~900 SKUs from memory — mind-numbing and error-prone. After deployment, the team gets a handful of actionable alerts per day ("SKU at 12 units, 5.8 days of supply, recommended PO with Supplier A") that take about two minutes each to act on. The morning late-order report surfaces anything in the 30-60 day bucket for immediate escalation, where previously orders sat 40+ days until a customer complained. The recovered 15 hours a week went to supplier negotiations, warehouse layout, and launch planning — and the bigger payoff was confidence: no alert means nothing is on fire.

---

## Planned Enhancements

**Phase 2**
1. **Auto-draft purchase orders in the ERP** — alert → pre-built PO draft, ops manager just approves (10 min/PO → 2 min/PO)
2. **Demand forecasting (30-60 day lookahead)** — predict stockouts from seasonal trends; plan peak-season inventory months ahead
3. **Supplier performance tracking** — on-time delivery rate by supplier; flag >20% late rates for renegotiation

**Phase 3**
1. **Automated rebalancing (DTC → Amazon FBA)** — auto-create FBA shipment plans when allocation drops <30%
2. **Multi-warehouse transfer recommendations** — extend beyond main warehouse + FBA to regional 3PLs, auto-recommend transfers when regional demand shifts

---

## Replicability: Can This Work for Your Business?

**You're a good fit if:**
- ✅ You have 50+ SKUs (manual tracking doesn't scale)
- ✅ You sell across 2+ channels (DTC, Amazon, wholesale, retail)
- ✅ Your ops manager spends >5 hrs/week on manual inventory checks
- ✅ You've had 3+ stockouts in the last 90 days
- ✅ You have an inventory system with API or CSV export (NetSuite, Shopify, WooCommerce, etc.)

**You might not need this if:**
- ❌ You have <20 SKUs (manual tracking is fine)
- ❌ You're pure B2B with lumpy order patterns (demand too unpredictable for velocity-based alerts)
- ❌ You have a dedicated inventory manager with custom ERP dashboards (already solved)

**System requirements:**
- Inventory system: NetSuite, Shopify, WooCommerce, Amazon, or Google Sheets
- Alert delivery: Telegram, Slack, Discord, or Email
- Hosting: VPS ($5-20/month) or serverless functions
- Dev time: 10-20 hours initial setup, 2-4 hours/month maintenance

---

## Conclusion

The Inventory Watchdog turned this deployment's inventory operations from reactive firefighting into proactive monitoring. In the illustrative 6-month model: ~87% reduction in ops time, ~90% reduction in stockouts, ~$127K in protected sales, ~$48K in freed working capital.

**The pattern is reusable.** Same system, different configurations. Whether you sell eyewear, apparel, or industrial supplies, the core workflow is universal:

1. Monitor stock levels (days-of-supply, not just unit counts)
2. Alert proactively (before stockout, not after)
3. Track fulfillment lag (late orders by age bucket)
4. Balance channels (DTC vs Amazon vs wholesale)
5. Automate reorder recommendations (supplier + quantity + lead time)

**Next step:** Fill out `config/intake-v1.md` and deploy your own Inventory Watchdog.

---

**Version:** 1.1
**Published:** April 2026
**Part of:** JBOT Protocol — Inventory Watchdog System
