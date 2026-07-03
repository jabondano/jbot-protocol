# Changelog

All notable changes to the JBOT Protocol will be documented in this file.

---

## [v2.2.1] — 2026-07-03

### Added
- **Case study: Twice-Weekly Inventory Rebalancing Loop** (`case-studies/inventory-rebalancing-loop.md`) — anonymized global multi-warehouse planning loop: propose-only cron + human-gated ERP execution, audit-before-build method, live-verification lessons (adversarial verification can't catch shared false premises; sentinel values poison classifiers; trigger on inventory position not on-hand; model economy as governance).

---

## [v2.2] — 2026-06-12

### Added
- **Systems layer** (`systems/`) — 4 pre-built configurable systems rescued from an orphaned branch, sanitized and anonymized: Inventory Watchdog, Sales Pipeline Engine, Static Content Engine, Marketing Performance Monitor. Each ships intake questionnaires, config schemas, example configs, and an anonymized deployment example.
- **Copilot Pattern protocol** (`protocols/copilot-pattern/`) — ad hoc single-purpose internal tools: deterministic-core/AI-edges architecture, owner + manual + loop operating model, skills-as-products packaging, graduation path.
- **Pillar 4 — Kill Hierarchy** — three-level kill procedures (isolate bot / freeze fleet / full stop) with rehearsal and authority rules. Closes the March gap analysis P1.
- **Pillar 4 — Decommissioning & Succession** — planned-retirement checklist from the executive-hub retirement: every signal needs a consumer that survives its producer; every layer needs a survivor plan.
- **Pillar 4 — Cost Discipline as Governance** — per-job model + effort assignments as policy, token spend as fleet-health KPI. Closes the March gap analysis P1.
- **Pillar 4 — Publication Trust Zones** — internal/public zones, declared visibility, CODEOWNERS on public paths, "when unsure → internal".
- **Pillar 3 — The Native-First Check** — pre-build platform check + deletion discipline (the cancelled-build case study).
- **Pillar 3 — Empty Mapping Table failure mode** — the silent EDI cross-reference failure class; monitor the fallback path's volume.
- **docs/MAINTENANCE.md** — how the protocol stays current with live operations (harvest loop, branch hygiene, sanitization gate).

### Changed
- **Model Selection Guide** — current June 2026 lineup and pricing (Haiku 4.5 $1/$5, Sonnet 4.6 $3/$15, Opus 4.8 $5/$25); new Tier 4 Frontier (Claude Fable 5, $10/$50) with the no-frontier-crons governance rule; effort parameter as a within-tier cost dial; recomputed budget tables.
- **Fleet roster** (README + OpenClaw guide) — executive hub bot retired June 2026; flat specialist topology now recommended; hub-and-spoke sections marked as a documented evolutionary stage.
- **Discovery interview v2 merged** — voice-first conversational flow with Master Context File output; tool-neutral header.
- **README** — added Systems & Protocols section, full repo-structure refresh covering `protocols/`, `systems/`, `docs/`, framework 00/07/08.

### Fixed
- Case studies from the orphaned branch carried real-company attribution with non-public targets and unverifiable projected results — rewritten as anonymized, explicitly illustrative deployment examples; leaked identifiers (chat IDs, VPS IP, CRM object IDs, supplier/staff names) scrubbed.
- JSON syntax error in static-content-engine config schema.

### Unlogged March–April work (recorded retroactively)
- Phase 0 Research Methodology (`framework/00`), Pillar 7 Agent Identity (`framework/07`), Evolution Layer (`framework/08`), Lattice + Discord architecture docs, Discord deployment guide with 10 live-deployment lessons, ICP Intelligence Engine protocol, pricing framework, ad creative workflow + copy brief template, sector research starter.

---

## [v2.1.2] — 2026-02-18

### Added
- **Family Operations Bot Case Study** (`case-studies/family-operations-bot.md`) — personal-scale JBOT Protocol implementation: father + 12-year-old son building a family AI operating system on OpenClaw with soul-first design, layered memory architecture, and Telegram interface

---

## [v2.1.1] — 2026-02-18

### Added
- **Executive Operations Fleet Case Study** (`case-studies/executive-operations-fleet.md`) — autonomous 4-bot fleet replacing manual executive information assembly across 7 divisions, with hub-and-spoke architecture, fleet health scoring, and "silent if nothing" principle

---

## [v2.1] — 2026-02-18

### Added
- **Industry Templates**: 3 vertical-specific blueprints (`industry-templates/`)
  - Real Estate Brokerage — speed-to-lead, transaction coordination, listing intelligence
  - Import/Export Distribution — container tracking, field staff coordination, time zone management
  - Manufacturing Operations — cross-layer data synthesis, factory monitoring, project lifecycle
- **B2B Lead Gen Case Study** (`case-studies/b2b-lead-gen-system.md`) — ICP scoring, 8-touch outreach, pilot framework
- **Multi-Bot Role Separation** pattern in Division Architecture (Pillar 1) — model-function pairing, SOUL.md identity, spin-off thresholds
- **Integration Failure Modes** section in Tool Integration (Pillar 3) — MCP limitations, custom fallback patterns, credential architecture
- **Fleet Watchdog Pattern** in Governance (Pillar 4) — zero-token monitoring, data freshness pre-checks, retention policies
- **Migration as Change Management** in Change Management (Pillar 5) — 4-phase VPS migration, "Silent If Nothing" principle
- **Fleet Health Score** in Measurement & ROI (Pillar 6) — weighted 5-factor formula, data pipeline architecture, layered retention

### Changed
- Updated CLAUDE.md: "Five Pillars" → "Six Pillars"
- Updated Getting Started guide: "Five Pillars" → "Six Pillars"
- Updated AGENT-SKILL.md template: YAML frontmatter, `token_budget`, `silent_if_nothing` fields, optional SQL/curl template sections
- Updated README: added `industry-templates/` and new case study to repo structure

---

## [v2.0] — 2026-02-07

### Added
- **Sixth Pillar**: Measurement & ROI (`framework/06-measurement-roi.md`)
- **Claude Code Implementation Guide** (`implementation/claude-code-guide.md`)
- **Model Selection Guide** (`implementation/model-selection-guide.md`)
- **Anonymized Case Study** (`case-studies/anonymized-implementation.md`)
- **Getting Started Guide** (`guides/getting-started.md`)
- **Agent Skill Template** (`templates/AGENT-SKILL.md`)
- **CHANGELOG.md** (this file)
- **CONTRIBUTING.md** with contribution guidelines
- 12 Mermaid diagrams across all framework and implementation docs
- GitHub issue templates (`.github/ISSUE_TEMPLATE/`)

### Changed
- Filled all 16 TODOs across the five framework pillars
- Updated OpenClaw guide with ClawHub, multi-agent routing, Opus 4.6 Agent Teams
- Enhanced README with table of contents, quick start, Five Pillars diagram
- Updated LICENSE copyright to 2025-2026
- Rebranded local knowledge base from "Agentic Ops" to "JBOT Protocol"

### Fixed
- Corrected research file path in README (`synthesis.md` → `enterprise-agentic-ai-guide.md`)

---

## [v1.2] — 2026-02-07

### Changed
- Rebranded from "Agentic Operations Framework" to "JBOT Protocol" due to trademark considerations
- Updated all framework references and terminology

---

## [v1.1] — 2026-01

### Added
- OpenClaw Fleet Implementation Guide (`implementation/openclaw-fleet.md`)
- Two-server topology architecture
- Agent scheduling and cross-agent communication patterns
- Twilio IVR phone system integration

---

## [v1.0] — 2025-12

### Added
- Initial release of the Five Pillars framework
- Division Architecture, Knowledge Capture, Tool Integration, Governance, Change Management
- Five reusable templates: DIVISION, PROCESSES, SYSTEMS, CUSTOM-INSTRUCTIONS, discovery-interview
- Enterprise Agentic AI research synthesis
- Lead magnet playbook outline
