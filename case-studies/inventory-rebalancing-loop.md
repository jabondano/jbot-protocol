# Case Study: Twice-Weekly Inventory Rebalancing Loop

**Anonymized implementation of the JBOT Protocol — global multi-warehouse inventory planning with human-gated execution. Figures are illustrative.**

---

## Company Profile

| Attribute | Detail |
|-----------|--------|
| **Industry** | Consumer electronics / wearable technology |
| **Revenue** | ~$4-5M annually (illustrative) |
| **Footprint** | 9 ERP inventory locations across US, Canada, Europe, and Asia; ~60 active SKUs |
| **Manufacturing** | Contract factories in Asia; MOQs 1,000-2,000 units/series; ~90-day PO-to-warehouse pipeline |
| **Challenge** | Inventory decisions (inter-warehouse transfers, factory POs, freight mode) made ad hoc from stale spreadsheets; no systematic connection between stock positions and revenue goals |

## The System

A planning loop that runs twice a week and a human-gated execution path:

```
Mon+Thu cron (fleet bot, mid-tier model, ~7K tokens/run)
  collector script (deterministic) — ERP queries: inventory by location,
    28/90-day sales velocity, open POs/TOs, monthly revenue
  → model reasons over the JSON per a facts/thresholds file
  → publisher script writes snapshot + recommendations + open-PO diff to the database
  → chat-channel summary (goal attainment, new/late PO alerts, top recommendations)
        ↓
  live dashboard page (database-first — data refreshes with zero redeploys)
        ↓
  operator approves a specific recommendation ID
  → desktop agent creates the actual ERP Transfer Order / Purchase Order
  → drafts warehouse shipping instructions for the freight contacts
```

Design rules that made it safe:
- **Propose-only loop.** The cron never writes to the ERP. Execution requires a named recommendation ID and a fresh human "yes" in an interactive session.
- **Deterministic core, AI edges** (Copilot Pattern): scripts do queries and database writes; the model only classifies, reasons, and formats. Failure modes stay legible.
- **Shared recommendation queue with dedup:** interactive runs and cron runs write to one table; equivalent recommendations (same lane, overlapping SKUs, <7 days old) supersede rather than duplicate.
- **Refuse-don't-estimate:** if the collector fails, the bot reports the failure and stops. (Its very first run did exactly this when a script path was wrong — the correct behavior.)

## The Build: Audit → Research → Build → Live-Verify

The distinctive move was spending the first phase NOT building:

1. **Asset audit (multi-agent):** every existing inventory doc, script, and cron the new system would build on was reviewed by parallel agents, and every finding adversarially verified before acceptance. Result: 63 confirmed defects across 9 assets, including a velocity query that inflated demand up to **3x** (it counted unfulfilled orders AND cost-of-goods lines alongside sales) and a dead-stock classifier that flagged pre-launch products for clearance.
2. **Domain research (multi-agent):** safety-stock formulas, periodic-review policy, lead-time variability, freight economics — each numeric claim verified by an independent skeptic agent, then synthesized into a machine-readable defaults file the system's config imports.
3. **Build on corrected foundations**, port the surviving logic, encode hard rules (never touch pre-launch SKUs, licensed-brand SKUs never leave their licensed region, prescription stock ships from one location only).
4. **Live-verify against the real ERP before automating.** This caught two things the audit could not: the ERP returned PO status codes in a different format than every internal doc claimed, and six dual-record SKUs held their sellable stock on the *other* record type — a ~75% undercount for one product line under the documented rule.

## What the First Run Found (illustrative)

- H1 revenue attainment ~104% of a seasonality-derived quota — on track, no panic needed.
- The operator's intuition was "move stock toward the underperforming regions." The data said the opposite: one region held ~8 years of cover at current velocity (a demand problem, not a supply problem), while the real urgency was three fast-moving colorways at a marketplace warehouse with 16-25 days of supply.
- Two open factory POs carried placeholder ETAs (ETA = order date) and one PO was 13 months past due — invisible until the diff-based PO tracker existed.

## Lessons

1. **Audit the foundations before automating on them.** A planner consuming a 3x-inflated velocity number automates the wrong answer faster.
2. **Adversarial verification catches inconsistency, not shared false premises.** The audit agents were handed a "ground truth" rule about item record types that was itself wrong; every fix applied it faithfully. Only probing the live system exposed it. Always budget a live-verification pass after doc-driven fixes.
3. **Trigger replenishment on inventory position (on-hand + on-order + in-transit), not on-hand.** With a 90-day pipeline, two-thirds of system inventory is in motion; on-hand triggers double-order every cycle.
4. **Sentinel values poison downstream logic.** "No sales in 28 days" encoded as DoS=999 auto-classified zero-velocity SKUs — including deliberate pre-launch inventory — as dead stock. Encode absence as NULL with an explicit status.
5. **Calendar facts rot fastest.** The canonical ops doc carried a lunar-new-year date that was a full year stale; order-by deadlines derived from it would have been fiction. Recompute event calendars each run; never trust a doc's "next year" claims.
6. **Model economy is governance, not thrift.** The build's early multi-agent fan-outs inherited the session's frontier model and burned ~9M tokens; the same work verified fine on mid-tier models. Rule now standing: every spawned agent gets an explicit model — frontier for the main loop and final synthesis only. (See Pillar 4 — Cost Discipline.)
7. **Two planners, one queue.** The moment both the interactive skill and the cron existed, they proposed the same transfer within hours. Any multi-entry-point planner needs dedup/supersede semantics from day one.

## Results (first day, illustrative)

- One planning cadence (Mon+Thu) replacing ad hoc spreadsheet sessions; ~7K tokens per cron run.
- 11 recommendations in the queue on day one: 2 transfers, 1 expedite, 5 PO-hygiene alerts, 1 demand-activation flag, 2 superseded duplicates.
- 9 legacy assets corrected as a by-product — the audit's fixes outlive the system that motivated them.
