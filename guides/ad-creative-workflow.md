# Ad Creative Workflow — JBOT Protocol Guide

**The full pipeline from marketing signal to approved creative. Every step routes through Discord.**

---

## Overview

```
Signals → Brief (copy + visual) → Human approval → Creative execution → Human approval → Platform
```

No step executes without a human touchpoint. Nothing costs money without an explicit ✅.

---

## The Two Channels

| Channel | Purpose |
|---------|---------|
| `#campaigns` | Copy brief approval — text-only review before any creative is generated |
| `#video-studio` | Video creative review — motion content, Seedance 2.0 outputs |
| `#static-studio` | Static creative review — image composites, ffmpeg overlays |
| `#ad-studio` | Future: master creative ops channel combining both |

---

## Step 1 — Signal Compilation (mktgbot, Monday 9:45 AM ET)

mktgbot reads from Supabase:
- `icps` → who we're talking to
- `ad_variants` → ROAS by ICP × product × angle (what's working)
- `hooks` → winning copy by platform + score
- `ad_campaigns` → active priorities this week

Selects the top 3 combinations. For each writes a **Copy Brief** to `ad_briefs` table with `status='awaiting_approval'`.

Posts each brief to **#campaigns** as a separate message:
```
BRIEF [N] — AWAITING COPY APPROVAL
ICP: [name] | Product: [product]
Headline option A: [copy]
Headline option B: [copy]  
Body: [2-3 sentences]
CTA: [call to action]
Angle: [derived from ROAS data]
Format: [9:16 / 16:9 / 1:1 / 4:5]
Product image: [Shopify CDN URL]
Est. cost: ~$0.25 (video) or ~$0.05 (static)

React APPROVE to queue for creative execution.
```

---

## Step 2 — Copy Approval (Human)

Team reviews the copy in **#campaigns**. React ✅ to approve an individual brief. React ❌ to reject (logs reason for iteration).

mktgbot checks for reactions every 30 min. On ✅: updates `ad_briefs.status` to `'copy_approved'`.

**Nothing is generated until this step is complete.**

---

## Step 3a — Video Creative (adsbot, Monday 10:00 AM ET)

adsbot reads `ad_briefs` where `status='copy_approved'` and format is video.

For each:
1. Pulls product image from `product_image_url` (Shopify CDN)
2. Generates Seedance 2.0 Fast image-to-video
3. **CRITICAL RULE:** Always image-to-video. Never text-to-video for product ads. Product identity comes from the source image, not the prompt.
4. Posts to **#video-studio** with brief context attached

---

## Step 3b — Static Creative (adsbot, same window)

adsbot reads `ad_briefs` where `status='copy_approved'` and format is static.

For each:
1. Downloads product image from Shopify CDN to local temp
2. Uses ffmpeg to composite:
   - Product image as base (untouched — never run through image-to-image model)
   - Approved headline in Reebok red (#CC0000) or white depending on background
   - Body copy in smaller white text
   - CTA at bottom
   - Optional: Reebok logo top-right
3. Posts to **#static-studio** with brief context

**CRITICAL RULE:** Never run product images through image-to-image transformation models. The product must be the exact Shopify CDN image. All creative lives in the text/overlay layer, not in the product layer.

---

## Step 4 — Creative Approval (Human)

React ✅ on any video in **#video-studio** or static in **#static-studio** to approve.

adsbot checks for reactions every 30 min. On ✅:
- Updates `ad_briefs.status` to `'approved'`
- Writes to `ad_generation_log`
- Notifies socialbot/adsbot to route to platform

---

## Step 5 — Platform Distribution

Approved creative routes to:
- **Meta Ads** → adsbot uploads via Meta Ads API
- **Instagram/TikTok/YouTube** → socialbot schedules via content calendar

---

## Product Image Sources (Canonical)

Always use these URLs. Never use local files, Drive files, or AI-generated product images.

```
Reebok Octane Shift:  https://cdn.shopify.com/s/files/1/0553/0324/1846/files/Reebok_Octane_LCD010-25_Perspective.jpg
Reebok Nitrous Shift: https://cdn.shopify.com/s/files/1/0553/0324/1846/files/Reebok_Nitrous_LCD010-41_Perspective_1.jpg
Reebok Nitrous:       https://cdn.shopify.com/s/files/1/0553/0324/1846/files/Reebok_Nitrous_LCD010-40_Perspective.jpg
Reebok Voltage:       https://cdn.shopify.com/s/files/1/0553/0324/1846/files/Reebok_Voltage_LCD010-11_Perspective.jpg
Reebok Octane:        https://cdn.shopify.com/s/files/1/0553/0324/1846/files/Reebok_Octane_LCD010-20_Perspective.jpg
```

---

## Copy Brief Template

```
STRATEGY
  Who: [ICP name + one sentence description]
  What: [product + 2 key specs]
  Why now: [campaign + urgency]
  Angle: [derived from highest ROAS pattern for this ICP]

MESSAGING HIERARCHY
  Core message: [the one thing this ad communicates]
  
  Headlines (pick one):
    A: [contrarian / "don't buy" style]
    B: [benefit-forward]
    C: [social proof / authority]
  
  Body (2 lines max):
    [Pain/situation] + [product as solution]
  
  CTA: [single action — "Shop Now" / "See the difference" / specific URL]

VISUAL BRIEF
  Product: [name + Shopify CDN URL]
  Format: [9:16 / 4:5 / 1:1 / 16:9]
  Platform: [Meta / Instagram / TikTok / YouTube]
  Motion: [video or static]
```

---

## What NOT To Do

- ❌ Never use text-to-video for product ads
- ❌ Never run product images through image-to-image models  
- ❌ Never show AR/lens/HUD overlays — Lucyd makes AUDIO glasses, not display glasses
- ❌ Never show someone touching earbuds or headphones — open ears is the USP
- ❌ Never auto-publish — every creative needs a human ✅

---

*JBOT Protocol · jabondano.co · 🦞*
