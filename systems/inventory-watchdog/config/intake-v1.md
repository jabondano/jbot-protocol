# Inventory Watchdog — Business Intake Sheet
*Version 1.0 | Draft | JBOT Protocol*

---

## Purpose

This intake captures the business context needed to configure the Inventory Watchdog system for your company. Your answers will determine:

- Inventory system integration approach
- Stock level monitoring thresholds
- Late order detection logic
- Reorder point automation
- Alert recipients and delivery channels
- Reporting structure

**Time to complete:** 15-25 minutes  
**Required for deployment:** Sections 1-4  
**Optional for optimization:** Sections 5-7

---

## Section 1: Inventory System & Warehouses

### 1.1 What inventory system do you currently use?
**What this configures:** Integration method, API vs CSV, data sync frequency

**Options:**
- [ ] **NetSuite** (Full ERP with SuiteQL API)
- [ ] **Shopify** (E-commerce with inventory API)
- [ ] **WooCommerce** (WordPress e-commerce with inventory plugins)
- [ ] **BigCommerce** (E-commerce platform with API)
- [ ] **Amazon Seller Central** (FBA + Seller Fulfilled inventory)
- [ ] **Inventory Planner** (Standalone inventory management)
- [ ] **Odoo** (Open-source ERP)
- [ ] **Cin7** (Inventory management SaaS)
- [ ] **DEAR Inventory** (Cloud inventory system)
- [ ] **Google Sheets** (Spreadsheet-based tracking)
- [ ] **Excel/CSV exports** (Manual exports from another system)
- [ ] **Custom/proprietary system** (in-house built)
- [ ] **Nothing** (No formal inventory tracking)
- [ ] **Other:** ________________ (specify)

**Example (Lucyd):** NetSuite (Full ERP, SuiteQL API integrated)

---

### 1.2 If using an inventory system, what's your access level?
**What this configures:** API capabilities, automation scope, integration depth

**Options:**
- [ ] **Admin** (full API access, can create custom fields/reports)
- [ ] **Power User** (API access, limited customization)
- [ ] **Standard User** (read-only API or CSV exports)
- [ ] **View Only** (no API, manual reports only)
- [ ] **N/A** (no inventory system)

**If Admin/Power User:**
- API credentials available? [ ] Yes [ ] No [ ] Need to request
- API rate limits known? [ ] Yes [ ] No (we'll look it up)
- Real-time data access? [ ] Yes [ ] Batch/CSV only

**Example (Lucyd):** Admin, OAuth 1.0 API access, real-time SuiteQL queries

---

### 1.3 How many SKUs do you manage?
**What this configures:** Monitoring scope, filtering strategy, performance optimization

**Options:**
- [ ] **1-50 SKUs** (monitor all)
- [ ] **50-200 SKUs** (monitor all or by product line)
- [ ] **200-1,000 SKUs** (likely filter by product line or velocity tier)
- [ ] **1,000-5,000 SKUs** (requires velocity-based filtering)
- [ ] **5,000+ SKUs** (A/B/C classification required)

**Example (Lucyd):** 893 active SKUs, filtered by product line (4 lines: Lyte, Armor, Reebok, AERO)

---

### 1.4 How many warehouses or storage locations do you have?
**What this configures:** Multi-location tracking, channel allocation, rebalancing logic

**Options:**
- [ ] **1 location** (simple single-warehouse monitoring)
- [ ] **2-3 locations** (e.g., warehouse + Amazon FBA, or US + EU warehouse)
- [ ] **4-10 locations** (multi-region fulfillment)
- [ ] **10+ locations** (enterprise multi-warehouse + 3PLs)

**If 2+ locations, list them:**

| Location Name | Type (warehouse/3PL/FBA/retail) | Country | Track separately? |
|---------------|----------------------------------|---------|-------------------|
| Example: Miami Warehouse | Warehouse | USA | Yes |
| Example: Amazon FBA US | Amazon FBA | USA | Yes |
| Example: Netherlands 3PL | 3PL | Netherlands | Yes |

**Example (Lucyd):** 15 locations (primary warehouse in Miami, 2 Amazon FBA locations, 12 international 3PL/sales supply locations)

---

## Section 2: Sales Channels & Stock Allocation

### 2.1 What sales channels do you sell through?
**What this configures:** Channel balance monitoring, allocation logic, stock splitting

**Check all that apply:**
- [ ] **DTC website** (Shopify, WooCommerce, BigCommerce, etc.)
- [ ] **Amazon FBA** (Fulfilled by Amazon)
- [ ] **Amazon Seller Fulfilled** (ship from your warehouse)
- [ ] **Wholesale** (B2B customers, bulk orders)
- [ ] **Retail stores** (physical retail partners)
- [ ] **Marketplaces** (eBay, Walmart, Etsy, etc.)
- [ ] **POS/In-store** (your own brick-and-mortar)
- [ ] **Other:** ________________

**Example (Lucyd):** DTC website (Shopify), Amazon FBA (US + DE), Wholesale (B2B portal)

---

### 2.2 Do you allocate stock differently across channels?
**What this configures:** Channel balance alerts, rebalancing triggers, allocation enforcement

**Options:**
- [ ] **Yes, strict allocation** (30% DTC, 50% Amazon, 20% wholesale — enforce)
- [ ] **Yes, flexible targets** (rough allocation, allow channel to borrow from others)
- [ ] **No, first-come-first-served** (any channel can sell any stock)
- [ ] **N/A** (single channel)

**If Yes, what's the split?**

| Channel | Target % | Allow borrowing? | Rebalance frequency |
|---------|----------|------------------|---------------------|
| DTC | 40% | Yes | Weekly |
| Amazon FBA | 40% | No | Weekly |
| Wholesale | 20% | Yes | Monthly |

**Example (Lucyd):** Flexible targets — DTC and wholesale can borrow from each other, Amazon FBA is ring-fenced (requires manual rebalancing)

---

## Section 3: Current Pain Points

### 3.1 What inventory problems do you face most often?
**What this configures:** Alert priorities, threshold tuning, focus areas

**Check all that apply:**
- [ ] **Stockouts** (running out of inventory, lost sales)
- [ ] **Late detection of stockouts** (didn't know until customers complained)
- [ ] **Overstocks** (too much inventory, cash tied up)
- [ ] **Channel imbalances** (Amazon runs out while warehouse has stock)
- [ ] **Late shipments** (orders sitting unfulfilled for days/weeks)
- [ ] **Reorder timing** (don't know when to place POs)
- [ ] **Supplier delays** (orders arrive late, causes stockouts)
- [ ] **Data quality** (inventory counts are wrong, can't trust the numbers)
- [ ] **Manual work** (spending 10+ hrs/week checking stock levels)
- [ ] **No visibility** (don't know what's low until it's zero)

**Top 3 priorities:**
1. ___________________________
2. ___________________________
3. ___________________________

**Example (Lucyd top 3):**
1. Late detection of stockouts (reactive, not proactive)
2. Manual inventory checks 2-3x/day (15-20 hrs/week ops manager time)
3. Amazon FBA stockouts while warehouse has stock (channel imbalance)

---

### 3.2 How many stockouts did you have in the last 90 days?
**What this configures:** Threshold sensitivity, alert urgency, ROI calculation

**Options:**
- [ ] **0 stockouts** (monitoring to maintain this)
- [ ] **1-3 stockouts** (rare but costly)
- [ ] **4-10 stockouts** (semi-regular, need better alerts)
- [ ] **10-30 stockouts** (frequent, major problem)
- [ ] **30+ stockouts** (constant issue, broken process)
- [ ] **Don't know** (no tracking)

**Estimated revenue lost per stockout:** $________________

**Example (Lucyd before deployment):** 8 stockouts in 90 days, avg $2,700 lost revenue per stockout = $21,600 lost

---

### 3.3 How often do you manually check inventory levels?
**What this configures:** Automation value, time savings calculation, cron frequency

**Options:**
- [ ] **Multiple times per day** (3+ manual checks)
- [ ] **Daily** (once per day)
- [ ] **2-3 times per week**
- [ ] **Weekly**
- [ ] **Never** (reactive only)

**Time spent per check:** ________ minutes  
**Who does the checking:** ___________________________

**Example (Lucyd before):** 2-3 times per day, 20-30 minutes per check, ops manager = 15-20 hrs/week

---

## Section 4: Stock Monitoring & Thresholds

### 4.1 How do you want to define "low stock"?
**What this configures:** Alert threshold logic, days-of-supply calculation, velocity tracking

**Options:**
- [ ] **Days of supply** (e.g., alert when <14 days of stock based on sales velocity)
- [ ] **Unit count** (e.g., alert when <25 units)
- [ ] **Both** (unit count AND days of supply)
- [ ] **Reorder point from inventory system** (use existing reorder points)
- [ ] **Don't know** (help me decide)

**If days of supply:**
- Warning threshold: <______ days
- Critical threshold: <______ days
- Velocity lookback period: ______ days (typically 30)

**If unit count:**
- Warning threshold: <______ units
- Critical threshold: <______ units

**Example (Lucyd):** Days of supply — Warning <14 days, Critical <7 days, using 30-day velocity

---

### 4.2 Should we track overstocks?
**What this configures:** Overstock alerts, capital efficiency monitoring, warehouse space optimization

**Options:**
- [ ] **Yes, alert on overstocks** (>X months of supply)
- [ ] **No, only care about low stock**
- [ ] **Maybe later** (focus on stockouts first)

**If Yes:**
- Overstock threshold: >______ months of supply

**Example (Lucyd):** No overstock alerts (warehouse space not a constraint, prefer safety stock)

---

### 4.3 Do you want to include in-transit inventory in stock counts?
**What this configures:** Stock availability logic, reorder timing, stockout risk calculation

**Options:**
- [ ] **Yes, count in-transit** (reduces false alerts when PO is already placed)
- [ ] **No, only count available stock** (more conservative, alerts sooner)
- [ ] **Depends on lead time** (count in-transit if arrival <7 days)

**Example (Lucyd):** Yes, track open POs and expected arrival dates — don't alert if PO arriving <7 days

---

## Section 5: Late Orders & Fulfillment

### 5.1 Do you want to monitor late/unfulfilled orders?
**What this configures:** Late order detection, aging buckets, fulfillment lag alerts

**Options:**
- [ ] **Yes** (critical for customer satisfaction)
- [ ] **No** (not a priority)
- [ ] **Maybe later**

**If Yes:**

**What defines "late"?**
- Orders unfulfilled >______ days (standard products)
- Orders unfulfilled >______ days (custom/made-to-order)

**Severity buckets:**

| Age Range | Severity | Action |
|-----------|----------|--------|
| 7-13 days | Monitor | Check status |
| 14-20 days | Attention | Follow up |
| 21-29 days | Warning | Escalate |
| 30-60 days | Critical | Refund risk |
| 60+ days | Archive | Likely resolved |

**Example (Lucyd):** Yes — Standard products >7 days = monitor, >14 days = attention, >30 days = critical. Rx/custom orders excluded (expected long cycle).

---

### 5.2 What types of orders should be excluded from "late" tracking?
**What this configures:** Exclusion rules, false positive reduction, custom order handling

**Check all that apply:**
- [ ] **Custom/personalized orders** (expected long lead time)
- [ ] **Prescription/Rx orders** (optical work, expected 2-4 weeks)
- [ ] **Made-to-order products** (manufactured on-demand)
- [ ] **Pre-orders** (not yet in stock)
- [ ] **Backorders** (known out of stock)
- [ ] **Orders tagged as "hold"** (customer requested delay)
- [ ] **None** (track all orders equally)

**Example (Lucyd):** Exclude Rx orders (tagged "lensadvizor", "lab", "custom lenses"), made-to-order

---

## Section 6: Reorder Process & Suppliers

### 6.1 How do you currently handle reordering?
**What this configures:** Automation scope, PO generation, approval workflow

**Options:**
- [ ] **Manual** (ops manager reviews stock weekly, places POs manually)
- [ ] **Semi-automated** (system alerts when low, ops manager places PO)
- [ ] **Automated** (system auto-generates PO draft, ops manager approves)
- [ ] **Fully automated** (for pre-approved suppliers, auto-send POs)
- [ ] **Ad-hoc** (no formal process, order when we remember)

**Example (Lucyd):** Semi-automated — Watchdog alerts, ops manager reviews and places PO in NetSuite

---

### 6.2 How many suppliers do you work with?
**What this configures:** Supplier database, lead time tracking, auto-routing logic

**Options:**
- [ ] **1 supplier** (single-source)
- [ ] **2-5 suppliers** (small vendor base)
- [ ] **5-15 suppliers** (moderate complexity)
- [ ] **15+ suppliers** (many vendors, need supplier routing)

**For each key supplier, provide:**

| Supplier Name | Lead Time (days) | Minimum Order Qty | SKUs Supplied | Auto-approve POs? |
|---------------|------------------|-------------------|---------------|-------------------|
| Example: Gonbes | 21 | 50 units | LCD008, LCD010 | Yes |
| Example: Richitek | 30 | 100 units | LCD012 | No (requires approval) |

**Example (Lucyd):** 4 primary suppliers (Gonbes, Richitek, Trans-Pacific, ITFIT Optical), 21-45 day lead times

---

### 6.3 Do you want the system to auto-generate PO recommendations?
**What this configures:** PO draft generation, supplier selection, quantity calculation

**Options:**
- [ ] **Yes, generate PO drafts** (include SKU, quantity, supplier, lead time)
- [ ] **Yes, but approval required** (human reviews before sending)
- [ ] **No, just alert me** (I'll handle the PO manually)

**If Yes, what info should be included?**
- [ ] SKU + quantity
- [ ] Supplier name + contact email
- [ ] Lead time + expected delivery date
- [ ] Safety stock calculation
- [ ] Cost estimate (if pricing data available)

**Example (Lucyd):** Yes — Generate PO recommendation with SKU, quantity, supplier, lead time, expected delivery. Ops manager reviews before placing in NetSuite.

---

## Section 7: Alerts & Reporting

### 7.1 Who needs to receive inventory alerts?
**What this configures:** Alert routing, notification channels, severity filtering

**List each recipient:**

| Name | Role | Alert Types | Delivery Channel | Contact Info |
|------|------|-------------|------------------|--------------|
| Example: Joaquin | COO | Critical stockouts, high-value late orders | Telegram | 8569898992 |
| Example: Warehouse Team | Fulfillment | Late orders, fulfillment lag | Email | warehouse@company.com |
| Example: Ops Manager | Operations | All alerts | Telegram + Slack | @opsmanager |

**Example (Lucyd):**
- Joaquin (COO): Critical stockouts, aged POs, high-value late orders → Telegram
- Ops Manager: All stock alerts, reorder recommendations → Telegram
- Warehouse: Late order buckets → Email digest

---

### 7.2 What alert delivery channels do you prefer?
**What this configures:** Integration setup, webhook URLs, bot tokens

**Check all that apply:**
- [ ] **Telegram** (instant mobile notifications)
- [ ] **Discord** (team channel notifications)
- [ ] **Slack** (workspace integration)
- [ ] **Email** (digest or individual alerts)
- [ ] **SMS** (critical alerts only)
- [ ] **Webhook** (custom integration)

**If Telegram:**
- Bot token: ________________ (or will create)
- Chat ID(s): ________________

**If Slack:**
- Webhook URL: ________________
- Channel: ________________

**If Discord:**
- Webhook URL: ________________

**Example (Lucyd):** Telegram (primary), Discord (backup for daily digests)

---

### 7.3 How often do you want summary reports?
**What this configures:** Cron schedule, report frequency, batching logic

**Options:**
- [ ] **Hourly** (high-velocity, fast-moving inventory)
- [ ] **Every 4 hours** (2-3 times per day)
- [ ] **Twice daily** (morning + afternoon)
- [ ] **Daily** (once per day, morning preferred)
- [ ] **Weekly** (low-touch monitoring)

**Preferred delivery time:** ________ (timezone: ________)

**Report sections to include:**
- [ ] Stock alerts (low stock, out of stock)
- [ ] Late orders (aging buckets)
- [ ] Reorder recommendations
- [ ] Channel balance status
- [ ] Open purchase orders
- [ ] Aged purchase orders (>45 days)

**Example (Lucyd):** Every 4 hours (10am, 2pm, 6pm, 10pm ET) for inventory watchdog, daily 9:30am for late orders

---

### 7.4 Do you want quiet hours (no alerts during certain times)?
**What this configures:** Alert suppression, batching, exception handling

**Options:**
- [ ] **Yes** (suppress non-critical alerts during off-hours)
- [ ] **No** (send alerts 24/7)

**If Yes:**
- Quiet hours: ________ to ________ (timezone: ________)
- Exceptions (still send during quiet hours):
  - [ ] Critical stockouts
  - [ ] Out of stock alerts
  - [ ] High-value late orders (>$______)

**Example (Lucyd):** No quiet hours (COO wants critical alerts 24/7, warehouse email digests sent during business hours)

---

## Section 8: Performance Targets (Optional)

### 8.1 What are your target metrics?
**What this configures:** Success benchmarks, ROI calculation, threshold tuning

**Fill in targets:**

| Metric | Current State | Target (3 months) | Target (6 months) |
|--------|---------------|-------------------|-------------------|
| Stockout prevention rate | ____% | ____% | ____% |
| Late orders (avg age) | ____ days | ____ days | ____ days |
| Fulfillment time (order to ship) | ____ hours | ____ hours | ____ hours |
| Overstock reduction | ____% | ____% | ____% |
| Ops manager time on inventory | ____ hrs/week | ____ hrs/week | ____ hrs/week |

**Example (Lucyd targets):**
- Stockout prevention: 60% → 90% → 95%
- Late orders avg age: 31 days → 18 days → 12 days
- Ops time: 15-20 hrs/week → 5 hrs/week → <2 hrs/week

---

## Next Steps

After completing this intake:

1. **Review** — We'll validate your answers and flag any gaps
2. **Configure** — We'll generate your `config.json` from `config/schema.json`
3. **Deploy** — Set up cron jobs, API integrations, alert channels
4. **Test** — Dry-run for 3-7 days, tune thresholds
5. **Launch** — Go live with alerts, iterate based on feedback

**Estimated timeline:**
- Intake completion: 20-30 minutes
- Configuration generation: 30 minutes
- Integration setup: 2-4 hours (API-based) or 30 minutes (CSV-based)
- Testing & tuning: 3-7 days
- Full deployment: 1-2 weeks

**Questions?** Review `README.md` for system overview and `examples/*.json` for real-world configurations.

---

**Version:** 1.0  
**Last updated:** April 2026  
**Part of:** JBOT Protocol — Inventory Watchdog System
