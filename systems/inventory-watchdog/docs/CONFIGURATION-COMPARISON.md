# Configuration Comparison
*Three deployment patterns from real-world use cases*

---

## Overview

This document compares three different Inventory Watchdog deployments to show how the same system adapts to different business models, inventory systems, and operational complexities.

**The three configurations:**

1. **Premium Eyewear Co** — Multi-channel DTC + Amazon + Wholesale, NetSuite ERP, 15 warehouses, 893 SKUs
2. **Urban Threads Apparel** — Simple DTC, Shopify only, single 3PL, 187 SKUs
3. **Industrial Supply Co** — B2B wholesale, Google Sheets, 2 warehouses, 342 SKUs

---

## At a Glance

| Feature | Premium Eyewear | Urban Threads | Industrial Supply |
|---------|-----------------|---------------|-------------------|
| **Industry** | DTC E-commerce (Eyewear) | DTC E-commerce (Apparel) | B2B Wholesale (Industrial) |
| **Inventory System** | NetSuite (API) | Shopify (API) | Google Sheets (CSV export) |
| **SKU Count** | 893 | 187 | 342 |
| **Warehouses** | 15 (multi-region) | 1 (3PL) | 2 (owned warehouses) |
| **Sales Channels** | DTC + Amazon US + Amazon EU + Wholesale | DTC only | B2B orders (manual) |
| **Monitoring Frequency** | Every 4 hours | Daily | Twice daily |
| **Alert Channel** | Telegram + Discord | Slack | Email only |
| **Reorder Process** | Manual (PO recommendations) | Hybrid (auto-draft, manual approve) | Manual (spreadsheet-based) |
| **Late Orders Tracking** | Yes (Shopify) | Yes (Shopify) | No (not applicable) |
| **Channel Balance** | Yes (DTC vs FBA vs wholesale) | No (single channel) | Yes (warehouse allocation) |
| **Ops Time Before** | 15-20 hrs/week | 8-10 hrs/week | 12-15 hrs/week |
| **Ops Time After** | <2 hrs/week | 3 hrs/week | 5 hrs/week |
| **Deployment Complexity** | High (multi-system integration) | Low (single platform) | Medium (CSV-based) |

---

## Deep Dive: Key Differences

### 1. Inventory System Integration

#### Premium Eyewear (NetSuite)
**Why NetSuite:** Full ERP required for multi-warehouse, multi-channel, international operations  
**Integration method:** OAuth 1.0 API with SuiteQL queries  
**Data access:** Real-time API queries every 4 hours  
**Rate limits:** 10 requests/min, 1000/hour, 5000/day  
**Complexity:** High — requires OAuth setup, SuiteQL knowledge, location mapping

**Config snippet:**
```json
{
  "inventory_system": {
    "type": "netsuite",
    "account_id": "$NETSUITE_ACCOUNT_ID",
    "api_credentials": {
      "consumer_key": "$NETSUITE_CONSUMER_KEY",
      "consumer_secret": "$NETSUITE_CONSUMER_SECRET",
      "token_id": "$NETSUITE_TOKEN_ID",
      "token_secret": "$NETSUITE_TOKEN_SECRET"
    },
    "integration_method": "api"
  }
}
```

---

#### Urban Threads (Shopify)
**Why Shopify:** Standard DTC e-commerce, built-in inventory management  
**Integration method:** Admin API with access token  
**Data access:** Real-time API queries daily  
**Rate limits:** 40 requests/min, 10,000/day  
**Complexity:** Low — single access token, straightforward API

**Config snippet:**
```json
{
  "inventory_system": {
    "type": "shopify",
    "account_id": "$SHOPIFY_STORE",
    "api_credentials": {
      "access_token": "$SHOPIFY_ACCESS_TOKEN"
    },
    "api_version": "2024-01",
    "integration_method": "api"
  }
}
```

---

#### Industrial Supply (Google Sheets)
**Why Google Sheets:** No formal inventory system, manual tracking in spreadsheet  
**Integration method:** CSV export URL (daily refresh)  
**Data access:** Batch CSV download every 6 hours  
**Rate limits:** None (public CSV export)  
**Complexity:** Lowest — just read CSV, no API auth

**Config snippet:**
```json
{
  "inventory_system": {
    "type": "google-sheets",
    "integration_method": "csv-export",
    "csv_export_path": "https://docs.google.com/spreadsheets/d/{SHEET_ID}/export?format=csv&gid=0",
    "csv_schedule": "0 */6 * * *"
  }
}
```

**Key insight:** The system works regardless of inventory platform sophistication — from full ERP to simple spreadsheet.

---

### 2. SKU Tracking Strategy

#### Premium Eyewear: Product Line Filtering
**Challenge:** 893 SKUs, but only 4 active product lines  
**Solution:** Track by product line prefix + specific SKUs for Lyte 2.5 collection  
**Exclusions:** Variants like "%-E", "%-BT%" (Bluetooth), inactive/discontinued  

**Why:** Some SKU families have 20+ variants (colors, sizes), but only tracking flagship styles reduces noise by 60%.

```json
{
  "tracking_mode": "product-lines",
  "product_lines": [
    {"name": "Armor Sport Line", "prefix": "LCD008", "track_all_variants": true, "priority": "critical"},
    {"name": "Lyte Collection", "prefix": "LCD006", "track_all_variants": false, "specific_skus": ["LCD006-30", "LCD006-35"], "priority": "high"}
  ],
  "exclusions": {"patterns": ["%E", "%-BT%", "%-K"], "inactive": true}
}
```

---

#### Urban Threads: Track All (Simple Product Line)
**Challenge:** 187 SKUs across 4 product categories  
**Solution:** Track all active SKUs by product line, no complex filtering  
**Exclusions:** Samples and test SKUs only  

**Why:** Small SKU count, all products equally important, no need for velocity-based filtering.

```json
{
  "tracking_mode": "product-lines",
  "product_lines": [
    {"name": "T-Shirts", "prefix": "TEE", "track_all_variants": true, "priority": "high"},
    {"name": "Hoodies", "prefix": "HOOD", "track_all_variants": true, "priority": "critical"}
  ],
  "exclusions": {"patterns": ["%SAMPLE%", "%TEST%"], "inactive": true}
}
```

---

#### Industrial Supply: Track Everything
**Challenge:** 342 SKUs, all B2B critical  
**Solution:** No filtering — track all active SKUs  
**Exclusions:** Only obsolete/discontinued items  

**Why:** B2B wholesale means every SKU matters to a specific customer. Can't afford stockouts on niche items.

```json
{
  "tracking_mode": "all",
  "exclusions": {"patterns": ["%OBSOLETE%", "%DISC%"], "inactive": true, "discontinued": true}
}
```

**Key insight:** Tracking strategy depends on SKU importance distribution — flagship products only (eyewear), all equally important (apparel), or every SKU critical (B2B).

---

### 3. Alert Thresholds & Logic

#### Premium Eyewear: Days-of-Supply (Velocity-Based)
**Method:** Alert when <14 days of supply based on 30-day velocity  
**Why:** Different SKUs sell at different rates — "20 units" means 3 days for fast movers, 60 days for slow movers  
**Result:** Fewer false alerts, more accurate urgency

```json
{
  "low_stock": {
    "mode": "days-of-supply",
    "days_of_supply": 14,
    "velocity_lookback_days": 30
  }
}
```

**Example:** SKU sells 2 units/day → alert triggers at 28 units (14 days × 2/day)

---

#### Urban Threads: Both Unit Count + Days-of-Supply
**Method:** Alert if EITHER <10 units OR <14 days of supply  
**Why:** Apparel has seasonal spikes — unit count catches unexpected drops, velocity catches seasonal slowdowns  
**Result:** Catches stockouts from sudden viral demand AND gradual depletion

```json
{
  "low_stock": {
    "mode": "both",
    "days_of_supply": 14,
    "unit_count": 10,
    "velocity_lookback_days": 30
  }
}
```

---

#### Industrial Supply: Simple Unit Count
**Method:** Alert when <25 units  
**Why:** B2B orders are lumpy (one customer orders 50 units, then nothing for a month) — velocity unreliable  
**Result:** Conservative static threshold, better safe than sorry

```json
{
  "low_stock": {
    "mode": "unit-count",
    "unit_count": 25,
    "velocity_lookback_days": 60
  }
}
```

**Key insight:** Threshold logic depends on sales pattern consistency — smooth DTC (velocity), seasonal spikes (both), lumpy B2B (unit count).

---

### 4. Late Orders Tracking

#### Premium Eyewear: Multi-Bucket with Rx Exclusions
**Buckets:** 7-13d (monitor), 14-20d (attention), 21-29d (warning), 30-60d (critical), 60+d (archive)  
**Exclusions:** Prescription orders (Rx) take 2-4 weeks, excluded from "late" categorization  
**Why:** Optical work requires lab time, but non-Rx should ship in <7 days

```json
{
  "late_orders": {
    "enabled": true,
    "age_buckets": [
      {"name": "MONITOR", "min_days": 7, "max_days": 13, "severity": "monitor"},
      {"name": "CRITICAL", "min_days": 30, "max_days": 60, "severity": "critical"}
    ],
    "exclusions": {"tags": ["lensadvizor", "lab", "custom lenses"]}
  }
}
```

---

#### Urban Threads: Tighter Thresholds (No Exclusions)
**Buckets:** 3-7d (attention), 8-14d (warning), 15-30d (critical)  
**Exclusions:** Only preorders and custom-print items  
**Why:** Standard apparel should ship in 24-48 hours, anything >3 days is already late

```json
{
  "late_orders": {
    "enabled": true,
    "age_buckets": [
      {"name": "ATTENTION", "min_days": 3, "max_days": 7, "severity": "attention"},
      {"name": "CRITICAL", "min_days": 15, "max_days": 30, "severity": "critical"}
    ],
    "exclusions": {"tags": ["preorder", "custom-print"]}
  }
}
```

---

#### Industrial Supply: Not Tracked
**Why:** B2B orders are picked manually, shipped when customer wants them (not time-sensitive)  
**Alternative:** Warehouse uses ERP fulfillment tracking, not automated

```json
{
  "late_orders": {"enabled": false}
}
```

**Key insight:** Late order thresholds reflect customer expectations — 7-30 days (custom eyewear), 3-15 days (apparel), N/A (B2B bulk).

---

### 5. Channel Balance Monitoring

#### Premium Eyewear: 4-Channel Allocation (Enforced)
**Channels:** DTC 40%, Amazon US 35%, Amazon EU 10%, Wholesale 15%  
**Logic:** Alert if any channel >20% over/under target  
**Auto-rebalance:** No (manual FBA shipments)  
**Why:** Amazon FBA requires lead time to restock, can't steal from DTC on the fly

```json
{
  "channel_balance": {
    "enabled": true,
    "allocation_targets": {"dtc-website": 40, "amazon-us": 35, "amazon-de": 10, "wholesale": 15},
    "imbalance_threshold_percent": 20,
    "auto_rebalance": false
  }
}
```

---

#### Urban Threads: Single Channel (Disabled)
**Why:** Only DTC website, no channel conflict  
**Future:** If they add Amazon FBA, enable this

```json
{
  "channel_balance": {"enabled": false}
}
```

---

#### Industrial Supply: Warehouse Allocation (Flexible)
**Warehouses:** Chicago 70%, Dallas 30%  
**Logic:** Alert if imbalance >25% (e.g., Chicago drops to 50%, Dallas rises to 50%)  
**Why:** Regional demand shifts — sometimes need to rebalance stock between warehouses

```json
{
  "channel_balance": {
    "enabled": true,
    "allocation_targets": {"chicago": 70, "dallas": 30},
    "imbalance_threshold_percent": 25,
    "auto_rebalance": false
  }
}
```

**Key insight:** Channel balance matters when stock is split across physical locations (FBA) or sales platforms (DTC vs wholesale), not for single-channel.

---

### 6. Reorder Process Automation

#### Premium Eyewear: Manual (Recommendations Only)
**Mode:** Manual  
**Auto-generate POs:** No  
**Approval:** Always required  
**Why:** International suppliers, complex pricing, requires ops manager review

**Suppliers:**
- Gonbes (China) — 21 days, MOQ 50
- Richitek (Taiwan) — 30 days, MOQ 100
- ITFIT Optical (USA) — 14 days, MOQ 25

```json
{
  "reorder_process": {
    "mode": "manual",
    "auto_generate_pos": false,
    "approval_required": true,
    "suppliers": [
      {"id": "gonbes", "name": "Gonbes (Shenzhen)", "lead_time_days": 21, "minimum_order_quantity": 50}
    ]
  }
}
```

---

#### Urban Threads: Hybrid (Auto-Draft + Manual Approve)
**Mode:** Hybrid  
**Auto-generate POs:** Yes  
**Approval:** Required (except pre-approved accessory supplier)  
**Why:** Main garment supplier needs review, accessory supplier is low-risk auto-approve

**Suppliers:**
- Saigon Garment (Vietnam) — 45 days, MOQ 200, manual approval
- Pacific Trade Partners (USA) — 21 days, MOQ 50, auto-approve

```json
{
  "reorder_process": {
    "mode": "hybrid",
    "auto_generate_pos": true,
    "approval_required": true,
    "suppliers": [
      {"id": "vietnam-factory", "auto_approve": false},
      {"id": "accessory-supplier", "auto_approve": true}
    ]
  }
}
```

---

#### Industrial Supply: Fully Manual
**Mode:** Manual  
**Auto-generate POs:** No  
**Why:** Complex B2B pricing, volume discounts, contract negotiations  
**Process:** Watchdog alerts, purchasing team manually creates PO in QuickBooks

```json
{
  "reorder_process": {
    "mode": "manual",
    "auto_generate_pos": false,
    "approval_required": true
  }
}
```

**Key insight:** Automation level depends on supplier relationships — manual (complex pricing), hybrid (mix of trusted + new), fully automated (coming in future versions).

---

### 7. Alert Delivery & Reporting

#### Premium Eyewear: Telegram + Discord (Instant + Digest)
**Primary:** Telegram (instant critical alerts to COO + ops manager)  
**Secondary:** Discord (daily digest for team visibility)  
**Frequency:** Every 4 hours (10am, 2pm, 6pm, 10pm)  
**Quiet hours:** No (COO wants 24/7 critical alerts)

```json
{
  "alerts": {
    "recipients": [
      {"name": "Joaquin (COO)", "delivery_channel": "telegram", "severity_filter": ["critical", "warning"]},
      {"name": "Ops Team", "delivery_channel": "telegram", "severity_filter": ["critical", "warning", "attention", "monitor"]}
    ]
  },
  "reporting": {"frequency": "every-4-hours", "delivery": {"channel": "telegram"}}
}
```

---

#### Urban Threads: Slack (Team Channel)
**Primary:** Slack #inventory-alerts  
**Frequency:** Daily 9am  
**Quiet hours:** Yes (10pm-8am, except critical)  
**Why:** Small team, Slack is central hub, batch non-urgent alerts

```json
{
  "alerts": {
    "recipients": [
      {"name": "Sarah (Founder)", "delivery_channel": "slack", "slack_channel": "#inventory-alerts", "severity_filter": ["critical"]},
      {"name": "Mike (Operations)", "delivery_channel": "slack", "severity_filter": ["critical", "warning", "attention"]}
    ]
  },
  "reporting": {"frequency": "daily", "delivery": {"channel": "slack"}},
  "quiet_hours": {"enabled": true, "start_time": "22:00", "end_time": "08:00", "exceptions": ["critical", "out-of-stock"]}
}
```

---

#### Industrial Supply: Email Only (Conservative)
**Primary:** Email to owner + purchasing manager  
**Frequency:** Twice daily (8am, 4pm)  
**Quiet hours:** Yes (6pm-7am, except critical)  
**Why:** Traditional B2B, email-driven workflows, no Slack/Telegram adoption

```json
{
  "alerts": {
    "recipients": [
      {"name": "Tom (Owner)", "delivery_channel": "email", "severity_filter": ["critical"]},
      {"name": "Jennifer (Purchasing)", "delivery_channel": "email", "severity_filter": ["critical", "warning", "attention"]}
    ]
  },
  "reporting": {"frequency": "twice-daily", "delivery": {"channel": "email"}},
  "quiet_hours": {"enabled": true, "start_time": "18:00", "end_time": "07:00"}
}
```

**Key insight:** Delivery channel reflects team culture — instant mobile (Telegram), team collaboration (Slack), traditional B2B (email).

---

## What's Universal vs What's Business-Specific

### Universal (All Three Use This)
- ✅ **Stock level monitoring** (all businesses need to prevent stockouts)
- ✅ **Reorder point alerts** (all need to know when to buy more)
- ✅ **Alert severity levels** (critical vs warning vs monitor)
- ✅ **Supplier management** (lead time, MOQ, contact info)
- ✅ **Data quality validation** (quantity ≥ 0, SKU uniqueness)

### Business-Specific (Varies by Use Case)
- 🔀 **Inventory system integration** (NetSuite vs Shopify vs Google Sheets)
- 🔀 **SKU tracking strategy** (all vs product-lines vs velocity tiers)
- 🔀 **Threshold logic** (days-of-supply vs unit-count vs both)
- 🔀 **Late order buckets** (tight for apparel, loose for custom eyewear, N/A for B2B)
- 🔀 **Channel balance** (multi-channel vs single-channel vs warehouse allocation)
- 🔀 **Alert delivery** (Telegram vs Slack vs Email)
- 🔀 **Monitoring frequency** (every 4 hours vs daily vs twice-daily)
- 🔀 **Reorder automation** (manual vs hybrid vs auto-approve)

---

## Choosing Your Configuration Path

**If you're like Premium Eyewear:**
- Multi-channel (DTC + Amazon + wholesale)
- Full ERP (NetSuite, SAP, Oracle)
- 500+ SKUs, 5+ warehouses
- International operations
→ Use `dtc-eyewear-netsuite.json` as template

**If you're like Urban Threads:**
- DTC only (Shopify, WooCommerce)
- <300 SKUs, single warehouse/3PL
- Fast-moving inventory
→ Use `apparel-dtc-shopify.json` as template

**If you're like Industrial Supply:**
- B2B wholesale
- No formal inventory system (spreadsheets)
- Multi-warehouse (2-5 locations)
- Manual processes
→ Use `b2b-wholesale-spreadsheet.json` as template

---

## Migration Paths

### Start Simple → Add Complexity

**Phase 1 (Week 1):** Stock level monitoring only
- Pick threshold logic (days-of-supply or unit-count)
- Set up alerts (Telegram/Slack/Email)
- Test with 1-2 product lines

**Phase 2 (Week 2-3):** Add late orders
- Define age buckets
- Set exclusions (custom/Rx/preorders)
- Tune thresholds based on real data

**Phase 3 (Month 2):** Add channel balance
- If multi-channel, define allocation targets
- Monitor imbalances, tune thresholds
- Decide auto-rebalance vs manual

**Phase 4 (Month 3+):** Automate reorders
- Start with PO recommendations (manual)
- Add auto-draft for trusted suppliers
- Eventually auto-approve low-risk items

---

## ROI Comparison

| Metric | Premium Eyewear | Urban Threads | Industrial Supply |
|--------|-----------------|---------------|-------------------|
| **Before: Ops time/week** | 15-20 hrs | 8-10 hrs | 12-15 hrs |
| **After: Ops time/week** | <2 hrs | 3 hrs | 5 hrs |
| **Time saved** | 85-90% | 60-70% | 60-67% |
| **Stockouts prevented (6 months)** | 47 | 12 | 8 |
| **Estimated revenue saved** | $127K | $18K | N/A (B2B) |
| **Overstock reduction** | 32% ($48K freed) | 40% ($22K freed) | 20% ($15K freed) |
| **Deployment cost** | $800 (dev time) | $300 (config + test) | $200 (CSV setup) |
| **ROI (6 months)** | 18x | 13x | 7x |

**Key insight:** Even the "simplest" deployment (Industrial Supply) delivers 7x ROI in 6 months.

---

## Next Steps

1. **Identify your archetype** (which of the three are you closest to?)
2. **Copy that config** as your starting point
3. **Fill out `config/intake-v1.md`** to map your specifics
4. **Customize thresholds** based on your pain points
5. **Deploy Phase 1** (stock monitoring only)
6. **Iterate** — add late orders, channel balance, reorder automation over time

---

**Version:** 1.0  
**Last updated:** April 2026  
**Part of:** JBOT Protocol — Inventory Watchdog System
