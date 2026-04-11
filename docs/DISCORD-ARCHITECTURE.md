# Discord Architecture Overview

> Full guide: [guides/discord-server-deployment.md](../guides/discord-server-deployment.md)

## Quick Reference

Discord is **Layer 3** of the JBOT Protocol stack — the interface layer between your AI agent fleet and your team.

### Stack
```
Layer 1: Agent Fleet        — bots, crons, skills, signals
Layer 2: Data Layer         — Supabase (bot-to-bot) + Airtable (human-facing)  
Layer 3: Interface Layer    — Discord (team ops) + Telegram (exec alerts)
Layer 4: Intelligence Layer — Active Memory, shared brain (bot_signals)
```

### Core Rules
1. **Category = Department** — map your org chart
2. **Channel = Workflow Stream** — one bot owns it
3. **Bot = Channel Owner** — clear accountability
4. **Thread = Work Item** — autoThread on review channels
5. **Role = Visibility** — 2-tier: Read (team) vs Write/Approve (exec)

### Routing Policy
```
info    → Discord only
warning → Discord + Telegram summary  
alert   → Telegram immediately + Discord detail
critical→ Telegram immediately (wake exec)
```

### Live Reference
First deployment: Lucyd/Innovative Eyewear (NASDAQ: LUCY)  
10 bots · 2 VPS · 6 categories · ~20 channels · 100+ cron jobs

See full guide for channel map, checklist, and lessons learned.
