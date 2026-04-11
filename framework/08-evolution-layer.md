# Layer 5: The Evolution Layer

**The system that gets better by being used.**

---

## What This Is

Most deployed agent fleets plateau. The bots run, the reports post, the team scans them. Nothing changes. Six months later the system is technically running but organizationally stale — bots optimized for how the business worked at deployment, not how it works now.

The Evolution Layer is the design pattern that prevents this.

It creates a feedback loop between the system's output and the team's behavior. The system surfaces patterns → humans act on the most interesting ones → those actions become new workflows → the workflows generate new patterns → repeat.

The humans make the system better. The system makes the humans more effective. Neither is sufficient alone.

---

## The Five Layers

```
Layer 1: Agent Fleet        — bots, crons, skills, signals
Layer 2: Data Layer         — Supabase (bot-to-bot) + Airtable (human-facing)
Layer 3: Interface Layer    — Discord (team ops) + Telegram (exec alerts)
Layer 4: Intelligence Layer — Active Memory, shared brain (bot_signals)
Layer 5: Evolution Layer    — feedback loop, pattern surfacing, team learning
```

Each layer depends on the ones below it. You can't have Layer 5 without a functional Layer 4. You can't skip to the learning channel before the bots are actually producing reliable output.

---

## The Core Mechanism: #lab

The Evolution Layer is operationalized through a single weekly channel post.

**The channel:** `#lab` — lives in BRIEFINGS. Visible to everyone.

**The cadence:** Once per week. Monday morning, after the weekly pulse.

**The format:** One thought. 3–5 lines. Written in plain language for the whole team, not the person who built the bots.

**The content types** (rotate to avoid wallpaper effect):
1. **Capability reminder** — something the fleet can already do that the team has forgotten or never tried
2. **Business idea** — something worth exploring given current priorities and what the bots are seeing
3. **Pattern observation** — a non-obvious trend the bots have noticed across data, framed as a question
4. **Technology signal** — something worth knowing about AI tools, competitor behavior, or market shifts
5. **Open question** — a genuine "has anyone thought about this?" that might lead somewhere

**What it is NOT:**
- A report (no data, no numbers, no chart summaries)
- A technical brief (no cron IDs, bot names, API references)
- A ticket (not "here is what we should do" — more "here is what I've been noticing")
- A status update (the other channels do that)

---

## The Feedback Loop

```
Bots generate output
      ↓
Patterns emerge (the bots see more than the humans do)
      ↓
#lab surfaces one pattern per week as a question or idea
      ↓
Team engages: ignores (signal: low value) or acts (signal: high value)
      ↓
Acting creates: new cron, new workflow, new integration, new experiment
      ↓
New workflow generates new patterns
      ↓
(repeat)
```

**Key insight:** The team's engagement is the signal. If a #lab post generates zero reaction, the system learned something — that framing didn't land, or that area isn't a priority. If a post generates a new project, the system learned something more valuable.

This is not autonomous learning. It requires humans in the loop. The humans are the judgment layer. The system provides the surface area for ideas to emerge.

---

## Implementation

### The #lab Cron

- **Bot:** mktgbot (most cross-channel awareness)
- **Model:** Sonnet (creative thinking required)
- **Schedule:** Every Monday, 10 AM ET — after the weekly-pulse
- **Prompt style:** Non-technical, curious, variable format week-to-week

The prompt instructs the agent to:
1. Consider what the fleet has been doing and what patterns might be worth surfacing
2. Choose a format type (capability reminder / idea / observation / signal / question)
3. Write 3–5 lines in plain team language
4. Post to #lab via message tool

The key constraint in the prompt: *"Do not reference specific data, reports, or numbers. Write like a smart colleague leaving a note on a whiteboard, not like a bot filing a report."*

### What Makes a Good #lab Post

**Good:**
> "The highest-performing Armor ads all show the glasses in a workplace context rather than an outdoor one. We've never built a landing page around that. Worth exploring?"

> "Reminder that the bots can draft B2B follow-up sequences based on deal stage. If there are accounts sitting in HubSpot without recent activity, ask in #b2b and salesbot will pull a list."

> "Three of the five influencers who posted about Lucyd this month came from the safety/PPE space, not the tech space. That's a new audience we haven't explicitly targeted."

**Not good:**
> "Ad set #4231 had a 14% ROAS decline. Consider pausing."
*(That's a report. It belongs in #ads.)*

> "We should add a cron job to adsbot at 3 PM that checks fatigue scores daily."
*(That's a technical ticket. It belongs in #ops or Slack.)*

---

## Governance

**Who reviews #lab posts before they publish?**
Nobody. The agent posts directly. The goal is low friction — if every post requires review it becomes another task and the cadence breaks.

**What if a post is bad or irrelevant?**
React with ❌ or leave it. The low-engagement signal feeds back into future posts (the agent can observe that nobody responded and adjust).

**What if a post sparks something real?**
Reply in the thread. If it becomes a real project, one person owns it and moves it to the appropriate project channel.

**Can anyone suggest a #lab topic?**
Yes. Post in #lab directly, tag the bot. It will incorporate it into future posts.

---

## Connection to JBOT Protocol Engagements

For every client deployment, Layer 5 is the last layer to activate — only after Layers 1–4 are stable.

The #lab channel should be added to every client's Discord server during the initial buildout, but left dormant until there is at least 4–6 weeks of bot output to draw from. An empty system has no patterns to surface.

At the 6-week mark, activate the lab-weekly cron. At 12 weeks, review whether the engagement pattern justifies expanding the cadence or adding a second format (e.g., a monthly deeper analysis).

---

*JBOT Protocol · jabondano.co · 🦞*

---

## Addendum: AI Video Generation Rules

**Core principle (always apply):**

> Never use text-to-video for product ads. Text-to-video generates AI-invented product visuals that will never match the real product. Always use image-to-video starting from the actual product photo.

### The Rule

| Use Case | Method | Source |
|----------|--------|--------|
| Product ad, hero shot | image-to-video | Shopify CDN / R2 product photo |
| Brand lifestyle B-roll | text-to-video | Prompt only (no product shown) |
| Product demo / spin | image-to-video | White-background product photo |
| UGC-style ad | image-to-video | Lifestyle photo with product on person |

### Source of Truth for Product Images

1. **Shopify CDN** — live product photos, always current
   - URL pattern: `https://cdn.shopify.com/s/files/1/0553/0324/1846/files/[filename]`
   - Use the `Perspective.jpg` variant as the primary input
2. **Cloudflare R2** (`lucyd-resources-assets`) — upload canonical set here for stable URLs
3. **Never use internal/Drive files** — local paths cause size errors on upload

### Prompt Structure for Product Image-to-Video

```
[What happens to/with the product] + [Scene/environment] + [Mood/energy] + [Camera movement]
```

Example:
> "Hands pick up the Reebok sport sunglasses, put them on, camera follows wearer outside into bright daylight. Sport energy, urban street, golden light."

The prompt describes the ACTION, not the product. The product comes from the image.
