# Sector Research Starter Pack

**8 industry verticals. For each: benchmark companies, AI-ready workflows, common tool stacks, pain points, research sources, and readiness red flags.**

Use this document during Phase 0 to orient sector research quickly. These are starting points, not comprehensive analyses. Your job is to go deeper on the specific sub-vertical and company size of your operator.

---

## How to Use This Document

1. Find your operator's vertical below
2. Use the benchmark companies to study what "good" looks like at scale
3. Use the AI-ready workflows to prime your operator interview — ask specifically about these
4. Use the red flags to determine if the sector/operator is ready to deploy now or needs foundational work first
5. Follow the research sources for deeper due diligence

---

## 1. Retail & E-Commerce

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| Shopify | Public, large | Their own merchant AI tools; how they've automated ops at scale |
| Wayfair | Public, large | Demand forecasting, inventory AI, customer service automation |
| Chewy | Public, mid-large | Customer service automation, subscription management, reorder prediction |
| Allbirds | Public, mid | DTC operations, sustainability reporting, small-team efficiency |
| A regional DTC brand with 20–100 SKUs | Private, SMB | Benchmark via trade press; look for case studies in Shopify Blog or Digital Commerce 360 |

### AI-Ready Workflows

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Order status updates and exception alerts | Continuous | Structured data in e-commerce platform; clear rules for "exception" |
| Inventory reorder point alerts | Daily | Threshold math on structured data; model in Haiku tier |
| Product description generation | On-demand | High volume, formulaic structure; strong ROI vs. copywriter time |
| Return rate analysis by SKU | Weekly | Clean transaction data; pattern recognition across structured fields |
| Customer segment performance report | Weekly | Standard analytics on CRM + e-commerce data |
| Abandoned cart sequence triggers | Continuous | Fully rules-based; typically handled by ESP but can be agent-assisted |

### Common Tool Stack

| Category | Common Tools |
|---|---|
| E-commerce platform | Shopify, WooCommerce, BigCommerce |
| ERP / inventory | NetSuite, Cin7, Brightpearl |
| CRM / email | Klaviyo, HubSpot, Mailchimp |
| Analytics | Google Analytics 4, Triple Whale, Northbeam |
| Customer support | Gorgias, Zendesk, Freshdesk |
| Fulfillment | ShipBob, ShipStation, own 3PL |

### Pain Points AI Addresses Well

- Ops manager is manually checking orders every morning for exceptions
- Customer service team is answering the same 10 questions repeatedly
- Merchandising decisions are based on gut feel, not data, because the data is too slow to pull
- Marketing reports take 2–3 hours each Monday to compile manually

### Research Sources

- Digital Commerce 360: industry benchmarks, case studies
- Shopify Commerce Trends: annual report on merchant operations
- Klaviyo Benchmark Report: email and SMS performance by vertical
- eMarketer: retail technology adoption data
- NRF (National Retail Federation): technology investment research

### Red Flags — Not Ready Yet

- Product catalog is inconsistent (different SKU formats, missing fields, duplicate entries)
- No e-commerce platform or all sales are offline / manual order entry
- Customer data is scattered across multiple disconnected systems with no clean CRM
- Return rate is >25% and the cause is unknown — product issues will overwhelm AI outputs with noise
- Operator has no one who "owns" the data and can answer basic questions about what's in each system

---

## 2. Manufacturing & Operations

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| Siemens | Public, large | Manufacturing AI, predictive maintenance, digital twin operations |
| Rockwell Automation | Public, large | Industrial automation AI; how they've integrated AI into ops |
| Watts Water Technologies | Public, mid | Lean manufacturing with AI augmentation; operations efficiency |
| A contract manufacturer, 200–500 employees | Private, mid | Look for case studies in IndustryWeek or Manufacturing Tomorrow |
| A job shop or custom fabricator, <50 employees | Private, SMB | Trade association publications; NIST Manufacturing Extension Partnership |

### AI-Ready Workflows

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Production schedule exception alerts | Daily | Structured data in MES/ERP; clear deviation rules |
| Supplier lead time monitoring | Weekly | Structured PO data; comparison against committed dates |
| Quality defect rate reporting by line/shift | Daily | Clean production data; pattern recognition |
| Inventory variance alerts | Daily | ERP threshold math |
| Maintenance request triage and routing | Continuous | Rules-based classification from structured request fields |
| Weekly ops summary for plant manager | Weekly | Multi-source synthesis of structured data |

### Common Tool Stack

| Category | Common Tools |
|---|---|
| ERP | SAP, Oracle, NetSuite, Epicor, Infor |
| MES (Manufacturing Execution) | Plex, Tulip, Ignition |
| Quality management | ETQ, InfinityQS, MasterControl |
| Maintenance (CMMS) | Fiix, UpKeep, Limble |
| Supply chain / procurement | Coupa, Jaggaer, SAP Ariba |

### Pain Points AI Addresses Well

- Plant manager spends 2 hours each morning pulling together a picture of what happened overnight
- Quality issues are discovered downstream because no one is watching the leading indicators
- Supplier delays aren't caught until a production line goes down
- Maintenance is reactive because no one has time to analyze the work order data for patterns

### Research Sources

- IndustryWeek: manufacturing operations benchmarks
- Manufacturing Tomorrow: technology adoption case studies
- Deloitte Manufacturing Study (annual): AI and automation maturity
- MIT Digital Economy Initiative: research on AI in manufacturing
- NIST Manufacturing Extension Partnership: SMB-focused implementation guides

### Red Flags — Not Ready Yet

- Machine data is trapped in proprietary PLCs with no API access
- ERP data quality is poor — costing, routing, and BOM data are unreliable
- Production floor is resistant to any digital tools (paper traveler cards, no MES)
- Quality data is captured on paper and batch-entered weekly
- IT infrastructure doesn't support real-time data access (air-gapped systems, legacy architecture)

---

## 3. Logistics & Freight

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| C.H. Robinson | Public, large | AI-powered load matching, carrier management, customer visibility |
| XPO Logistics | Public, large | Predictive freight, exception management at scale |
| Echo Global Logistics | Public, mid | Brokerage AI; how they've automated carrier sourcing |
| A regional 3PL, 50–200 employees | Private, mid | FreightWaves case studies; trade press |
| An owner-operator freight broker, 5–20 employees | Private, SMB | Look for DAT or Freightos research on small broker operations |

### AI-Ready Workflows

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Shipment status and exception monitoring | Continuous | Structured tracking data; clear exception criteria |
| Carrier performance scorecards | Weekly | Clean structured data from TMS |
| Late delivery alerts to customers | Continuous | Threshold rules on structured ETA data |
| Rate quote compilation | On-demand | Structured inputs; consistent output format |
| Weekly freight cost analysis | Weekly | TMS + invoice data; structured aggregation |
| Claims status tracking | Ongoing | Structured case data; rules-based follow-up |

### Common Tool Stack

| Category | Common Tools |
|---|---|
| TMS (Transportation Management) | McLeod, TMW, MercuryGate, Freight OS |
| Load boards | DAT, Truckstop, Coyote |
| Tracking | project44, FourKites, Visibility Hub |
| ERP / accounting | QuickBooks, NetSuite, SAP |
| Customer portal | Custom or TMS-native |

### Pain Points AI Addresses Well

- Ops team manually refreshes tracking portals every hour to catch exceptions before customers notice
- Account managers spend 30–60 min each morning pulling together customer status updates
- Carrier scorecarding is done quarterly (or never) because it takes too long
- Claims follow-up falls through the cracks because there's no structured tracking

### Research Sources

- FreightWaves: industry news and data; strong on technology adoption
- Supply Chain Dive: operational AI case studies
- Freightos Research: SMB freight brokerage technology
- DAT Intelligence: carrier and market data insights
- McKinsey Supply Chain Report (annual): strategic benchmarks

### Red Flags — Not Ready Yet

- No TMS — all freight is managed through carrier portals and spreadsheets
- Carrier relationships are informal with no structured performance data
- Invoicing is manual with frequent discrepancies — data quality issue before AI can help
- Customer communication is entirely ad-hoc — no structured touchpoint cadence
- Small team (2–5 people) where the "process" is entirely one person's judgment

---

## 4. Real Estate (Brokerage & Property Management)

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| CBRE | Public, large | AI in property management, lease analysis, market intelligence |
| Compass | Public, mid-large | Agent-facing AI tools; lead routing and nurture automation |
| Marcus & Millichap | Public, mid | Investment sales AI; research and comp automation |
| A regional brokerage, 20–100 agents | Private, mid | NAR tech survey; Inman case studies |
| A property management company, 200–2,000 units | Private, SMB | Property management publication benchmarks |

### AI-Ready Workflows

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Lead response and qualification routing | Continuous | Rules-based; structured inquiry data |
| Listing performance summary | Weekly | Structured MLS + website analytics data |
| Maintenance request routing and follow-up | Continuous | Structured request data; rules-based assignment |
| Lease expiration alerts | Monthly | Calendar + structured lease data |
| Rent delinquency follow-up triggers | Monthly | Structured payment data; rules-based escalation |
| Market comp summary for listing presentations | On-demand | Structured MLS data; consistent output format |

### Common Tool Stack

| Category | Common Tools |
|---|---|
| CRM | Follow Up Boss, LionDesk, HubSpot, Salesforce |
| Property management | AppFolio, Buildium, Yardi, RealPage |
| Transaction management | Dotloop, DocuSign Rooms, Skyslope |
| Marketing | BombBomb, Mailchimp, Canva |
| MLS access | Board-specific (Stellar MLS, CRMLS, etc.) |

### Pain Points AI Addresses Well

- Leads sit uncontacted for 2–6 hours because no one is watching the inbox
- Agents don't follow up systematically — top agents have a system, most don't
- Property managers are manually tracking lease expirations in spreadsheets
- Maintenance follow-up falls through the cracks between the request and the resolution

### Research Sources

- NAR Technology Survey (annual): agent technology adoption benchmarks
- Inman News: real estate technology case studies
- CBRE Real Estate Outlook: institutional market intelligence
- PropTech Insider: venture-backed proptech tracking
- National Apartment Association: property management operations research

### Red Flags — Not Ready Yet

- CRM is not being used consistently — contacts are scattered across email, phone, and spreadsheets
- Listings data is not structured or regularly updated
- Agent compensation and commission tracking is manual and disputed — fix this before automating
- Brokerage has fewer than 5 agents — not enough volume to justify fleet deployment

---

## 5. Healthcare (Clinics, Practices, Health Systems — Non-Clinical)

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| HCA Healthcare | Public, large | Operational AI (non-clinical): scheduling, supply chain, staff optimization |
| DaVita | Public, large | Operational efficiency; patient communication automation |
| Amedisys | Public, mid | Home health operations; scheduling and compliance |
| A multi-location specialty practice, 5–20 providers | Private, mid | MGMA benchmarks; Health Affairs case studies |
| A single-location primary care or specialty practice | Private, SMB | HIMSS small practice technology adoption data |

### AI-Ready Workflows (Non-Clinical Only)

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Appointment reminder and no-show reduction | Daily | Structured scheduling data; rules-based outreach |
| Prior authorization status tracking | Continuous | Structured payer data; defined follow-up rules |
| Billing exception and denial rate monitoring | Daily | Structured claims data; clear exception criteria |
| Staff scheduling conflict alerts | Daily | Structured scheduling data |
| Inventory reorder alerts (medical supplies) | Daily | Threshold rules on structured inventory data |
| Insurance aging report summary | Weekly | Structured AR data; consistent output format |

**Important:** All AI workflows in healthcare must be strictly non-clinical. Do not automate anything that touches diagnosis, treatment recommendations, or clinical decision support without specialized regulatory guidance.

### Common Tool Stack

| Category | Common Tools |
|---|---|
| EHR / PM | Epic, Athenahealth, eClinicalWorks, Kareo |
| Billing | Kareo, AdvancedMD, Waystar, Availity |
| Scheduling | Zocdoc, Phreesia, NexHealth |
| Communication | Relatient, Luma Health, Klara |
| Supply chain | Provista, Cardinal Health tools |

### Pain Points AI Addresses Well

- Front desk staff spend significant time on manual appointment reminders
- Prior auth follow-up is reactive — denials discovered during billing, not during the auth process
- Billing team doesn't have a daily view of denial rates by payer — only learns of problems monthly
- Supply ordering is driven by whoever notices supplies are low, not by structured thresholds

### Research Sources

- HIMSS Analytics: health IT adoption benchmarks
- MGMA (Medical Group Management Association): practice operations benchmarks
- Health Affairs: healthcare operations research
- NEJM Catalyst: operational transformation case studies
- American Hospital Association: technology investment research

### Red Flags — Not Ready Yet

- EHR data is not structured or accessible via API — many older systems have no integration path
- Billing data quality is poor (high error rate, inconsistent coding)
- Practice has not achieved HIPAA security compliance basics — fix before adding AI data flows
- Clinical and administrative workflows are not clearly separated — too much risk of scope creep into clinical territory
- Staff is significantly resistant to technology generally — change management prerequisite not met

---

## 6. Professional Services (Legal, Accounting, Consulting)

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| Deloitte | Private, large | Internal AI deployment for ops; client service automation |
| BDO | Private, mid-large | Accounting workflow automation; audit technology |
| A mid-size law firm (50–200 attorneys) | Private, mid | ALM Intelligence research; legal tech adoption data |
| A regional accounting firm (10–50 CPAs) | Private, mid | Accounting Today technology benchmarks |
| A boutique consulting firm (5–20 consultants) | Private, SMB | Harvard Business Review; peer-reviewed practice management research |

### AI-Ready Workflows

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Matter/project status summary | Weekly | Structured data in practice management system |
| Billing and time entry exception alerts | Daily | Structured time tracking data; clear rules for under-billing |
| Client deliverable deadline tracking | Continuous | Calendar + project management data |
| New matter/engagement intake routing | On-demand | Structured intake form → routing rules |
| Staff utilization rate monitoring | Weekly | Time tracking data; threshold math |
| Accounts receivable aging and follow-up triggers | Weekly | Structured billing data; rules-based escalation |

### Common Tool Stack

| Category | Common Tools |
|---|---|
| Practice management (legal) | Clio, MyCase, PracticePanther, iManage |
| Practice management (accounting) | Karbon, XPM, CCH Axcess, Thomson Reuters |
| Time tracking | Harvest, Toggl, practice management native |
| Document management | iManage, NetDocuments, SharePoint |
| CRM / BD | HubSpot, Salesforce, InterAction |

### Pain Points AI Addresses Well

- Partners don't have a live view of which matters are at risk of missing deadlines
- Billing is consistently late because time entry is inconsistent and hard to track
- Business development follow-up is ad-hoc — no systematic nurture of prospects
- Staff utilization is monitored monthly; by the time a problem is identified, it's already a problem

### Research Sources

- ALM Intelligence (legal operations research)
- Accounting Today: technology adoption benchmarks for accounting firms
- Thomson Reuters Institute: professional services operations research
- Harvard Business Review: professional services management
- ILTA (International Legal Technology Association): legal tech adoption surveys

### Red Flags — Not Ready Yet

- Time entry is inconsistent or not done in a structured system — can't automate billing management without clean data
- Client confidentiality policies are not clearly defined for AI data flows — legal/compliance prerequisite
- Matter data is not structured — files and notes live in email and shared drives with no tagging
- Partners are the primary obstacle — senior buy-in at this level is a hard prerequisite

---

## 7. Hospitality (Hotels, Restaurants, Events)

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| Marriott International | Public, large | Revenue management AI; guest experience automation |
| Hyatt Hotels | Public, large | Operations AI; loyalty program and guest service automation |
| Apple Hospitality REIT | Public, mid | Asset management and operations efficiency |
| A regional hotel group, 3–10 properties | Private, mid | American Hotel & Lodging Association tech research |
| An independent boutique hotel, 20–100 rooms | Private, SMB | Skift research; Hotel Management magazine case studies |

### AI-Ready Workflows

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Daily occupancy and revenue summary | Daily | Structured PMS data; consistent report format |
| Housekeeping task routing and priority | Daily | Structured room status + checkout schedule |
| Online review monitoring and alert | Continuous | Structured review platform data; sentiment classification |
| F&B cost variance alerts | Daily | Structured POS + inventory data |
| Event booking follow-up sequences | On-demand | Structured inquiry data; rules-based nurture |
| Staff labor cost vs. forecast alert | Daily | Structured scheduling + payroll data |

### Common Tool Stack

| Category | Common Tools |
|---|---|
| PMS (Property Management) | Opera, Mews, Cloudbeds, RMS Cloud |
| Revenue management | IDeaS, Duetto, Revinate |
| POS (F&B) | Toast, Square, Lightspeed |
| Review management | Revinate, TrustYou, ReviewPro |
| HR / scheduling | HotSchedules, Sling, 7shifts |

### Pain Points AI Addresses Well

- GM spends the first hour of every day manually pulling together a picture of last night's performance
- Online reviews go unmonitored for days — negative reviews compound before anyone responds
- Labor costs spike above forecast because scheduling isn't connected to real-time occupancy
- F&B cost control is monthly — issues aren't caught until they've been going on for weeks

### Research Sources

- Skift Research: hospitality technology adoption
- Hotel Management: operations and technology case studies
- American Hotel & Lodging Association (AHLA): technology investment surveys
- Cornell Center for Hospitality Research: academic research on hospitality operations
- STR (CoStar): hospitality performance benchmarks

### Red Flags — Not Ready Yet

- PMS data quality is poor — inconsistent rates, orphaned reservations, unreliable occupancy data
- Multiple properties on different PMS systems with no data integration — fix before fleet deployment
- High staff turnover makes MEMORY.md maintenance impractical — every bot's contact list is outdated weekly
- No structured process for anything — "we figure it out every day" culture resists rules-based automation

---

## 8. Financial Services (RIAs, Mortgage, Insurance Operations)

### Benchmark Companies (Large → Small)

| Company | Size | What to Study |
|---|---|---|
| Vanguard | Private, large | Client communication automation; operational AI in asset management |
| Raymond James | Public, mid-large | Advisor-facing AI tools; compliance automation |
| LPL Financial | Public, mid-large | Advisor operations automation; compliance monitoring |
| A regional RIA, 5–25 advisors | Private, mid | Aite-Novarica research; WealthManagement.com case studies |
| An independent insurance agency, 5–20 staff | Private, SMB | IIABA technology survey; Insurance Journal |

### AI-Ready Workflows

| Workflow | Frequency | Why AI-Ready |
|---|---|---|
| Client account summary and exception alerts | Daily | Structured custodian data; clear exception criteria |
| Compliance deadline tracking | Continuous | Structured regulatory calendar; rules-based |
| Application/policy status monitoring | Continuous | Structured case management data |
| Client birthday and review milestone alerts | Ongoing | CRM calendar data; rules-based triggers |
| Premium/AUM reporting | Monthly | Structured financial data; consistent output format |
| E&O and licensing expiration tracking | Ongoing | Structured expiration data; threshold alerts |

### Common Tool Stack

| Category | Common Tools |
|---|---|
| CRM (wealth / advisory) | Salesforce Financial Services Cloud, Redtail, Wealthbox |
| Portfolio management | Orion, Black Diamond, Tamarac |
| Compliance | ComplySci, RIA in a Box, Smarsh |
| Insurance agency management | Applied Epic, HawkSoft, QQ Catalyst |
| Document management | Laserfiche, DocuSign, ShareFile |

### Pain Points AI Addresses Well

- Advisors don't have a daily view of client exceptions — only learn of problems reactively
- Compliance deadlines are tracked in spreadsheets — missed deadlines create regulatory risk
- Client review scheduling is ad-hoc — top advisors have a system, most don't
- Insurance renewals are missed because the tracking spreadsheet is out of date

### Research Sources

- Aite-Novarica Group: financial services technology research
- Celent Research: insurance and wealth management technology benchmarks
- American Banker: financial services technology adoption
- WealthManagement.com: RIA operations and technology case studies
- LIMRA: insurance industry research and benchmarks

### Red Flags — Not Ready Yet

- Regulatory environment for this specific firm type is not clearly understood — compliance prerequisite before AI data flows
- Client data is fragmented across multiple custodians with no aggregation layer
- CRM is not consistently used — advisor notes and client data live in email and personal files
- Firm has not completed a cybersecurity assessment — financial data sensitivity requires security baseline first
- Principal or compliance officer is not engaged — this stakeholder is a hard prerequisite in regulated industries

---

## Quick Reference: Readiness Signals by Pattern

Use these cross-sector signals to quickly calibrate operator readiness regardless of vertical:

### Green Signals (Ready to Deploy)
- At least one person can answer "what's in each system" without hesitation
- Data lives in SaaS tools with documented APIs
- At least one process already has a documented SOP
- Operator checks the same dashboard or report every morning (agent can replace this)
- Someone on the team has already experimented with AI tools personally

### Yellow Signals (Proceed with Caution)
- Data quality is acknowledged as a problem but not yet fixed
- Key processes are understood by only one person
- Tooling is partially SaaS, partially legacy
- Operator is interested but has tried AI pilots that failed (understand why before proceeding)

### Red Signals (Fix First, Deploy Later)
- "We don't really track that" is the answer to multiple workflow questions
- All key knowledge is in one person's head with no backup
- Systems don't have APIs and there's no middleware
- Previous AI pilots failed without a clear understanding of why
- Security and compliance fundamentals are not in place

---

*This starter pack covers 8 verticals at a survey level. For deeper research, follow the sources listed for each sector and use the Phase 0 sector research template to document your findings systematically.*
