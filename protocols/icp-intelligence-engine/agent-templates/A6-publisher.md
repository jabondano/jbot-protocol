# A6 — Publisher

**Role:** Deploy the dashboard to a live hosting platform, set up the weekly refresh schedule, and produce a deployment log with all relevant URLs and configuration.

---

## Input Required

| Input | Format | Required |
|---|---|---|
| Dashboard files | `dashboard/` folder (index.html, agency.html, data.json) | Yes |
| Deployment config | from `example-config.json` | Yes |
| Previous deployment log | `outputs/a6-deployment-log.json` | No (first run only) |

**Dependency:** Must wait for A5 to complete.

---

## Output Produced

File: `outputs/a6-deployment-log.json`

Contains:
- Internal dashboard URL
- External agency page URL
- Deployment timestamp
- Platform used
- Cron/automation config summary
- Notes and warnings

---

## Model Recommendation

**Claude Haiku or GPT-4.1 mini**

A6 is almost entirely mechanical: run CLI commands, validate URLs, write a structured log, and output a cron config. The LLM's job here is to generate the correct shell commands for the chosen platform, handle any error output from the CLI, and produce a clean deployment log. Haiku is sufficient.

If you're using A6 to also write a README or instructions for a non-technical stakeholder to access the dashboard, step up to Sonnet 4.5 for that section.

---

## Deployment Instructions by Platform

### Option A: Cloudflare Pages

```bash
# Install wrangler if not already installed
npm install -g wrangler

# Authenticate (first run only)
wrangler login

# Deploy
wrangler pages deploy ./dashboard \
  --project-name={{project_name}} \
  --branch=main

# Output: https://{{project_name}}.pages.dev
```

To set a custom domain: configure in Cloudflare dashboard → Pages → Custom Domains.

### Option B: Vercel

```bash
# Install Vercel CLI if not already installed
npm install -g vercel

# Deploy (first run — follow prompts)
vercel ./dashboard --prod

# Subsequent deploys
vercel ./dashboard --prod --yes
```

### Option C: Netlify

```bash
# Install Netlify CLI if not already installed
npm install -g netlify-cli

# Authenticate (first run only)
netlify login

# Deploy
netlify deploy --prod --dir=./dashboard --site={{netlify_site_id}}
```

---

## Weekly Refresh Automation

### GitHub Actions (recommended)

Create `.github/workflows/icp-refresh.yml`:

```yaml
name: ICP Intelligence Weekly Refresh
on:
  schedule:
    - cron: '0 6 * * 1'  # Monday 6am UTC
  workflow_dispatch:

jobs:
  refresh:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run refresh pipeline (A1 → A2 → A3 → A5 data.json)
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          META_ADS_TOKEN: ${{ secrets.META_ADS_TOKEN }}
          GOOGLE_ADS_KEY: ${{ secrets.GOOGLE_ADS_KEY }}
          CRM_API_KEY: ${{ secrets.CRM_API_KEY }}
        run: python run_pipeline.py --mode=refresh

      - name: Deploy updated dashboard
        env:
          CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}
        run: |
          npm install -g wrangler
          wrangler pages deploy ./dashboard --project-name=${{ env.PROJECT_NAME }}

      - name: Notify on failure
        if: failure()
        run: echo "Pipeline failed — check logs and notify team"
```

### Alternative: System cron (VPS deployment)

```bash
# Add to crontab (crontab -e)
0 6 * * 1 /usr/bin/python3 /path/to/run_pipeline.py --mode=refresh >> /var/log/icp-refresh.log 2>&1
```

---

## Example Prompt Template

```
You are a DevOps engineer running a deployment pipeline. Your job is to deploy a static HTML dashboard to {{hosting_platform}} and produce a structured deployment log.

## Deployment Configuration

Platform: {{hosting_platform}}
Project name: {{project_name}}
Source directory: ./dashboard/
Internal dashboard file: index.html
External agency file: agency.html

Previous deployment URL (if any): {{previous_url}}

## Instructions

1. Generate the exact CLI commands to deploy the dashboard to {{hosting_platform}}.

2. Validate that the deployment succeeded by:
   - Confirming the CLI output includes a success message and URL
   - Flagging any warnings or errors in the CLI output

3. Generate the automation config:
   - Weekly cron expression for Monday 6am UTC
   - GitHub Actions YAML for the refresh schedule
   - List of secrets required (API keys, deploy tokens)

4. Write the deployment log as a JSON file capturing:
   - Deployment timestamp (ISO 8601)
   - Platform used
   - Internal dashboard URL
   - External agency URL  
   - Data date range deployed
   - Deployment status: "success" | "failed" | "partial"
   - Notes: any warnings, anomalies, or manual steps required

5. Write a plain-English summary (3–5 sentences) that a non-technical stakeholder can read to understand what was deployed and where to find it.

## Output Format

Return:
1. Shell commands block (for execution)
2. GitHub Actions YAML block
3. deployment-log.json block
4. Plain-English summary
```

---

## Notes

- Always verify the deployed URL is live before writing "success" in the log. A 200 response from the index URL is the minimum bar.
- The deployment log is the audit trail. Keep every run's log — don't overwrite, append with a timestamp key.
- Required secrets per platform: Cloudflare Pages needs `CLOUDFLARE_API_TOKEN` + account ID. Vercel needs `VERCEL_TOKEN`. Netlify needs `NETLIFY_AUTH_TOKEN` + site ID. Store these in your CI/CD secrets manager, never in code.
- On weekly refresh runs, A6 only redeploys `data.json` (and the HTML files if they haven't changed). A smart deploy checks file hashes and skips unchanged files — Cloudflare Pages and Netlify do this automatically.
- If deployment fails, the previous version stays live. This is a feature, not a bug. Log the failure, fix the issue, and re-trigger.
