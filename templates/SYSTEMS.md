# System Documentation Template

> **Instructions:** Use this template to document any software system, tool, or platform used by your organization. This documentation helps AI agents understand how to interact with systems and helps humans troubleshoot issues.

---

## System: [System Name]

**Category:** [CRM / ERP / Communication / Database / Custom / etc.]
**Vendor:** [Vendor name or "Internal"]
**Last Updated:** [Date]
**Document Owner:** [Name]
**Version:** 1.0

---

## 1. Overview

### Purpose
[What business function does this system serve?]

### Users
| User Group | Use Case | Access Level |
|------------|----------|--------------|
| [Group 1] | [What they use it for] | [Admin/Editor/Viewer] |
| [Group 2] | [What they use it for] | [Admin/Editor/Viewer] |

### Business Criticality
- **Criticality Level:** [High/Medium/Low]
- **Impact if unavailable:** [What breaks if this system is down?]
- **RTO (Recovery Time Objective):** [Target time to restore]

---

## 2. Access & Authentication

### Access Request Process
[How does someone get access to this system?]

### Authentication Method
- [ ] Username/Password
- [ ] SSO (Provider: _______)
- [ ] MFA Required
- [ ] API Keys
- [ ] OAuth

### Service Account for AI Agents
- **Account name:** [If applicable]
- **Permission level:** [What can the service account do?]
- **Key rotation schedule:** [How often are credentials rotated?]

---

## 3. Key Features & Capabilities

### Feature 1: [Feature Name]
**Purpose:** [What does this feature do?]
**How to access:** [Navigation path or command]
**Common use cases:** [When would someone use this?]

### Feature 2: [Feature Name]
**Purpose:** [What does this feature do?]
**How to access:** [Navigation path or command]
**Common use cases:** [When would someone use this?]

<!-- Add more features as needed -->

---

## 4. Data Model

### Key Entities
| Entity | Description | Key Fields |
|--------|-------------|------------|
| [Entity 1] | [What it represents] | [Important fields] |
| [Entity 2] | [What it represents] | [Important fields] |

### Relationships
[Describe how entities relate to each other]

### Data Retention
- **Retention period:** [How long is data kept?]
- **Archival process:** [What happens to old data?]

---

## 5. Integration Points

### Incoming Data
| Source System | Data Type | Method | Frequency |
|---------------|-----------|--------|-----------|
| [System 1] | [What data] | [API/File/Manual] | [Real-time/Daily/etc.] |

### Outgoing Data
| Destination | Data Type | Method | Frequency |
|-------------|-----------|--------|-----------|
| [System 1] | [What data] | [API/File/Manual] | [Real-time/Daily/etc.] |

### API Information
- **API Documentation:** [Link]
- **Base URL:** [URL]
- **Authentication:** [Method]
- **Rate Limits:** [Limits if any]

---

## 6. AI Agent Integration

### Available for Agent Use?
- [ ] Yes - Full access
- [ ] Yes - Read only
- [ ] Yes - With human approval
- [ ] No - Not integrated
- [ ] Planned

### MCP Server
- **Server name:** [If applicable]
- **Available tools:** [List of tools/functions]
- **Documentation:** [Link to MCP server docs]

### Agent Capabilities
| Action | Allowed? | Approval Required? |
|--------|----------|-------------------|
| Read data | [Yes/No] | [Yes/No] |
| Create records | [Yes/No] | [Yes/No] |
| Update records | [Yes/No] | [Yes/No] |
| Delete records | [Yes/No] | [Yes/No] |
| [Custom action] | [Yes/No] | [Yes/No] |

---

## 7. Common Operations

### Operation 1: [Operation Name]
**When needed:** [Trigger or schedule]
**Steps:**
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Operation 2: [Operation Name]
**When needed:** [Trigger or schedule]
**Steps:**
1. [Step 1]
2. [Step 2]

---

## 8. Troubleshooting

### Common Issue 1: [Issue Description]
**Symptoms:** [What does the user see?]
**Cause:** [Why does this happen?]
**Resolution:** [How to fix it]

### Common Issue 2: [Issue Description]
**Symptoms:** [What does the user see?]
**Cause:** [Why does this happen?]
**Resolution:** [How to fix it]

### Support Escalation
- **Level 1 (Internal):** [Who to contact]
- **Level 2 (Vendor):** [Support contact/portal]
- **Emergency:** [Emergency contact process]

---

## 9. Related Resources

- **Vendor documentation:** [Link]
- **Internal training:** [Link]
- **Related processes:** [Links to PROCESSES.md files]
- **Related systems:** [Links to other SYSTEMS.md files]

---

## 10. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | [Name] | Initial version |

---

*Template version: 1.0 | JBOT Protocol*
