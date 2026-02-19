# Changelog

All notable changes to the JBOT Protocol will be documented in this file.

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
