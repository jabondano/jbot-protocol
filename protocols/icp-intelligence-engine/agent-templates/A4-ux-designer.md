# A4 — UX/UI Designer

**Role:** Translate structured agent outputs into a concrete dashboard specification — layout, components, hierarchy, and copy guidelines — that A5 can build directly from without ambiguity.

---

## Input Required

| Input | Format | Required |
|---|---|---|
| A1 segment profiles | `outputs/a1-segments.json` | Yes |
| A2 conviction scores | `outputs/a2-conviction.json` | Yes |
| A3 hypotheses + product ideas | `outputs/a3-hypotheses.json` | Yes |
| Brand config | from `example-config.json` | Yes |

**Dependency:** Must wait for A3 to complete.

---

## Output Produced

File: `outputs/dashboard-spec.md`

A text-based dashboard specification. No image files required — describe wireframes in prose and structured lists. A5 reads this file as its primary build instruction.

---

## Model Recommendation

**Claude claude-sonnet-4-5 (synthesis/creative)**

A4 is part creative director, part information architect. The model needs to understand data hierarchy, visual communication principles, and how to organize complex information for a non-technical audience. Sonnet 4.5 handles this well.

This is not a mechanical task (don't use Haiku) but it's not pure strategy either (Sonnet 4.6 is overkill here). Sonnet 4.5 is the right call.

---

## Example Prompt Template

```
You are a senior UX designer and information architect. You're designing a live internal dashboard that makes complex ICP intelligence accessible to marketers, executives, and product teams.

## Brand Configuration

Company: {{company_name}}
Primary color: {{brand_primary_color}}
Secondary color: {{brand_secondary_color}}
Font preference: {{brand_font}}
Logo URL: {{brand_logo_url}}
Dashboard title: {{dashboard_title}}

## Data to Display

**Segment profiles (A1):**
{{a1_segments_json}}

**Conviction scores and signals (A2):**
{{a2_conviction_json}}

**Hypotheses and product ideas (A3):**
{{a3_hypotheses_json}}

## Dashboard Audience

Primary users: Marketing team, creative team, product managers
Secondary users: Executives, external agency partners
External page: Stripped-down version for agency/partner reference

## Design Constraints

- Pure HTML/CSS/JS output (no React, no build tools)
- Self-contained (no external CDN dependencies — inline or bundle everything)
- Must work when opened as a local file
- Mobile-readable (not mobile-first, but not broken on phones)
- Last updated timestamp must be visible on every page

## Instructions

Design two pages:

### Page 1: Internal Dashboard (index.html)
Full intelligence hub for the internal team.

Specify:
1. **Header** — What appears at the top (company name, logo position, last-updated date, navigation links)
2. **Summary section** — A quick-read overview: top conviction tier, number of active segments, number of P1 hypotheses, last refresh date
3. **Conviction stack section** — Visual ranking of all segments by score. Include: segment name, score, tier badge, trend indicator, 1-line summary
4. **Segment deep-dives** — For each segment: full X-ray data, supporting signals, top 3 hypotheses tied to this segment
5. **Hypothesis backlog** — Filterable list of all hypotheses. Show: hypothesis text, segment, type, priority, effort, signal basis
6. **Product ideas** — Card-based layout. Show: idea, segment, pain point, priority
7. **Footer** — Last run date, data date range, link to external page

### Page 2: Agency Reference (agency.html)
Simplified external-facing page. Omit: raw signal data, product ideas, detailed scoring breakdown. Include: segment names and brief descriptions, top emotional triggers and language patterns, top 2 hypotheses per segment (P1/P2 only), brand voice notes.

### Component Inventory
List every UI component you've specified, in this format:
- Component name
- Data source (which JSON field)
- Display format (table / card / list / badge / chart type)
- Notes

### Copy Guidelines
- Dashboard copy tone: {{copy_tone}} (e.g., "direct and data-driven" or "strategic and consultative")
- Label conventions: specify what to call scores, tiers, and sections
- Any terminology to avoid

## Output Format

Write the specification as a Markdown document with clear section headers. Be specific enough that a developer can build from this spec without asking clarifying questions. If you're specifying a component, describe its exact contents, not just its purpose.
```

---

## Notes

- A4 runs once per full pipeline cycle (not on weekly refresh). Only re-run it if the data structure changes, you add new segments, or you want to redesign the dashboard.
- The most common mistake: vague specs that say "show segment data here" without specifying which fields, in what order, with what visual treatment. Be specific.
- If the company has an existing color palette and font in the config, use it. If not, default to a clean dark-header / white-body layout with a sans-serif font. Don't get creative with the defaults — the goal is usability.
- The spec should explicitly note which sections are "static on build" (only change on full pipeline runs) vs. "data-driven from data.json" (update on every weekly refresh). A5 needs to know this to architect the HTML correctly.
