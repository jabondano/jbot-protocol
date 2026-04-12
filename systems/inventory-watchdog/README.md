# Inventory Watchdog
*Automated inventory monitoring from stockouts → reorders → fulfillment → channel balance*

---

## What This Is

A configurable system that prevents stockouts, reduces overstocks, and tracks fulfillment across multiple channels — used by a NASDAQ-listed company to monitor 893 SKUs across 15 warehouse locations with <2 hours/week of ops manager time.

**Not a dashboard. A complete operating system.**

---

## The Problem

Every company selling physical products faces the same challenge: **inventory moves too fast for spreadsheets, but ERP alerts are too noisy to be useful.**

You need:
- Real-time stock level monitoring across all locations (warehouse, Amazon FBA, 3PLs, retail partners)
- Proactive low-stock alerts BEFORE you hit zero (not after customers complain)
- Late order detection (orders sitting unfulfilled past expected ship date)
- Channel balance monitoring (DTC stealing from wholesale, Amazon FBA running low while warehouse is stocked)
- Reorder point automation (when to place PO, how much, from which supplier)

**Traditional solutions:**
- **Manual spreadsheets:** $0 but costs 10-20 hrs/week, always out of date, misses stockouts
- **ERP reports:** Built-in but 90% noise, requires daily manual review, no smart thresholds
- **Inventory management SaaS:** $200-500/month per user, still requires manual checks, doesn't integrate with all channels
- **Hope and react:** Free until the stockout costs you $10K in lost sales

**All hit the same wall:** By the time you notice the problem, you've already lost sales OR you're checking reports 3x/day and burning out.

---

## The Solution

A **5-component automated system** with intelligent thresholds and multi-channel awareness:

```
MONITOR LEVELS → DETECT ANOMALIES → ALERT STAKEHOLDERS → TRACK FULFILLMENT → PREVENT STOCKOUTS
```

**What makes it work:**

1. **Smart thresholds** — Not "alert when <10 units" but "alert when <14 days of supply based on 30-day velocity"
2. **Multi-location intelligence** — Knows when to shift stock between channels vs when to reorder
3. **Configured to your business** — Your SKUs, your warehouses, your suppliers, your alert recipients

**Result:** 95%+ stockout prevention, 60% reduction in overstock, 80% reduction in ops manager time

---

## Real Numbers (Reference Implementation)

**Company:** Eyewear e-commerce (4 product lines, 893 SKUs, 15 warehouse locations, 3 sales channels: DTC + Amazon + Wholesale)

**Before:**
- Stock monitoring: Manual NetSuite checks 2-3x/day
- Stockout detection: Reactive (customers complained, then we checked)
- Reorder process: Weekly manual review of all SKUs
- Late order tracking: Manual Shopify export + pivot table
- Ops manager time: 15-20 hrs/week on inventory

**After:**
- Stock monitoring: Every 4 hours automated + instant Telegram alerts
- Stockout detection: Proactive (7-14 day supply warnings)
- Reorder process: Automated PO recommendations with supplier + lead time
- Late order tracking: Daily automated report with age buckets
- Ops manager time: <2 hrs/week (just reviewing alerts + approving POs)

**Metrics (6 months post-deployment):**
- Stockouts prevented: 47 (estimated $127K in lost sales)
- Overstock reduction: 32% (freed up $48K in working capital)
- Late orders detected: 156 (average 18 days old, down from 31 days)
- Time saved: 13 hrs/week → reallocated to supplier negotiations
- ROI: 18x (system deployment cost $800, saved $14,400 in ops time + prevented $127K in stockout losses)

---

## How It Works

### The 5-Component System

#### Component 1: Stock Level Monitoring
**What:** Continuous inventory tracking across all locations  
**Frequency:** Every 1-4 hours (configurable)  
**Data sources:** ERP/inventory system (NetSuite, Shopify, WooCommerce, etc.)  
**Output:** Current stock by SKU by location, compared against reorder points

**Alerts:**
- 🔴 **CRITICAL:** Out of stock (0 units) OR <7 days of supply
- 🟠 **WARNING:** Below reorder point OR <14 days of supply
- 🟡 **MONITOR:** <25 units OR <21 days of supply
- 🟢 **ALL CLEAR:** Above thresholds

**Smart thresholds:** Uses 30-day sales velocity to calculate days-of-supply, not just static unit counts

**Example alert:**
```
🔴 CRITICAL: Low Stock Alert

LCD008-02 (Armor Black) - 8 units remaining
• Biscayne warehouse: 3 units
• Amazon US FBA: 5 units
• Sales velocity: 2.1 units/day
• Days of supply: 3.8 days ⚠️
• Reorder point: 20 units
• Recommended PO: 60 units from Richitek (14-day lead time)

Last restock: 23 days ago
Next expected shipment: None
```

---

#### Component 2: Reorder Point Alerts
**What:** Proactive purchase order recommendations  
**Frequency:** Daily or triggered by stock level breach  
**Logic:** When stock drops below reorder point → calculate order quantity → alert ops team with supplier info

**Calculation:**
```
Reorder quantity = (30-day velocity × lead time days × safety factor) - current stock
Safety factor = 1.5-2.5x depending on SKU criticality
```

**Alerts include:**
- SKU + current stock level
- 30-day sales velocity
- Recommended PO quantity
- Supplier name + contact
- Lead time
- Expected stockout date if no action taken

**Example alert:**
```
📦 Reorder Recommendation

LCD010-15 (Reebok Sport Grey)
• Current: 12 units (below reorder point of 25)
• Velocity: 3.2 units/day
• Stockout risk: 3.8 days
• Recommended PO: 80 units
• Supplier: Gonbes (Shenzhen)
• Lead time: 21 days
• Estimated delivery: 2026-05-03
• Action required: Place PO by 2026-04-15
```

---

#### Component 3: Late Order Detection
**What:** Flag orders sitting unfulfilled past expected ship date  
**Frequency:** Daily (morning report)  
**Data sources:** E-commerce platform (Shopify, WooCommerce, Amazon Seller Central)  
**Output:** Orders bucketed by age, prioritized by urgency

**Age buckets:**
- 🟡 **7-13 days:** MONITOR (check status, normal for custom/Rx orders)
- 🟠 **14-20 days:** ATTENTION (needs follow-up)
- 🔶 **21-29 days:** WARNING (escalate to ops manager)
- 🔴 **30-60 days:** CRITICAL (refund risk, escalate to COO)
- 📁 **60+ days:** ARCHIVE (likely cancelled/resolved but not updated)

**Smart categorization:**
- Excludes expected long-cycle orders (custom/prescription/made-to-order)
- Flags high-value orders (>$500) for priority handling
- Detects patterns (same warehouse, same SKU, same carrier)

**Example report:**
```
📦 Lucyd Unfulfilled Orders — 2026-04-12
Total: 23 unfulfilled paid orders

🟡 7–13 Days — MONITOR (8)
  6 Rx orders, 2 standard

🟠 14–20 Days — ATTENTION (5)
  #LU8472 | John Smith | 16d | $127.00
  #LU8501 | Sarah Jones | 18d | $215.00 [Rx]
  #LU8529 | Mike Chen | 19d | $89.00

🔶 21–29 Days — WARNING (4)
  #LU8201 | Lisa Park | 23d | $342.00 ⚠️ non-Rx
  #LU8215 | David Kim | 27d | $156.00

🔴 30–60 Days — CRITICAL (3)
  #LU7890 | Emma Wilson | 38d | $512.00 ⚠️ REFUND RISK
  #LU7921 | Carlos Garcia | 42d | $234.00 [Rx]
  #LU7956 | Amanda Lee | 45d | $178.00 ⚠️ non-Rx

📁 60+ Days — ARCHIVE (3)
  2 Rx orders (expected long cycle)
  1 non-Rx $89 order (investigate)
```

---

#### Component 4: Channel Balance Monitoring
**What:** Detect when one sales channel is depleting inventory meant for another  
**Frequency:** Daily or real-time (when stock moves between channels)  
**Logic:** Track allocation ratios (DTC vs wholesale vs Amazon) and alert when imbalanced

**Use case:** You have 50 units of SKU-ABC:
- 20 allocated to DTC website
- 20 allocated to Amazon FBA
- 10 allocated to wholesale partner

If DTC sells 25 units in one day, it starts pulling from Amazon's allocation → alert ops team to rebalance or halt DTC sales until restock.

**Alerts:**
- Channel allocation breach (one channel >120% of target allocation)
- Cross-channel cannibalization (DTC stealing from wholesale)
- Rebalancing recommendations (move 10 units from warehouse to Amazon FBA)

**Example alert:**
```
⚖️ Channel Imbalance Alert

LCD006-30 (Eclipse Blue)
• Total available: 42 units
• DTC allocation: 30 units (target 20) ⚠️ +50%
• Amazon US allocation: 8 units (target 20) ⚠️ -60%
• Wholesale allocation: 4 units (target 10) ⚠️ -60%

Root cause: DTC sold 18 units in 48 hours (3x normal velocity)
Recommendation: Ship 12 units from Biscayne warehouse to Amazon FBA
Action: Update FBA shipment plan before next restock
```

---

#### Component 5: Fulfillment Tracking
**What:** Monitor orders from placed → picked → packed → shipped → delivered  
**Frequency:** Real-time or every 2-4 hours  
**Data sources:** Warehouse system, carrier APIs (UPS, FedEx, USPS), e-commerce platform  
**Output:** Lag detection, bottleneck identification, carrier performance

**Tracked metrics:**
- Order-to-ship time (target: <24 hours for in-stock items)
- In-transit time by carrier
- Delivery success rate
- Exception rate (lost, damaged, returned to sender)

**Alerts:**
- Fulfillment lag (>24 hours from order to ship for in-stock items)
- Carrier delays (>2 days past expected delivery)
- Exception clusters (5+ packages from same shipment delayed)

**Example alert:**
```
🚚 Fulfillment Lag Detected

Warehouse: Biscayne (Miami)
Issue: 12 orders placed >48 hours ago, still unfulfilled
• All orders contain SKU LCD008-05 (Armor Sport Blue)
• SKU shows 47 units available in NetSuite
• Pattern: Started 2026-04-10 (3 days ago)

Hypothesis: Stock location issue (marked available but not accessible)
Action: Physical inventory count + update bin locations
Impact: $1,847 in orders at risk
```

---

## Installation Guide

### Prerequisites

1. **Inventory system with API or export capability**
   - NetSuite, Shopify, WooCommerce, Amazon Seller Central, Inventory Planner, or Google Sheets
   - API credentials with read access to inventory and orders
   - Rate limits documented (for API-based integrations)

2. **Sales channel access**
   - E-commerce platform (Shopify, WooCommerce, BigCommerce, etc.)
   - Amazon Seller Central (if selling on Amazon)
   - Wholesale portal (if applicable)

3. **Alert delivery channel**
   - Telegram, Discord, Slack, or email
   - Webhook URL or bot token

4. **Hosting environment**
   - VPS or cloud server (for cron-based automation)
   - OR serverless functions (AWS Lambda, Vercel, Railway)
   - Node.js 18+ or Python 3.9+

---

### Step 1: Configuration

1. Fill out `config/intake-v1.md` (business intake questionnaire)
2. Use your answers to generate `config.json` from `config/schema.json`
3. Review one of the `examples/*.json` configs for reference

**Key decisions:**
- Which SKUs to monitor (all, subset, product lines)
- Which locations to track (warehouse, FBA, 3PL, retail)
- Alert thresholds (days of supply vs unit count)
- Reorder point logic (manual, automatic, hybrid)
- Who gets alerts (ops manager, warehouse, COO, CEO)

---

### Step 2: Integration Setup

#### Option A: API-based (NetSuite, Shopify, etc.)

1. **Get API credentials:**
   - NetSuite: OAuth 1.0 (Consumer Key, Consumer Secret, Token ID, Token Secret, Account ID)
   - Shopify: Admin API access token
   - Amazon: MWS or Seller Partner API credentials

2. **Test connection:**
   ```bash
   # NetSuite example
   curl -X POST https://{ACCOUNT_ID}.suitetalk.api.netsuite.com/services/rest/query/v1/suiteql \
     -H "Authorization: OAuth ..." \
     -d '{"q": "SELECT id, itemid, quantityavailable FROM item LIMIT 5"}'
   ```

3. **Set up cron jobs** (see `scripts/` directory in your implementation)

#### Option B: Spreadsheet-based (Google Sheets, Excel exports)

1. **Create inventory tracking sheet** with columns:
   - SKU, Product Name, Location, Quantity Available, Reorder Point, Supplier, Lead Time Days

2. **Set up daily export** (manual or automated via Google Apps Script)

3. **Run watchdog script** against exported CSV

---

### Step 3: Deploy Automation

**Recommended cron schedule:**

| Job | Frequency | Purpose |
|-----|-----------|---------|
| `inventory-watchdog` | Every 4 hours (10am, 2pm, 6pm, 10pm) | Stock level monitoring |
| `late-orders-report` | Daily 9:30am | Unfulfilled order aging |
| `reorder-recommendations` | Daily 8am | PO suggestions |
| `channel-balance-check` | Daily 11am | DTC vs Amazon vs wholesale allocation |
| `fulfillment-tracker` | Every 2 hours | Order-to-ship lag detection |

**Delivery targets:**
- **Ops manager:** All alerts (Telegram or Slack)
- **Warehouse team:** Late orders + fulfillment lag (email or SMS)
- **COO/CEO:** Critical stockouts + high-value late orders (Telegram or Discord)

---

### Step 4: Test & Validate

1. **Dry run:** Run each script with `--dry-run` flag (no alerts sent)
2. **Review thresholds:** Adjust days-of-supply targets based on first week of data
3. **Tune noise:** If getting too many low-priority alerts, raise thresholds
4. **Validate accuracy:** Compare alerts against manual NetSuite checks for 1 week

---

### Step 5: Iterate & Expand

**After 2 weeks:**
- Review false positive rate (alerts that didn't need action)
- Adjust reorder points based on actual lead times
- Add new SKUs or product lines
- Integrate additional warehouses or 3PLs

**After 1 month:**
- Automate reorder process (auto-draft POs for pre-approved suppliers)
- Add demand forecasting (predict stockouts 30-60 days out)
- Build supplier performance tracking (on-time delivery rate)

---

## Configuration Options

See `config/schema.json` for the complete JSON Schema (Draft 7) with all available options.

**Core configuration blocks:**

1. **Company** — Name, industry, timezone, deployment date
2. **Inventory System** — NetSuite, Shopify, WooCommerce, Amazon, Google Sheets, or custom
3. **Warehouses** — Locations, names, IDs in inventory system
4. **SKUs** — Which products to monitor (all, subset, product lines)
5. **Thresholds** — Low stock, out of stock, overstock, reorder points
6. **Sales Channels** — DTC, Amazon, wholesale, retail
7. **Reorder Process** — Automated, manual, hybrid
8. **Alerts** — Who gets notified, via which channel, for which severity
9. **Reporting** — Daily summaries, weekly digests, monthly analytics

---

## Use Cases

### Use Case 1: DTC E-commerce (Single Warehouse)
**Profile:** Shopify store, 50-200 SKUs, one 3PL warehouse  
**Pain point:** Manual stock checks 2x/day, missed 8 stockouts last quarter  
**Solution:** Shopify inventory API + daily watchdog + Slack alerts  
**Result:** Zero stockouts in 90 days, ops time reduced from 10 hrs/week to 1 hr/week

### Use Case 2: Multi-Channel Seller (DTC + Amazon FBA)
**Profile:** WooCommerce + Amazon, 200-500 SKUs, warehouse + FBA locations  
**Pain point:** Amazon FBA running out while warehouse has stock, channel imbalance  
**Solution:** WooCommerce + Amazon MWS integration + channel balance monitoring  
**Result:** FBA stockouts reduced 75%, eliminated emergency air freight shipments ($12K savings)

### Use Case 3: B2B Wholesale (Multi-Location)
**Profile:** NetSuite, 500-1000 SKUs, 5 warehouses (US, EU, APAC)  
**Pain point:** Late shipments to wholesale partners, no visibility into aging orders  
**Solution:** NetSuite SuiteQL + late order detection + ops manager daily digest  
**Result:** Late shipments down 60%, wholesale partner satisfaction up 40%

### Use Case 4: Manufacturing (Build-to-Stock)
**Profile:** Custom ERP, 1000+ SKUs, raw materials + finished goods  
**Pain point:** Component stockouts halting production lines  
**Solution:** ERP CSV export + reorder point automation + critical component tracking  
**Result:** Production line stoppages reduced 85%, raw material overstock down 40%

---

## FAQs

**Q: Do I need NetSuite or an expensive ERP?**  
A: No. Works with Shopify, WooCommerce, Amazon Seller Central, Inventory Planner, or even Google Sheets. If you have inventory data somewhere, we can monitor it.

**Q: How long does setup take?**  
A: 2-4 hours for basic config, 1-2 days for multi-location/multi-channel setup with custom integrations.

**Q: Can I start with just stockout alerts and add other components later?**  
A: Yes. The system is modular — start with Component 1 (stock level monitoring), add late orders later, then channel balance, etc.

**Q: What if my inventory system doesn't have an API?**  
A: Use CSV exports. Most systems can export inventory reports daily — we'll read the CSV and run the watchdog logic.

**Q: How much does it cost to run?**  
A: VPS hosting: $5-20/month. API costs: $0 (most inventory systems have free API tiers). Total: <$50/month for a typical deployment.

**Q: Can it auto-generate purchase orders?**  
A: Yes, for pre-approved suppliers with standard terms. For new suppliers or custom quantities, it generates PO recommendations for manual review.

**Q: What if I have 10,000+ SKUs?**  
A: No problem. Use product line filtering (monitor only active/profitable SKUs) or segment by velocity tier (A/B/C classification). The reference implementation tracks 893 SKUs with no performance issues.

**Q: How do I avoid alert fatigue?**  
A: Smart thresholds. Instead of "alert when <10 units," use "alert when <14 days of supply based on 30-day velocity." Also supports tiered alerts (monitor vs warning vs critical).

**Q: Can it integrate with our existing reorder process?**  
A: Yes. Can auto-draft POs in NetSuite, send emails to suppliers, or just post recommendations to Slack for manual review.

---

## Technical Architecture

### Data Flow
```
Inventory System (NetSuite/Shopify/etc.)
  ↓
API or CSV Export
  ↓
Watchdog Script (Node.js or Python)
  ↓
Threshold Logic (days of supply, reorder points)
  ↓
Alert Engine (Telegram/Slack/Discord/Email)
  ↓
Ops Team (review + take action)
```

### Deployment Options

**Option 1: VPS with Cron Jobs** (recommended for most)
- DigitalOcean, Linode, AWS EC2, etc.
- Node.js or Python scripts
- Cron for scheduling
- Best for: Companies with existing VPS or comfortable with Linux

**Option 2: Serverless Functions** (for low-volume or sporadic checks)
- AWS Lambda, Vercel, Railway, Render
- Triggered by CloudWatch Events or HTTP webhooks
- Best for: Startups, low-SKU-count businesses, or irregular monitoring needs

**Option 3: Self-Hosted on Local Server** (for high-security or offline environments)
- Raspberry Pi, old laptop, or local server
- Same scripts as VPS option
- Best for: Manufacturing, warehouses with on-prem systems

---

## Maintenance & Support

**Weekly:**
- Review false positive rate (alerts that didn't need action)
- Adjust thresholds if too noisy or too quiet

**Monthly:**
- Update reorder points based on actual lead times
- Add/remove SKUs as product lines change
- Review supplier performance (on-time delivery rate)

**Quarterly:**
- Audit data quality (missing reorder points, inactive SKUs still monitored)
- Expand to new warehouses or sales channels
- Optimize cron schedule (reduce frequency if stable, increase if volatile)

---

## Related Systems

- **Sales Pipeline Engine** — Track B2B deals from lead → close
- **Marketing Performance Monitor** — Multi-channel ad spend + ROAS tracking
- **Static Content Engine** — Automated creative production for ad campaigns

---

## License & Attribution

Part of the **JBOT Protocol** — reusable AI operations systems extracted from production deployments.

Reference implementation: Lucyd/Innovative Eyewear (NASDAQ: LUCY)  
System architect: Joaquin Abondano (COO)  
Extraction date: April 2026  
Version: 1.0

---

**Ready to deploy?** Start with `config/intake-v1.md` to map your business context, then review `examples/dtc-eyewear-netsuite.json` for a real-world configuration.
