# Program-in-a-Repo

*A JBOT Protocol method chapter. Sanitized pattern distilled from a live hardware-platform program run by a two-executive team with AI agents as program staff. No client-confidential content.*

## The problem it solves

A serious product program generates strategy docs, specs, vendor packages, decisions, decks, and status — faster than any small team can file, reconcile, and keep honest. The default failure isn't missing documents; it's *disagreeing* documents: the deck says October, the plan says December, the vendor package names something it shouldn't, and nobody notices until a counterpart does.

Program-in-a-Repo treats the program itself as a software artifact: one repo as the canonical home, one live dashboard as the projection, one state store as the truth about status, and AI agents on a loop as the staff that keeps all three aligned.

## The six layers

1. **The repo, tiered by disclosure.** Every document lives in a directory that encodes who may ever see it (internal crown jewels → trusted team → NDA partners → factory/codename-only). The cardinal rule: content moves DOWN in sensitivity only by deliberate rewrite, never copy. Counterparts get projections, never the source.
2. **Frontmatter → everything.** Each doc carries a small metadata block (tier, type, state, hub placement, render eligibility, privileged flag). One script walks the tree and generates the manifest, the dashboard library, the master index tables, and the web renders. Filing a document = writing it. There is no second step, so there is no forgotten step. Privileged docs are hard-blocked from rendering *in code*, not by convention.
3. **The hub.** An access-gated dashboard projects the repo for humans: story-first, grouped by reader intent (orient → decide → build → operate), a decisions board with owners, gate countdowns, and a "changed this week" strip. No card walls; the hub answers questions in the order a returning executive asks them.
4. **State lives in a database, not in prose.** Gates, tasks, budgets, and vendor status live in structured tables with an approval inbox. Documents cite state; they never store it. This is what lets agents check "does the doc match reality" mechanically.
5. **Gates and Gantt.** Borrowed from hardware practice (EVT/DVT/PVT): every phase has written exit criteria and a kill-switch budget, and the schedule works backward from an immovable public date. Agents enforce gates ("no spend until the checklist is signed"); humans decide at them.
6. **The agent loop.** A weekly warden agent files new docs, regenerates projections, checks contradictions (numbers, dates, claims) against the canonical sources, flags stale status against the database, runs a leak check on externally-bound tiers, and proposes exactly three improvements. Ambiguities go to the human approval inbox — agents propose, humans approve. Heavier multi-agent audits run on demand before anything ships externally.

## The operating principles that make it work

- **DRI everywhere; agents are staff.** Every deliverable has one named human owner. Agents coordinate, verify, chase, and file — they never own outcomes and never make trade-off decisions.
- **The gate judges the diff, not the author.** Humans and agents contribute through the same CI + code-owner + branch-protection boundary. AI involvement is disclosed in commit metadata; every agent commit links its session for auditability.
- **Vendored trust for external firms.** Contract deliverables land in isolated, code-owner-fenced intake dirs; acceptance is an automated conformance suite, not a trust-based review; external partners work in physically separate slim repos when platform access can't be scoped.
- **Claims discipline as code.** Anything externally falsifiable (maturity claims, superlatives, partner names) is governed by a written banned-phrases list, enforced by the warden's sweep — because the cheapest diligence a counterpart runs is checking whether you exaggerate.
- **Demo culture.** Weekly review looks at artifacts (a running simulator, a rendered page, a signed doc), never status slides. If it can't be shown, it isn't done.

## Minimum viable adoption (a client can start Monday)

Day 1: create the repo with tier directories + a 20-line agent-onboarding file (AGENTS.md). Day 2: add frontmatter to the ten documents that matter; write the one generator script. Day 3: stand up the hub page reading the generated manifest. Week 2: move status into a structured store with an approval inbox. Week 3: schedule the warden. Total new software: ~one script, one dashboard page, one skill file. The leverage is not the tooling — it's that filing, projection, and verification stop being human memory tasks.

## What to measure

Time-to-file a new document (target: zero — it's the frontmatter). Contradictions caught per week (should spike, then decay). External-claim incidents (target: zero — the sweep catches them first). Executive prep time before partner meetings (the hub + index should make it minutes).

---
*JBOT Protocol · Agentic Ops framework · v1.0 · July 2026*
