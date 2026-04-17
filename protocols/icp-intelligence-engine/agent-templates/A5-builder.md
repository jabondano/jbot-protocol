# A5 — Builder

**Role:** Generate the complete, production-ready HTML dashboard from the spec. Ship something that opens in a browser and works.

---

## Input Required

| Input | Format | Required |
|---|---|---|
| Dashboard spec | `outputs/dashboard-spec.md` | Yes |
| Segment profiles | `outputs/a1-segments.json` | Yes |
| Conviction scores + signals | `outputs/a2-conviction.json` | Yes |
| Hypotheses + product ideas | `outputs/a3-hypotheses.json` | Yes |
| Brand config | from `example-config.json` | Yes |

**Dependency:** Must wait for A4 to complete.

---

## Output Produced

Three files in the `dashboard/` folder:

| File | Purpose |
|---|---|
| `dashboard/index.html` | Main internal ICP hub |
| `dashboard/agency.html` | External agency reference page |
| `dashboard/data.json` | Structured data snapshot (used by both HTML files) |

**Architecture rule:** The HTML files must load their content from `data.json` using inline JavaScript. This is the key design decision: weekly refresh only requires updating `data.json` and redeploying — no HTML rebuild needed.

---

## Model Recommendation

**Claude claude-sonnet-4-5 or OpenAI o3 / GPT-4.1**

Building a complete, functional HTML file from a spec requires sustained attention, precise code generation, and the ability to hold a complex spec in context while generating 500–1000+ lines of code. 

Preferred: **Claude claude-sonnet-4-5** — excellent at following detailed specs and producing clean HTML/CSS/JS in one shot.  
Alternative: **GPT-4.1** — strong at code generation, handles long contexts well.  
Avoid: Haiku (too many spec-following failures on complex layouts), any model with <32k context if your spec is long.

If the first output has bugs, do one targeted fix pass before re-generating the full file.

---

## Example Prompt Template

```
You are an expert frontend developer. Build a complete, self-contained HTML dashboard from the specification below.

## Spec

{{dashboard_spec_markdown}}

## Brand Configuration

Primary color: {{brand_primary_color}}
Secondary color: {{brand_secondary_color}}  
Font: {{brand_font}}
Logo URL: {{brand_logo_url}}
Company name: {{company_name}}

## Data

All content data is in `data.json`. Your HTML must load this file on page load using fetch() and populate the DOM from it. Do not hardcode any segment, hypothesis, or signal data into the HTML.

Here is the data to generate data.json from:

**Segments (A1):**
{{a1_segments_json}}

**Conviction data (A2):**
{{a2_conviction_json}}

**Hypotheses (A3):**
{{a3_hypotheses_json}}

## Technical Requirements

1. **Zero external dependencies at runtime.** Inline all CSS. If you use any JavaScript library (e.g., a small charting lib), bundle it inline. The file must work when opened directly in a browser with no internet connection.

2. **Data-driven from data.json.** On load, fetch('./data.json', parse it, and populate the DOM. Add a graceful error state if the fetch fails (e.g., "Unable to load data. Ensure data.json is in the same directory.").

3. **Last-updated timestamp.** Visible on every page. Read from `data.json` field `meta.last_updated`.

4. **Responsive layout.** Must not break at mobile widths. A single-column stacked layout at <768px is acceptable.

5. **Navigation.** If you have multiple sections, include a sticky top nav with anchor links. If you have two pages (index.html + agency.html), link between them.

6. **Conviction tier badges.** Use distinct colors: Core = green, Emerging = yellow/amber, Speculative = gray. Include a trend indicator: ▲ up, — flat, ▼ down.

7. **Hypothesis priority badges.** P1 = red/urgent, P2 = orange, P3 = gray.

8. **Print-friendly.** Add a CSS print media query that removes nav, collapses the layout to single-column, and prints cleanly.

## Deliverables

Produce THREE separate code blocks, clearly labeled:

### FILE: dashboard/data.json
[complete JSON]

### FILE: dashboard/index.html
[complete HTML with inline CSS and JS]

### FILE: dashboard/agency.html
[complete HTML — simplified external version per spec]

Do not truncate. Output complete files.
```

---

## Notes

- The data.json architecture is load-bearing. Don't skip it. Weekly refresh works by regenerating data.json + redeploying — if content is hardcoded in HTML, every refresh requires a full HTML rebuild.
- Generate `data.json` first (it's the simplest file) and confirm the schema matches what the HTML JavaScript expects before writing the HTML.
- If the model truncates output due to length, prompt it to continue from where it stopped rather than regenerating the whole file. Keep a running file and append.
- Common bugs to watch for: fetch() fails silently when the spec uses async/await without error handling; CSS variables not defined in `:root`; mobile layout breaking due to `min-width` on flex children.
- A5 does NOT run on weekly refresh. Only `data.json` regenerates weekly. Full A5 re-run only for design changes or new segment additions.
