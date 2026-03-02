---
name: sourcing-from-atlassian
description: "Retrieval procedures for fetching user stories, epics, acceptance criteria, and Confluence pages from Atlassian via MCP. Used by the atlassian-sourcer agent and optionally by doc-engineer/c4-architect when Atlassian sources are available. Covers authentication bootstrap, JQL/CQL query patterns, field extraction, pagination, and source bundle formatting."
context: fork
agent: Plan
mcp_servers:
  - name: atlassian
    url: https://mcp.atlassian.com/v1/mcp
---

# Sourcing from Atlassian — MCP Retrieval Skill

## Overview

This skill defines the exact procedures for retrieving documentation source material from Jira and Confluence via the Atlassian MCP server. It covers connection setup, query patterns, field extraction maps, pagination, and the canonical source bundle format consumed by the doc-engineer and c4-architect agents.

---

## 1. Connection Bootstrap

Before any retrieval, establish the Atlassian context. Do this once per session.

```
Step 1: Call getAccessibleAtlassianResources
  → Returns: list of { id (cloudId), name, url, scopes }
  → Select the cloudId that matches the user's workspace

Step 2: Call atlassianUserInfo
  → Confirms identity and available permissions
  → If permissions are insufficient, surface the specific missing scope to the user

Step 3: Store cloudId in working context for all subsequent calls
```

If the user provides a Jira URL (e.g., `https://myorg.atlassian.net`), extract the subdomain as the workspace identifier and confirm it matches a cloudId from step 1.

---

## 2. Jira Retrieval Patterns

### 2a. Fetch a Single Issue

**Tool:** `getJiraIssue`  
**Input:** `{ issueKey: "PROJ-123", cloudId }`  
**Extract these fields:**

| Field | Jira path | Purpose |
|-------|-----------|---------|
| Summary | `fields.summary` | Issue title |
| Description | `fields.description` (Atlassian Document Format → plain text) | Body content |
| Status | `fields.status.name` | Is this story still active? |
| Priority | `fields.priority.name` | Informs documentation urgency |
| Issue type | `fields.issuetype.name` | Story / Bug / Epic / Task |
| Acceptance Criteria | `fields.customfield_10016` OR look for "Acceptance Criteria" heading in description | Core doc input |
| Labels | `fields.labels[]` | Feature tagging |
| Epic link | `fields.customfield_10014` OR `fields.parent.key` (Next-gen) | Upward traceability |
| Linked issues | `fields.issuelinks[]` | Related items |
| Assignee | `fields.assignee.displayName` | SME to contact for gaps |
| Reporter | `fields.reporter.displayName` | Original requester |
| Created / Updated | `fields.created`, `fields.updated` | Staleness check |
| Fix version | `fields.fixVersions[]` | Release mapping |
| Attachments | `fields.attachment[]` | Supplementary files |
| Comments (last 5) | `fields.comment.comments[-5:]` | Late-breaking decisions |

**Acceptance Criteria extraction rules:**
1. Check `customfield_10016` first (standard AC field in many Jira configs)
2. If empty, scan the issue description for a section headed "Acceptance Criteria", "AC", "Given/When/Then", or a numbered list immediately following "Definition of Done"
3. If still empty, mark as `[MISSING AC]` in the gap report

### 2b. Search Issues by JQL

**Tool:** `searchJiraIssuesUsingJql`

**Common JQL patterns for documentation sourcing:**

```jql
-- All stories in an epic (classic projects)
"Epic Link" = PROJ-42 AND issuetype = Story ORDER BY created ASC

-- All stories in an epic (next-gen / team-managed)
issueType = Story AND parentEpic = PROJ-42

-- All issues in a sprint
project = PROJ AND sprint in openSprints() ORDER BY rank ASC

-- All issues for a release
project = PROJ AND fixVersion = "v2.1.0" ORDER BY issuetype ASC

-- Recent changes to a feature area
project = PROJ AND labels = "auth-service" AND updated >= -30d

-- Completed stories for release notes
project = PROJ AND issuetype = Story AND status = Done 
  AND fixVersion = "v2.1.0" ORDER BY priority ASC

-- All bugs in scope
project = PROJ AND issuetype = Bug AND status != Done 
  AND affectedVersion = "v2.0.0"
```

**Pagination:**
- Request `maxResults: 50` per call
- If `total > startAt + maxResults`, increment `startAt` and repeat
- Cap total retrieval at 200 issues; if more exist, ask the user to narrow scope

**Field projection:** Always request `fields=summary,description,status,priority,issuetype,customfield_10016,labels,issuelinks,assignee,reporter,created,updated,fixVersions,customfield_10014,parent`

### 2c. Fetch an Epic with All Children

```
1. getJiraIssue(epicKey) → store epic summary, description, goal
2. searchJiraIssuesUsingJql('"Epic Link" = <epicKey>') → classic projects
3. searchJiraIssuesUsingJql('parentEpic = <epicKey>') → next-gen projects
4. Merge results, deduplicate by issue key
5. For each child story: extract fields per 2a
```

---

## 3. Confluence Retrieval Patterns

### 3a. Fetch a Single Page

**Tool:** `getConfluencePage`  
**Input:** `{ pageId: "123456", cloudId }`

**Extract:**

| Field | Purpose |
|-------|---------|
| `title` | Page heading |
| `space.key` and `space.name` | Namespace for traceability |
| `body.storage.value` (HTML/ADF) or `body.view.value` | Full content |
| `version.number` and `version.when` | Staleness check |
| `version.by.displayName` | Last editor (SME) |
| `ancestors[]` | Page hierarchy context |
| `children.page[]` (if any) | Sub-pages to recursively fetch |

**Content cleaning:**
- Strip HTML tags to markdown-equivalent structure: headings → `##`, `<table>` → markdown table, `<code>` → fenced code block
- Preserve macro outputs (info panels, expand blocks) as blockquotes with `> [INFO]` prefix
- Remove navigation elements, breadcrumbs, footer macros
- Preserve all technical content verbatim — do not summarize

### 3b. Search Pages by CQL

**Tool:** `searchConfluenceUsingCql`

**Common CQL patterns:**

```cql
-- Find pages by title keyword in a specific space
type = page AND space = "ENG" AND title ~ "authentication"

-- Find recently modified pages in a space
type = page AND space = "ENG" AND lastModified >= "2024-01-01"

-- Find pages related to a Jira epic
type = page AND text ~ "PROJ-42"

-- Find pages with a specific label
type = page AND label = "api-design" AND space = "ENG"

-- Find all pages in a space updated this quarter
type = page AND space = "ENG" AND lastModified >= startOfQuarter()

-- Find design documents
type = page AND title ~ "design" AND space in ("ENG","ARCH")
```

**Pagination:**
- Use `limit: 25` per call
- Check `_links.next` for additional pages
- Cap at 50 pages; if more exist, ask user to narrow

### 3c. Map Jira Issues to Confluence Pages

After retrieving Jira issues, find associated Confluence pages:

```
For each Jira issue key (e.g., PROJ-123):
  1. searchConfluenceUsingCql('text ~ "PROJ-123"')
  2. Also check issue links for "Confluence Page" remote links via getJiraIssueRemoteIssueLinks
  3. Add found pages to the traceability index
```

---

## 4. Field Mapping to Documentation Concepts

Use this table to map Atlassian fields to documentation sections:

| Atlassian field | Documentation section |
|---|---|
| Epic summary + description | Feature overview / introduction |
| Story summary | Feature name / section heading |
| Acceptance criteria | Functional requirements, expected behavior |
| Description body | Background, context, technical notes |
| Labels | Tags, categories, audience hints |
| Fix version | Release notes scope |
| Linked bugs | Known issues, limitations |
| Confluence page body | Architecture context, design rationale, specs |
| Confluence page ancestors | Document hierarchy, related guides |
| Comments (resolved) | Historical decisions, rationale (use sparingly) |

---

## 5. Staleness and Quality Checks

Run these checks on every retrieved item before including in the bundle:

| Check | Condition | Action |
|---|---|---|
| Story without AC | `customfield_10016` is null AND no AC section in description | Flag `[MISSING AC]` in gap report |
| Stale Confluence page | `lastModified` > 90 days AND related Jira stories are active | Flag `[STALE - verify]` |
| Closed story in scope | `status.name in ["Done", "Closed", "Won't Do"]` | Include but mark `[CLOSED]` — may be release notes source |
| Contradiction | Two sources disagree on same fact | Flag `[CONTRADICTION]` with both sources cited |
| Missing AC definition | Story is "In Progress" or "Done" but has no AC | Flag as high-priority gap |
| Version mismatch | Fix version on story doesn't match Confluence page's stated version | Flag `[VERSION MISMATCH]` |

---

## 6. Source Bundle Format

Every invocation of this skill must produce output in exactly this format:

```markdown
# Atlassian Source Bundle

**Generated:** <ISO 8601 timestamp>
**Cloud:** <cloudId> (<workspace name>)
**Scope:** <brief description of what was fetched>
**Total Jira issues:** <n>  
**Total Confluence pages:** <n>

---

## Jira Sources

### Epic: <KEY> — <Summary>
- **Status:** <status>
- **Fix Version:** <version or "unset">
- **Description:**  
  <cleaned epic description>
- **Business Goal:**  
  <goal field or extracted from description>

---

### User Stories

#### <KEY>: <Summary>
- **Type:** <issuetype>  
- **Status:** <status> | **Priority:** <priority>
- **Assignee:** <name or "unassigned">
- **Fix Version:** <version>
- **Labels:** <label1>, <label2>

**Acceptance Criteria:**
1. <criterion>
2. <criterion>

**Description:**  
<cleaned description body>

**Linked Issues:** <KEY1 (Blocks)>, <KEY2 (Relates to)>

**Notes from Comments:**  
<relevant resolved comments — skip trivial/status updates>

---

[Repeat per story]

---

## Confluence Sources

### <Page Title>
- **Space:** <SPACE_KEY> | **Page ID:** <id>
- **Last Modified:** <date> by <author name>
- **URL:** <full URL>
- **Version:** <n>

**Content:**

<cleaned, markdown-formatted page body>

---

[Repeat per page]

---

## Gap Report

| # | Severity | Location | Issue | Recommended Action |
|---|----------|----------|-------|--------------------|
| 1 | 🔴 Blocker | KEY-123 | No acceptance criteria | Request from Product Owner before drafting |
| 2 | 🟡 Warning | Confluence "Auth Design" | Not updated in 4 months | Verify still accurate with page owner |
| 3 | 🟡 Warning | KEY-456 vs "API Spec v3" | Contradiction: AC says JWT, spec says session cookie | Resolve before writing auth docs |
| 4 | ⚪ Info | KEY-789 | Status is "Done" — use for release notes only | Include in release notes section |

---

## Traceability Index

| Jira Key | Summary | Status | Confluence Pages |
|----------|---------|--------|-----------------|
| KEY-123 | User login | In Progress | "Auth Design", "Login Flow Spec" |
| KEY-456 | Dashboard | Done | — |

---

## Recommended Next Step

<One sentence recommending which doc-engineer action to invoke next and why.>
For example: "Pass this bundle to doc-engineer using authoring-user-docs — the acceptance criteria map directly to tutorial steps for the login flow."
```

---

## 7. Error Handling

| Error | Response |
|---|---|
| MCP connection failure | Report: tool name, parameters, error message. Do not retry silently. |
| Issue not found (404) | Mark as `[NOT FOUND: KEY-XXX]` in the bundle. Continue with remaining items. |
| Permission denied (403) | Report which resource requires elevated access. Surface to user. |
| Rate limit (429) | Wait per `Retry-After` header; note delay in bundle header. |
| Empty search results | Return bundle with empty sections and note the JQL/CQL used, so the user can adjust. |
| ADF parse failure | Include raw content with `[RAW - parse failed]` prefix. |
