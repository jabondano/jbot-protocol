# The Copilot Pattern

**Single-purpose internal tools, built ad hoc, governed proportionally**

Part of the JBOT Protocol — Protocols layer.

---

## What This Is

The bot fleet (Pillars 1–6) handles an organization's *recurring* operational cadence. The Copilot Pattern covers everything underneath: the long tail of small operational problems that were never worth solving at pre-AI build costs — the vendor invoices nobody reconciles, the customer feedback nobody structures, the weekly report someone assembles by hand from four systems.

A **copilot** is an internal tool that is:

1. **Single-purpose.** It does one job — audit these invoices, interview these customers, process this return. It is not a platform. Platforms are how internal tools used to justify their cost; copilots don't need to.
2. **Built next to the problem.** The person who owns the problem is in the room when it's built — the spec is the actual mess on their desk, not a requirements document three translations away from it.
3. **Deterministic at the core, AI at the edges.** Code does the volume work; models handle only exceptions, judgment, conversation, and narrative. This is what makes copilots cheap to *run*, not just cheap to build.
4. **Disposable by design.** If the process changes, rebuild it in a day. Sunk-cost gravity never accumulates.

## The Economics (Why This Is Now a Category)

| | The old paths (backlog / SaaS / outside firm) | The copilot |
|---|---|---|
| Time to first value | Months | Days, sometimes hours |
| Upfront cost | Dev sprints, license fees, or a consulting quote | An afternoon of builder time |
| Recurring cost | Per-seat pricing, retainers, maintenance contracts | Pennies of model spend |
| Fit | 60% of what you need, 300% of what you'll use | Exactly the problem |
| Ownership | The vendor's roadmap | Yours — including the data |

The unserved backlog of small problems is the largest pool of untapped ROI inside most companies, precisely because every individual item was too small to justify the old acquisition paths. The constraint has moved from engineering capacity to **knowing your operations well enough to name the problem precisely.**

## Before Building: Two Gates

1. **The Native-First Check.** Confirm the platform you already pay for doesn't ship this natively — see [Pillar 3 → The Native-First Check](../../framework/03-tool-integration.md). Copilots live in the connective tissue *between* systems of record, never in replicating one.
2. **The Owner Test.** Name the person whose problem this solves and who will work its output. If nobody will own the output, the tool shouldn't exist — an unowned copilot is future clutter, not future leverage.

## Architecture: Deterministic Core, AI Edges

The design rule that drives both cost and trustworthiness: **deterministic tools do the volume; language models handle only the residual.**

A representative document-processing copilot (invoice audit) decomposes like this:

| Stage | Tool | Marginal cost |
|-------|------|---------------|
| Extraction | Native parsers, no re-rendering | $0 |
| Parsing | Code + document geometry | $0 |
| Validation | The document's own arithmetic (rows × prices = totals) | $0 |
| Repair of failures | Small model agents, one per flagged item, forced schema | Cheap, bounded |
| Analytics | SQL/code — never a model | $0 |
| Narrative | One model call per report, fed pre-computed numbers only | Trivial |

Three sub-rules:

- **Exploit self-checking structure before buying intelligence.** Business documents and records usually carry their own validation (arithmetic, totals, paired line items). Every deterministic check you find is a fleet of model calls you never pay for.
- **Force transcription, never inference, at the AI edges.** A repair agent's instruction is *read exactly what is printed; never infer*. Inferred data poisons an audit; flagged-and-missing data just narrows it honestly.
- **Models narrate; code calculates.** No number in a copilot's output should originate inside a model.

## The Operating Model: Ad Hoc Without Chaos

The obvious objection to ad hoc tools is fifty orphaned scripts. The answer is governance *proportional to the artifact* — four rules, no committee:

1. **Every copilot has an owner** — the person whose problem it solves, not the builder.
2. **Every copilot has a one-page operating manual** — what it does, how to run it, what to do when it breaks. Written the day it ships, while the builder is honest.
3. **Every recurring copilot joins a loop.** A slot in the fleet's cadence: a reminder fires, a human drops a file, the pipeline runs, a dashboard refreshes. A one-off run decays; a loop compounds. (The reminder belongs to a fleet bot; the copilot itself stays dumb.)
4. **Deterministic core, AI edges — always.** This is the cost rule and the trust rule at once: every number traces to a check or a no-inference read, and whatever can't be validated is labeled, not hidden.

Notice what's *not* here: a platform team, a tooling budget line, an intake process. Reintroduce the old overhead and you reintroduce the old math that kept these problems unsolved.

## Packaging: Skills as Products

The natural packaging for a copilot in this protocol is an **agent skill** — a versioned instruction file ([AGENT-SKILL template](../../templates/AGENT-SKILL.md)) that names the inputs, the command, the validation steps, and the escalation rules. Packaging matters because it converts a builder's afternoon into an organizational asset:

- Anyone (human or fleet bot) can run it without the builder present.
- The monthly loop invokes the skill, not the person.
- A second instance (new vendor, new product line, new entity) is a config change, not a rebuild — design the first schema with an empty slot for the second tenant.

## The Graduation Path

Most copilots stay small, and should. But because you own the code and the data, roughly one in ten compounds into something more: a voice-of-customer copilot of ours — an AI interviewer that conducts adaptive conversations with customers and synthesizes themes — started as an ad hoc build and graduated into a platform we iterate on weekly, the kind of capability that would otherwise be a five-figure annual vendor line.

Signals that a copilot is graduating: a second team asks for it; its output starts appearing in leadership reviews; you're tuning it weekly rather than monthly. At that point give it real infrastructure (repo, deploy pipeline, monitoring) — graduation is the *planned* exception to disposability.

## Worked Example (Anonymized)

A manufacturing-services vendor billed against individual e-commerce orders via scanned PDF invoices — five months, ~1,200 pages, no text layer. Under old math this audit never happens. As a copilot: one afternoon to build, a few dollars of model spend. Deterministic OCR + arithmetic validation handled ~92% of rows; ~100 flagged items went to small vision agents under no-inference instructions. The audit surfaced a double-documentation pattern (a standing double-payment risk) and systematic full-price rebilling of vendor-fault rework — roughly 4% of spend — now a tracked claims process with a named owner. The monthly re-run is one dropped PDF, one command, fifteen unattended minutes.

The full architecture write-up is public: [The Ad Hoc Copilot](https://jabondano.co/notes/ad-hoc-copilots.html) and [the deployment example in this repo](../../systems/inventory-watchdog/docs/DEPLOYMENT-EXAMPLE.md) for an adjacent system.

## Build Checklist

- [ ] Native-first check passed (the platform doesn't already do this)
- [ ] Output owner named — and it isn't the builder
- [ ] Deterministic core identified; AI confined to exceptions/narrative
- [ ] Validation uses the data's own structure where possible
- [ ] AI edges run under transcription-not-inference instructions; unvalidated residual is labeled
- [ ] One-page operating manual written the day it ships
- [ ] Recurring? → slot in the fleet loop (reminder bot + run command + refresh target)
- [ ] Second-tenant slot designed into the schema
- [ ] Disposal note: what would make this obsolete, and who deletes it

---

*Part of the [JBOT Protocol](../../README.md). Related: [Pillar 3 — Tool Integration](../../framework/03-tool-integration.md), [Pillar 4 — Governance](../../framework/04-governance.md), [AGENT-SKILL template](../../templates/AGENT-SKILL.md).*
