# Protocol Maintenance: Keeping This Repo Current with Live Operations

**Audience:** the protocol author and any agent assisting with repo maintenance.
**Problem this solves:** the protocol is extracted from live operations, and live operations move faster than documentation. Twice in 2026 the repo drifted badly — an 11-commit local/remote divergence, and an entire v1.0 systems layer stranded on an orphaned branch for two months.

---

## The Harvest Loop (monthly)

On the first week of each month (pairs naturally with the monthly board-metrics rhythm), run a one-hour harvest:

1. **Collect candidates.** Review the past month's operational work — project memory, published blog posts, fleet changes, incidents. Anything that produced a *generalizable* lesson is a candidate.
2. **Classify each candidate:**
   - **Lesson** → a section or table row in an existing pillar (failure modes, governance rules, checklists)
   - **Pattern** → a new protocol under `protocols/` (only if it has been used at least twice)
   - **System** → a packaged implementation under `systems/` (only if productized with intake + schema)
   - **Correction** → existing doc says something now false (fleet roster, pricing, tool capabilities)
   - **Not protocol material** → company-specific, confidential, or one-off; skip
3. **Apply the sanitization gate** (below) to everything that passes.
4. **Ship as a patch release** (`v2.x.y`) with a CHANGELOG entry. Small monthly patches beat quarterly mega-releases — review stays cheap and drift never accumulates.

**Blog-post rule:** anything published on the author's public blog is pre-sanitized by definition and should be absorbed into the protocol within 30 days — the blog is the narrative version, the protocol is the how-to version. A blog post with no corresponding protocol section is an open harvest item.

## The Sanitization Gate (every commit)

This repo is publicly linked. The author is the COO of a NASDAQ-listed company. Every commit must pass:

- **Financials:** only figures from public press releases or SEC filings. No internal targets, segment margins, channel mix, pipeline values, or revenue-loss estimates — *even approximately, even as "illustrative" if attributed to the named company*. Anonymized + explicitly-illustrative framing is the standard for deployment examples.
- **Identifiers:** no supplier/partner/staff names, customer data, internal hostnames, IPs, ports, chat IDs, CRM object IDs, bucket names, or tokens. Generic role labels ("Supplier A", "the creative director") only.
- **Licensed brands:** never name third-party licensed brands in deployment content.
- **Results:** never attribute unverifiable or projected results to the named company. If the numbers aren't audited measurements, the example must be anonymized AND labeled illustrative.
- **Verification:** before pushing, grep the changed files for the company name, ticker, exec names, and known partner names. Zero hits outside author-byline lines.

## Branch & Sync Hygiene

The two failure modes that caused real drift, and their rules:

1. **The repo is edited from multiple surfaces** (local Claude Code, claude.ai web sessions). Rule: **always `git fetch && git status -sb` before any local edit**, and rebase before pushing. Web sessions must merge their PRs promptly — an unmerged feature branch older than 2 weeks is an open item in the next harvest.
2. **`main` is the only long-lived branch.** The orphaned-`master` incident: a parallel v1.0 was built and pushed to a `master` branch with no common ancestor, then forgotten for two months. Rule: any push to a branch other than `main` gets either a PR the same week or an explicit note in this file's Open Items. Delete merged branches immediately.

## Version Discipline

- `v2.x` minor bumps: new pillars, protocols, or systems.
- `v2.x.y` patches: lessons, corrections, template updates.
- **Every push to main updates CHANGELOG.md in the same commit.** The March–April 2026 gap (9 substantial features, zero changelog entries) made the repo's own history unusable for review.
- The README version badge tracks the changelog's top entry.

## Standing Freshness Checks (each harvest)

| Check | Why |
|-------|-----|
| Model names + pricing in `implementation/model-selection-guide.md` | Model generations turn over ~quarterly; stale pricing in a consulting doc is credibility damage |
| Fleet roster in README + `implementation/openclaw-fleet.md` | Bots get added/retired; the roster is the first thing a reader checks against reality |
| Forward-looking dates anywhere (`grep -rn "2026\|planned\|upcoming" --include="*.md"`) | Roadmaps with passed dates read as abandonment |
| GAP-ANALYSIS.md roadmap table | Mark shipped items; it's the public to-do list |
| External links (blog posts, GitHub repos) | Link rot in the first 10 minutes of reading kills trust |

---

*Created 2026-06-12 as part of v2.2, after recovering the orphaned systems layer.*
