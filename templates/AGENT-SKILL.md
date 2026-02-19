# [Skill Name]

> Template for writing agent skills that bridge human SOPs and agent instructions.
> Uses RFC 2119 keywords: **MUST**, **SHOULD**, **MAY**, **MUST NOT**, **SHOULD NOT**

---

```yaml
# Metadata (YAML frontmatter)
division: [e.g., Fulfillment, Marketing, Finance]
owner: [Human responsible for this skill]
agent: [e.g., shipbot, mktgbot, financebot]
trigger: [schedule | event | on-demand]
schedule: [e.g., "0 6 * * *" or "Every 4 hours" — only if trigger is schedule]
source_sop: [Link to PROCESSES.md or internal SOP this skill encodes]
model_tier: [haiku | sonnet | opus]
token_budget: [e.g., ~8K, ~15K — estimated tokens per execution]
silent_if_nothing: [true | false — suppress output when no actionable data found]
last_reviewed: [YYYY-MM-DD]
```

---

## Purpose

[1-2 sentences: What this skill does and why it matters to the business.]

---

## Inputs

The agent MUST have access to:

| Input | Source | Required |
|-------|--------|:--------:|
| [e.g., Unfulfilled orders] | [e.g., Shopify API via MCP] | Yes |
| [e.g., Shipping status] | [e.g., NetSuite via MCP] | Yes |
| [e.g., Customer tier] | [e.g., CRM lookup] | No |

---

## Procedure

1. **[Step Name]**
   - The agent MUST [specific action with all parameters].
   - [Additional detail if needed.]

2. **[Step Name]**
   - The agent SHOULD [recommended action].
   - If [condition], the agent MUST [alternative action].

3. **[Step Name]**
   - The agent MAY [optional enhancement].
   - The agent MUST NOT [prohibited action].

---

## Decision Criteria

| Condition | Action |
|-----------|--------|
| [e.g., Order age > 7 days] | [e.g., Escalate to COO immediately] |
| [e.g., Order value > $500] | [e.g., Flag as high-priority] |
| [e.g., Customer is wholesale] | [e.g., Include account manager in alert] |

---

## Output Format

The agent MUST format output as:

```
[Example output format — what the Slack message, report, or response looks like]
```

---

## SQL Template (Optional)

Include if the skill relies on a specific query pattern:

```sql
-- [Brief description of what this query returns]
SELECT ...
FROM ...
WHERE ...
```

---

## Curl Template (Optional)

Include if the skill calls an external API directly:

```bash
# [Brief description of the API call]
curl -s -X GET "https://..." \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json"
```

---

## Boundaries

### Always
- [e.g., Include order numbers in all alerts]
- [e.g., Log every execution to audit trail]

### Ask First
- [e.g., Before contacting a customer directly]
- [e.g., Before modifying any record]

### Never
- [e.g., Send external emails without human approval]
- [e.g., Delete or cancel orders]
- [e.g., Share financial data outside designated channels]

---

## Error Handling

| Error | Response |
|-------|----------|
| [e.g., MCP server unreachable] | [e.g., Retry once after 30s; if still failing, alert human and skip this run] |
| [e.g., Data format unexpected] | [e.g., Log raw data, send partial results with disclaimer] |
| [e.g., Permission denied] | [e.g., Escalate to IT/Security immediately] |

---

## Examples

### Good Output

```
[Example of a correct, well-formatted skill execution result]
```

### Bad Output (What to Avoid)

```
[Example of incorrect output and why it's wrong]
```

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | [Name] | Initial skill creation |
