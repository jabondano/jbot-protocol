# Discord Design Generator

**JBOT Protocol — Template v1.0**

> **Purpose:** Takes a completed client intake (discovery interview + master context file) and generates a full Discord server architecture. Run this after Phase 0 research and discovery are complete, before deploying any bots.

---

## How to Use This

1. Complete the `discovery-interview.md` with the client
2. Have the master context file ready (or the notes from the intake)
3. Feed both into this generator prompt (use with jbot or any JBOT Protocol agent)
4. Review the output — override anything that doesn't match the business reality
5. Use the output as the canonical reference for bot routing, channel config, and onboarding

---

## Generator Prompt

Copy this prompt into jbot (or any JBOT Protocol agent) with the intake context attached:

```
You are a JBOT Protocol architect. You have just completed a discovery interview with a new client.

Using the intake data below, generate a complete Discord server design for deploying their AI agent fleet.

Your output must include:

1. SERVER NAME
   Short, internal-facing. Not the company's public brand name.
   Format: [Company Nickname] Ops

2. CATEGORY MAP
   Map each major department or business function to a Discord category.
   Rules:
   - Max 6–7 categories (one per real department or domain)
   - Name from the team's vocabulary, not consultant vocabulary
   - Order by daily operational priority (what they look at most = first)

3. CHANNEL MAP
   For each category, list 2–4 channels.
   Each channel must have:
   - Name (kebab-case, workflow-first naming)
   - Bot owner (which agent writes here)
   - Content type (what gets posted, how often)
   - Human action trigger (what causes a human to respond)
   - autoThread: yes/no

   Rules:
   - Name after the workflow, not the bot (#fulfillment not #shipbot)
   - Start with fewer channels — add later when the workflow exists
   - Include #video-studio under Marketing if video generation is in scope

4. PERMISSION MODEL
   Based on the org chart and team composition from the intake:
   - List roles (e.g. Executive, Ops Team, Agency Partner, Read-Only)
   - For each role: which categories are visible, which are hidden
   - Flag which channels are exec-only vs team-visible

5. ROUTING POLICY
   Define the Discord vs mobile (Telegram/WhatsApp) split:
   - What signal types go to Discord only
   - What signal types get a mobile summary
   - What signal types wake the executive immediately
   Use the client's actual communication preferences from the intake.

6. BOT OWNERSHIP MAP
   List each bot and its primary channel(s).
   Flag any gaps — workflows that need a bot but don't have one yet.

7. IMPLEMENTATION CHECKLIST
   Ordered list of manual steps the client needs to complete in Discord
   before the bots go live (channel creation, role setup, etc.).
   Note: channel creation from a VPS is often blocked by Discord's API —
   these steps must be completed manually or from a local browser session.

8. FIRST 30 DAYS
   Which channels will be active immediately vs which are placeholders.
   Recommend starting with 1–2 channels per category max.

---

INTAKE DATA:
[Paste discovery interview output or master context file here]
```

---

## Output Format Reference

A well-formed output looks like this (abbreviated):

```
SERVER NAME: Lucyd Ops

CATEGORY MAP:
  📊 BRIEFINGS      → Cross-functional pulse + system health
  🏷️ PRODUCT LINES  → Product-specific ops by SKU
  💰 REVENUE        → DTC, B2B, key accounts
  📣 MARKETING      → Campaigns, content, ads, video
  🚀 GROWTH         → Strategic initiatives
  ⚙️ OPERATIONS     → Supply chain, fulfillment, system

CHANNEL MAP (Marketing):
  #marketing    | mktgbot     | daily digest          | weekly review  | autoThread: no
  #campaigns    | mktgbot     | active campaign status | decision needed | autoThread: yes
  #ads          | adsbot      | Meta performance + fatigue alerts | action needed | autoThread: yes
  #social       | socialbot   | weekly content calendar | approval | autoThread: no
  #content      | contentbot  | copy + SEO drops       | review     | autoThread: yes
  #video-studio | adsbot      | AI video variants      | approve/reject | autoThread: yes

PERMISSION MODEL:
  Executive    → all categories visible
  Ops Team     → BRIEFINGS, OPERATIONS, their department
  Agency       → MARKETING only (no REVENUE, no OPERATIONS)
  Read-Only    → BRIEFINGS only

ROUTING POLICY:
  info     → Discord only
  warning  → Discord + Telegram summary
  alert    → Telegram immediately + Discord detail
  critical → Telegram (wake exec)

BOT OWNERSHIP:
  mktgbot   → #marketing, #campaigns
  adsbot    → #ads, #video-studio
  socialbot → #social
  shipbot   → #fulfillment
  salesbot  → #sales, #dtc, #b2b
  sysbot    → #system, #safety

IMPLEMENTATION CHECKLIST:
  [ ] Create server (or identify existing workspace)
  [ ] Enable Developer Mode in Discord settings
  [ ] Create categories (manual — API blocked from VPS)
  [ ] Create channels under each category (manual)
  [ ] Copy channel IDs (right-click each → Copy Channel ID)
  [ ] Paste IDs into bot openclaw.json config
  [ ] Set systemPrompt per channel (use templates in /templates/CUSTOM-INSTRUCTIONS.md)
  [ ] Set autoThread: true on review + strategy channels
  [ ] Configure roles + visibility (Discord role settings)
  [ ] Send pinned explainer to each channel
  [ ] Test: post a message, confirm bot responds in correct thread
  [ ] Wire bot_signals table for cross-bot awareness

FIRST 30 DAYS (active channels):
  Week 1: #sales, #fulfillment, #system (highest existing pain)
  Week 2: #marketing, #ads (after Meta API confirmed)
  Week 3: #content, #social (content pipeline running)
  Week 4: All remaining channels + video-studio if Seedance integrated
```

---

## Design Principles (Apply to Every Client)

**1. Org chart = category map.**
If the client has a Sales team, a Marketing team, and an Ops team, those are your three core categories. Don't invent structure that doesn't exist in the business.

**2. Empty channels are debt.**
Every channel you create that has no active bot or no active workflow is noise. Delay creation until the work is real.

**3. The executive is not on Discord all day.**
Design for async. The executive gets a Telegram (or WhatsApp) nerve ending. Discord is the work floor. These are different audiences with different needs.

**4. Name channels after outcomes, not agents.**
`#fulfillment` tells you what happens there. `#shipbot` tells you who's talking. The team cares about the outcome.

**5. Start with signal channels, add conversation later.**
The first channels should be bot-write, human-read. Conversation channels (threads, async discussion) come after the team trusts the bots.

**6. Thread everything that has a lifecycle.**
If a bot posts something that might need follow-up, enable autoThread. The thread is the work item. When it's resolved, it's done. The channel stays clean.

---

## Common Patterns by Business Type

### Distribution / Supply Chain Heavy
Lead with OPERATIONS. Key channels: `#fulfillment`, `#inventory`, `#vendors`, `#alerts`.
Bot priority: shipbot, opsbot first. Marketing can wait.

### DTC E-Commerce
Lead with REVENUE + MARKETING. Key channels: `#dtc`, `#ads`, `#social`, `#video-studio`.
Bot priority: adsbot, socialbot, salesbot.

### B2B / Wholesale
Lead with REVENUE + OPERATIONS. Key channels: `#key-accounts`, `#pipeline`, `#fulfillment`.
Bot priority: salesbot, shipbot.

### Agency / Services
Lead with PROJECTS. Key channels: one per active client (thread-heavy, autoThread: yes on everything).
Bot priority: one project bot per client, shared coordinator bot.

### Family / SMB
Keep it simple. 2–3 categories max. 1 bot to start.
Discord may be overkill — evaluate Telegram-only first.

---

## Related Templates
- `templates/discovery-interview.md` — feeds into this generator
- `templates/CUSTOM-INSTRUCTIONS.md` — systemPrompt templates per channel type
- `docs/DISCORD-ARCHITECTURE.md` — Layer 3 architecture overview
- `guides/discord-server-deployment.md` — full deployment strategy + lessons learned

---

*JBOT Protocol · jabondano.co · 🦞*
