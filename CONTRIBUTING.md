# Contributing to JBOT Protocol

Thank you for your interest in contributing! JBOT Protocol is built through real deployments, and your contributions make the methodology stronger.

## How to Contribute

### 1. Share Your Configuration

Deployed a system at your company? Share your anonymized config:

1. Copy your config JSON
2. Anonymize company-specific data (brand names, contact info, etc.)
3. Add it to `systems/[system-name]/examples/`
4. Create a PR with:
   - Your config file
   - A brief note about your use case (industry, company size, what you learned)

### 2. Document Edge Cases

Hit a configuration issue? Document it:

1. Open an issue describing the problem
2. Explain your use case
3. Share what you tried
4. Propose a solution (if you found one)

We'll add it to the docs so the next person doesn't hit the same issue.

### 3. Add Platform Integrations

Want to add support for a new platform?

Examples: New CRM (Salesforce adapter), new ad platform (TikTok), new inventory system (WooCommerce)

Process:
1. Create an adapter in `systems/[system-name]/adapters/[platform].py`
2. Update the intake questionnaire to include the new platform
3. Add an example config using the new platform
4. Submit a PR

### 4. Improve Intake Questions

Found a question that's confusing or missing?

1. Open an issue describing the problem
2. Propose better wording or a new question
3. Explain what business context it captures

We refine intake questions with every deployment.

### 5. Write Case Studies

Deployed at your company and have metrics?

We'd love to feature your story:
1. Write a case study (use `docs/CASE-STUDY-LUCYD.md` as a template)
2. Anonymize if needed
3. Include before/after metrics
4. Share what worked and what didn't
5. Submit as a PR to `systems/[system-name]/docs/`

### 6. Build New Systems

Have an operational pattern from your company that could be a new system?

Examples: Support triage, customer success monitoring, partnership pipeline

Process:
1. Open an issue describing the system
2. Explain the workflow (what gets automated)
3. Share real metrics (if you have them)
4. We'll work together to extract the universal pattern

## Code of Conduct

- **Be respectful** — Everyone is here to learn and improve operations
- **Share learnings** — Document what worked and what didn't
- **Anonymize responsibly** — Don't expose proprietary data
- **Give credit** — If you adapted someone's config, acknowledge them

## Questions?

Open an issue or start a discussion in GitHub Discussions.

Built in public. Deploy in reality.
