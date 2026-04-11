# Discord as an Operations Layer for AI Agent Fleets

**JBOT Protocol — Deployment Strategy v1.0**
*Author: Joaquin Abondano | jabondano.co*
*Status: Live — deployed at Lucyd/Innovative Eyewear (NASDAQ: LUCY)*

---

## The Core Idea

Most companies use Discord as a chat room. That's the wrong mental model.

When you're running a fleet of AI agents, Discord becomes a **control plane** — a persistent, structured workspace where bots do work, humans review it, and decisions get made without anyone needing to be in the same room.

This document captures the architecture pattern deployed at Lucyd with 10 active bots across two VPS servers. It is the reusable JBOT Protocol playbook for Discord server deployment with multi-agent teams.

---

## Why Discord (Not Slack, Not Telegram)

| Platform | Best For | Why We Use It |
|----------|----------|--------------|
| **Discord** | Bot-heavy ops, structured channels, free | Primary work floor — all bot output lives here |
| **Telegram** | Executive alerts, mobile-first, individual DMs | Executive layer — synthesized summaries only |
| **Slack** | Large human teams, integrations | Too expensive for bot-heavy deployments |
| **Email** | External comms, formal records | Not for real-time ops |

**Rule:** Discord = detail and volume. Telegram = signal and decision.

Bots write to Discord with full context. Joaquin gets a Telegram summary when something needs a decision.

---

## Architecture Principles

### 1. Category = Department
Each Discord category maps to a business function or organizational unit. It is not a project folder — it is a persistent operational domain.

```
📊 BRIEFINGS        → Cross-functional summaries and pulse
🏷️ PRODUCT LINES    → Product-specific ops (by SKU line)
💰 REVENUE          → Sales channels (DTC, B2B, key accounts)
📣 MARKETING        → Campaign, content, ad, video workflows
🚀 GROWTH PROJECTS  → Active strategic initiatives
⚙️ OPERATIONS       → Supply chain, fulfillment, system health
```

### 2. Channel = Workflow Stream
A channel is not a topic — it is an **owned workflow stream**. One bot owns it. That bot writes to it. Humans read and react.

```
#ads          → adsbot writes ad performance + creative variants
#fulfillment  → shipbot writes order status + logistics alerts
#social       → socialbot writes weekly content calendar
#influencer   → studiobot writes creator analysis + outreach
#sales        → salesbot writes HubSpot + Amazon + Shopify digests
```

### 3. Bot = Channel Owner
Each bot owns a domain. It does not bleed into other channels unless routing a signal. Ownership = accountability. When something breaks in `#fulfillment`, that's shipbot's problem. Clear ownership also makes debugging easy.

### 4. Thread = Work Item
For channels with `autoThread: true`, every message from a bot (or human) starts a new thread. That thread is a work item — discussion, follow-up, and resolution all live there. The channel is clean. The history is traceable.

### 5. Role = Visibility Layer
Not everyone should see everything. The 2-tier access model:

| Tier | Who | Can Do |
|------|-----|--------|
| **Read** | Team members, partners | See channel output, react, comment |
| **Write/Approve** | Exec (Joaquin) | Trigger crons, approve actions, confirm decisions |

Example: Cristina (Lemonlight, Reebok partner) sees `#ad-creative`, `#social`, `#content` — not `#sales`, `#fulfillment`, or `#system`.

**Enforcement:** Role permissions via Discord + behavioral guardrails in bot SOUL.md files. Not approval prompts — behavioral policy.

---

## The Full Channel Map (Lucyd v1)

### 📊 BRIEFINGS
| Channel | Owner | Frequency | Content |
|---------|-------|-----------|---------|
| `#weekly-pulse` | mktgbot | Weekly (Mon) | Cross-channel KPI summary |
| `#safety` | sysbot | On alert | Fleet health, security events |
| `#system` | sysbot | Daily | Cost watchdog, cron registry |

### 🏷️ PRODUCT LINES
| Channel | Owner | Content |
|---------|-------|---------|
| `#eyewear-sun` | contentbot | Sun/fashion line updates |
| `#sports` | contentbot | Armor + Reebok SKU ops |
| `#legacy` | contentbot | Older SKU wind-down |
| `#apps` | contentbot | App store + firmware updates |

### 💰 REVENUE
| Channel | Owner | Content |
|---------|-------|---------|
| `#dtc` | salesbot | Shopify + Amazon DTC performance |
| `#b2b` | salesbot | HubSpot wholesale pipeline |
| `#key-accounts` | salesbot | Strategic account tracking |
| `#international` | opsbot | EU/Canada/global distribution |

### 📣 MARKETING
| Channel | Owner | Content |
|---------|-------|---------|
| `#marketing` | mktgbot | Daily marketing digest |
| `#campaigns` | mktgbot | Active campaign tracker |
| `#ads` | adsbot | Meta ad performance + creative fatigue alerts |
| `#social` | socialbot | Weekly content calendar |
| `#content` | contentbot | Blog, SEO, Amazon copy drops |
| `#influencer` | studiobot | Creator profiles + collab pipeline |
| `#video-studio` | adsbot + socialbot | AI-generated video — review before publish |

### 🚀 GROWTH PROJECTS
| Channel | Owner | Content |
|---------|-------|---------|
| `#exchange-app` | contentbot | Lucyd Exchange app progress |
| `#research-app` | contentbot | Lucyd Research app progress |
| `#klick-by-lucyd` | mktgbot | Klick product updates |
| `#lead-gen` | salesbot | Inbound lead tracking |

### ⚙️ OPERATIONS
| Channel | Owner | Content |
|---------|-------|---------|
| `#alerts` | opsbot | Supply chain + freight alerts |
| `#sales` | salesbot | Full sales cron output |
| `#fulfillment` | shipbot | Order fulfillment + shipment tracking |

---

## Bot Routing Policy

```
Discord = operational detail + team visibility
Telegram = executive alerts + approval requests

If signal.severity == 'info':    → Discord only
If signal.severity == 'warning': → Discord + Telegram summary
If signal.severity == 'alert':   → Telegram immediately + Discord detail
If signal.severity == 'critical':→ Telegram immediately + wake Joaquin
```

This keeps Telegram clean. Noise in Discord is acceptable — signal in Telegram is sacred.

---

## The Video Studio Pattern (New in April 2026)

With Seedance 2.0 (ByteDance) integrated via fal.ai:

```
adsbot detects creative fatigue
  → generates 3 video variants via Seedance 2.0
  → drops in #video-studio with performance context
  → Joaquin reviews → reacts ✅ to approve
  → adsbot routes approved clip to Meta Ads Manager
```

```
socialbot generates weekly content calendar
  → pairs copy with AI-generated video clip
  → drops full package in #video-studio
  → approved clips scheduled to Instagram/TikTok/YouTube
```

`#video-studio` is the only channel where humans are the bottleneck by design. Everything else runs autonomously.

---

## Deployment Checklist for a New Client

```
[ ] Create Discord server (or use existing)
[ ] Create 6 categories matching org chart
[ ] Create channels per department (start with 2-3 per category)
[ ] Enable Developer Mode → copy channel IDs
[ ] Configure bot openclaw.json: channels.discord.guilds[guildId].channels
[ ] Assign systemPrompt per channel (context-aware)
[ ] Set autoThread: true on review + strategy channels
[ ] Configure role permissions (read vs write tier)
[ ] Seed channels with pinned explainer messages
[ ] Test: send a message, confirm bot responds in correct thread
[ ] Wire bot_signals for cross-channel awareness
```

---

## What We Learned (Live Deployment Lessons)

1. **VPS can't manage Discord API** — `403: error code: 1010` (Cloudflare blocks datacenter IPs). Channel creation and moves must be done manually from a browser or local bot session.

2. **Start with fewer channels** — 3 channels per category is better than 10. Add when the workflow exists, not before.

3. **Thread everything that matters** — `autoThread: true` is the right default for strategy and review channels. Signal channels (alerts, system) don't need it.

4. **Telegram is not optional** — Discord alone misses the executive. The two-channel model (Discord work floor + Telegram exec layer) is the correct architecture.

5. **Bot identity matters** — Each bot should have a clear SOUL.md with its domain, philosophy, and routing rules. Ambiguity = noise.

---

## Relationship to JBOT Protocol

This Discord architecture is Layer 3 of the JBOT Protocol stack:

```
Layer 1: Agent Fleet       — bots, crons, skills, signals
Layer 2: Data Layer        — Supabase (bot-to-bot), Airtable (human-facing)
Layer 3: Interface Layer   → Discord (team ops) + Telegram (exec)
Layer 4: Intelligence Layer— Active Memory, shared brain (bot_signals)
```

Every JBOT Protocol client deployment should replicate this structure. The org chart → category mapping is the key design decision. The rest follows from it.

---

*Built through deployment, not theory.*
*First live instance: Lucyd/Innovative Eyewear — 10 bots, 2 VPS, 100+ cron jobs.*
*jabondano.co | JBOT Protocol | 🦞*
