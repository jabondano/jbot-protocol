# Sales Pipeline Engine — Business Intake Sheet
*Version 1.0 | Draft | JBOT Protocol*

---

## Purpose

This intake captures the business context needed to configure the Sales Pipeline Engine for your company. Your answers will determine:

- CRM integration approach
- Pipeline stage definitions
- Outreach cadence
- Follow-up automation
- Deal scoring logic
- Reporting structure

**Time to complete:** 20-30 minutes  
**Required for deployment:** Sections 1-4  
**Optional for optimization:** Sections 5-7

---

## Section 1: Sales System & CRM

### 1.1 What CRM/sales system do you currently use?
**What this configures:** Integration method, data sync approach, API vs manual workflows

**Options:**
- [ ] **HubSpot** (Full CRM with API)
- [ ] **Salesforce** (Enterprise CRM with API)
- [ ] **Zoho CRM** (Mid-market CRM with API)
- [ ] **Pipedrive** (Sales-focused CRM with API)
- [ ] **Close** (Sales CRM with API)
- [ ] **Copper** (Google Workspace CRM with API)
- [ ] **Airtable** (Custom CRM/database with API)
- [ ] **Google Sheets** (Spreadsheet-based tracking, no native CRM)
- [ ] **Excel/Spreadsheets** (Manual tracking, no automation)
- [ ] **Nothing** (No system, tracking in email/notes/head)
- [ ] **Other:** ________________ (specify)

**Example (Lucyd):** HubSpot (Full CRM, API integrated, 21 daily cron jobs)

---

### 1.2 If using a CRM, what's your access level?
**What this configures:** What automations are possible, API rate limits, permission scope

**Options:**
- [ ] **Admin** (full API access, can create custom fields/workflows)
- [ ] **Power User** (API access, limited custom field creation)
- [ ] **Standard User** (read-only API, no customization)
- [ ] **View Only** (no API, manual exports only)
- [ ] **N/A** (no CRM)

**If Admin/Power User:**
- API credentials available? [ ] Yes [ ] No [ ] Need to request
- API rate limits known? [ ] Yes [ ] No
- Custom fields allowed? [ ] Yes [ ] No [ ] Limited

**Example (Lucyd):** Admin, API credentials configured, unlimited custom fields

---

### 1.3 If NO CRM, what's your current tracking method?
**What this configures:** Migration path, data import approach, training needs

**Options:**
- [ ] Google Sheets (with some structure)
- [ ] Excel (local files)
- [ ] Email threads (searching inbox)
- [ ] Notes app (Apple Notes, Notion, etc.)
- [ ] Memory (nothing written down)
- [ ] Mix of the above

**Willingness to adopt a CRM:**
- [ ] **Yes, ready now** (will set up HubSpot/Salesforce/etc.)
- [ ] **Yes, but need help choosing** (open to recommendations)
- [ ] **Maybe later** (want to start with spreadsheets, migrate later)
- [ ] **No** (prefer manual, just want automation on top of current method)

**Example:** If "Google Sheets" → Can we build Airtable-style automation on top? If "Nothing" → Start with HubSpot free tier?

---

## Section 2: Sales Process & Pipeline

### 2.1 What's your sales motion?
**What this configures:** Lead flow, outreach cadence, sales cycle assumptions

**Options:**
- [ ] **Outbound** (cold outreach, SDR-driven)
- [ ] **Inbound** (marketing-driven leads, forms, content)
- [ ] **Hybrid** (mix of outbound + inbound)
- [ ] **Partner/Channel** (referrals, partnerships)
- [ ] **Self-Serve** (product-led, low-touch)

**Primary customer type:**
- [ ] **B2B** (selling to businesses)
- [ ] **B2C** (selling to consumers)
- [ ] **Mix**

**Average deal size:** $______

**Average sales cycle:** [ ] <1 week [ ] 1-4 weeks [ ] 1-3 months [ ] 3-6 months [ ] 6+ months

**Example (Lucyd):** Hybrid (inbound DTC + outbound wholesale/B2B), B2B for wholesale, avg deal $2K-50K, 2-8 week cycle

---

### 2.2 What are your pipeline stages?
**What this configures:** Stage-based automation, deal progression tracking, conversion rate analysis

**Standard stages (check all that apply, or write custom):**
- [ ] Lead (unqualified contact)
- [ ] Qualified Lead (MQL/SQL)
- [ ] Contact Made (first touch completed)
- [ ] Meeting Scheduled
- [ ] Demo/Presentation Completed
- [ ] Proposal Sent
- [ ] Negotiation
- [ ] Closed Won
- [ ] Closed Lost

**Custom stages (if different):**
```
1. ________________
2. ________________
3. ________________
4. ________________
5. ________________
```

**Example (Lucyd Wholesale):** Lead → Qualified → Sample Sent → Demo Call → PO Received → Closed Won

---

### 2.3 Who manages the sales pipeline?
**What this configures:** Notification routing, approval gates, reporting recipients

**Primary sales owner:**
- Name: ________________
- Role: ________________
- Preferred notification channel: [ ] Email [ ] Slack [ ] Telegram [ ] Discord [ ] Other: ______

**Team size:**
- [ ] Solo (founder/owner only)
- [ ] 2-3 people
- [ ] 4-10 people (small team)
- [ ] 10+ people (sales org)

**If team >1, who gets what notifications?**
- Daily pipeline summary → ________________
- New leads → ________________
- Stalled deals → ________________
- Closed won → ________________

**Example (Lucyd):** Joaquin (COO), Telegram notifications, team of 3 (COO + 2 sales reps)

---

## Section 3: Lead Sources & Capture

### 3.1 Where do your leads come from?
**What this configures:** Lead capture automation, source tagging, attribution tracking

**Select all:**
- [ ] Website forms (Contact Us, Demo Request, etc.)
- [ ] Inbound email (sales@, info@, etc.)
- [ ] Cold outreach (manual prospecting)
- [ ] LinkedIn (InMail, connection requests)
- [ ] Referrals
- [ ] Events/Trade shows
- [ ] Paid ads (Google, Meta, LinkedIn)
- [ ] Partnerships/Channel
- [ ] Other: ________________

**Highest volume source:** ________________

**Example (Lucyd B2B):** Website forms (40%), inbound email (30%), LinkedIn (20%), referrals (10%)

---

### 3.2 How do leads currently enter your system?
**What this configures:** Automation entry points, manual touchpoint reduction

**Options:**
- [ ] **Automatically** (form → CRM via Zapier/webhook)
- [ ] **Semi-automatically** (form notification → manual CRM entry)
- [ ] **Manually** (copy-paste from email/LinkedIn → CRM)
- [ ] **Inconsistently** (sometimes entered, sometimes forgotten)

**Pain points with current process:**
```
[What's broken? What takes too long? What gets missed?]
```

**Example (Lucyd):** HubSpot forms auto-create deals, but LinkedIn leads require manual entry (pain point)

---

## Section 4: Outreach & Follow-Up

### 4.1 What's your outreach cadence?
**What this configures:** Follow-up automation, reminder triggers, stale deal alerts

**First touch → follow-up timeline:**
- First follow-up after: [ ] 24h [ ] 2-3 days [ ] 1 week [ ] Other: ______
- Second follow-up after: [ ] 3 days [ ] 1 week [ ] 2 weeks [ ] Other: ______
- Third follow-up after: [ ] 1 week [ ] 2 weeks [ ] 1 month [ ] Other: ______

**When do you mark a lead "cold"?**
- [ ] After X days of no response: ______ days
- [ ] After X follow-up attempts: ______ attempts
- [ ] Never (keep in pipeline indefinitely)

**Example (Lucyd):** 3-day, 1-week, 2-week cadence; cold after 30 days + 3 unanswered follow-ups

---

### 4.2 Current follow-up method?
**What this configures:** Automation opportunities, template generation, manual work reduction

**Options:**
- [ ] **Manual** (write every email from scratch)
- [ ] **Templates** (copy-paste from saved templates)
- [ ] **CRM sequences** (automated email sequences)
- [ ] **Mix** (automated for early stage, manual for late stage)

**Do you track follow-up completion?**
- [ ] Yes (in CRM tasks)
- [ ] Somewhat (mental note, not systematic)
- [ ] No (reactive only)

**Pain points:**
```
[What's hard about follow-ups? What gets missed?]
```

**Example (Lucyd):** Templates in HubSpot, but reminders are manual → deals go stale if COO doesn't check daily

---

## Section 5: Deal Scoring & Prioritization

### 5.1 How do you prioritize deals?
**What this configures:** Deal scoring algorithm, alert thresholds, focus recommendations

**Current method:**
- [ ] **Gut feel** (no formal scoring)
- [ ] **Deal size** (biggest deals first)
- [ ] **Close date** (soonest closing first)
- [ ] **Engagement** (most responsive first)
- [ ] **Stage** (furthest along first)
- [ ] **Combo** (mix of the above)

**Would you want automated deal scoring?**
- [ ] Yes (score based on size, engagement, stage)
- [ ] Yes (but only for flagging high-priority deals)
- [ ] No (prefer manual judgment)

**Example (Lucyd):** Combo of deal size + stage + engagement; want automated flagging for "hot deals"

---

### 5.2 What makes a deal "high priority"?
**What this configures:** Alert triggers, priority scoring weights

**Select all that apply:**
- [ ] Deal size >$X: $______
- [ ] Decision maker engaged (not just gatekeeper)
- [ ] Proposal sent
- [ ] Meeting scheduled within 1 week
- [ ] Referral from key partner
- [ ] Competitor mention (considering alternatives)
- [ ] Budget confirmed
- [ ] Timeline urgent (<30 days to close)

**Example (Lucyd):** Deal >$10K + proposal sent + meeting scheduled = high priority → daily reminder

---

## Section 6: Reporting & Visibility

### 6.1 What sales metrics do you track?
**What this configures:** Daily/weekly report structure, dashboard KPIs

**Select all:**
- [ ] Pipeline value (total $ in pipeline)
- [ ] New leads this week/month
- [ ] Conversion rate by stage
- [ ] Average deal size
- [ ] Average time in each stage
- [ ] Win rate
- [ ] Revenue closed this week/month
- [ ] Activities (calls, emails, meetings)
- [ ] Other: ________________

**Example (Lucyd):** Pipeline value, new leads, conversion rate, win rate, revenue closed

---

### 6.2 How often do you want sales updates?
**What this configures:** Report frequency, delivery channel, summary vs detail level

**Frequency:**
- [ ] Daily (morning brief)
- [ ] Weekly (Monday morning)
- [ ] Bi-weekly
- [ ] Monthly
- [ ] On-demand only

**Delivery method:**
- [ ] Telegram
- [ ] Discord
- [ ] Slack
- [ ] Email
- [ ] Dashboard (no push notifications)

**Level of detail:**
- [ ] **Summary** (top-line metrics only, 2-3 min read)
- [ ] **Detailed** (all deals, stage changes, full pipeline, 10-15 min read)
- [ ] **Custom** (summary daily, detailed weekly)

**Example (Lucyd):** Daily summary to Telegram (5-min read), weekly detailed to Discord

---

## Section 7: Integrations & Technical

### 7.1 What other tools connect to your sales process?
**What this configures:** Cross-system automation, data sync, attribution tracking

**Select all:**
- [ ] Email platform (Gmail, Outlook, etc.)
- [ ] Calendar (Google Calendar, Outlook, etc.)
- [ ] Marketing automation (HubSpot Marketing, Mailchimp, Klaviyo, etc.)
- [ ] Accounting/Billing (QuickBooks, Xero, Stripe, etc.)
- [ ] Project management (Asana, Monday, ClickUp, etc.)
- [ ] Communication (Slack, Teams, etc.)
- [ ] E-commerce (Shopify, WooCommerce, etc.)
- [ ] Other: ________________

**Example (Lucyd):** HubSpot (CRM + Marketing), Gmail, Slack, NetSuite (accounting), Airtable (projects)

---

### 7.2 OpenClaw deployed?
**What this configures:** Deployment method, skill installation approach

- [ ] Yes — active OpenClaw deployment
- [ ] Yes — OpenClaw installed but not using bots yet
- [ ] No — new to OpenClaw (will install)
- [ ] No — using different automation (specify: ________________)

**If yes:**
- Existing bots running? [ ] Yes [ ] No
- Comfortable with cron jobs? [ ] Yes [ ] No

**Example (Lucyd):** Yes, 10-bot fleet, salesbot has 21 crons running daily

---

## Review & Next Steps

### Configuration Summary

After completing this intake, you'll receive:

1. **CRM integration plan** — API setup or manual workflow (based on your system)
2. **Pipeline stage mapping** — Your stages → automation triggers
3. **Follow-up cadences** — Automated reminders, stale deal alerts
4. **Deal scoring model** — Priority algorithm based on your criteria
5. **Daily/weekly reports** — Structured sales visibility
6. **Outreach templates** — Email/LinkedIn templates for your process

### Estimated Setup Timeline

**If CRM with API (HubSpot, Salesforce, etc.):**
- **Week 1:** API setup, data sync, pipeline mapping
- **Week 2:** Follow-up automation, deal scoring, reporting
- **Week 3:** Pilot with 10-20 active deals, tune scoring
- **Week 4:** Full system live

**If no CRM / Google Sheets:**
- **Week 1:** Set up Airtable or HubSpot Free, import existing data
- **Week 2:** Define pipeline stages, build automation
- **Week 3:** Team training, workflow adoption
- **Week 4:** Pilot with new leads, iterate

**If enterprise CRM (Salesforce):**
- **Week 1-2:** API permissions, field mapping, data model design
- **Week 3:** Build automation, test with sandbox
- **Week 4-5:** Deploy to production, tune

---

**Completed by:** ________________  
**Date:** ________________  
**Company:** ________________  
**Contact:** ________________
