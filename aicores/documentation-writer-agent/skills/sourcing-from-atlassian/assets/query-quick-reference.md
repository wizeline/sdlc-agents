# Atlassian Sourcing — Query Quick Reference

This asset is loaded by the `sourcing-from-atlassian` skill and `atlassian-sourcer` agent
as a lookup table for common retrieval scenarios.

---

## JQL Patterns (Jira)

### Documentation source retrieval

```jql
-- User stories for a specific epic (classic Jira)
"Epic Link" = <EPIC-KEY> AND issuetype = Story ORDER BY priority ASC

-- User stories for a specific epic (next-gen / team-managed)
parent = <EPIC-KEY> AND issuetype = Story ORDER BY rank ASC

-- All issues in a fix version (release notes source)
project = <PROJECT> AND fixVersion = "<version>" 
  ORDER BY issuetype ASC, priority DESC

-- Active sprint stories
project = <PROJECT> AND sprint in openSprints() AND issuetype = Story

-- Feature area by label
project = <PROJECT> AND labels in ("<label>") AND issuetype in (Story, Epic)

-- Done stories in the last sprint (retrospective / docs)
project = <PROJECT> AND sprint in closedSprints() AND status = Done
  AND issuetype = Story ORDER BY updated DESC

-- All bugs affecting a version
project = <PROJECT> AND issuetype = Bug AND affectedVersion = "<version>"
  AND status != Done

-- Issues assigned to a component
project = <PROJECT> AND component = "<component>" AND issuetype = Story

-- Issues with missing acceptance criteria (custom field empty)
project = <PROJECT> AND cf[10016] is EMPTY AND issuetype = Story
  AND status != Done

-- Issues updated in the last N days (freshness check)  
project = <PROJECT> AND updated >= -<N>d AND issuetype in (Story, Bug, Epic)
```

---

## CQL Patterns (Confluence)

### Documentation source retrieval

```cql
-- Pages by title keyword in a space
type = page AND space = "<SPACE_KEY>" AND title ~ "<keyword>"

-- Pages containing a Jira issue key
type = page AND text ~ "<JIRA-KEY>"

-- Pages in a specific space updated recently
type = page AND space = "<SPACE_KEY>" AND lastModified >= "yyyy-MM-dd"

-- Pages with a specific label
type = page AND label = "<label>" AND space = "<SPACE_KEY>"

-- All pages under a parent page (section)
type = page AND parent = <PARENT_PAGE_ID>

-- Architecture / design documents
type = page AND title ~ "design" AND space in ("<ENG_SPACE>","<ARCH_SPACE>")

-- API design pages
type = page AND label in ("api-design","openapi","swagger")

-- ADR pages
type = page AND (title ~ "ADR" OR title ~ "Architecture Decision")

-- Pages mentioning a feature name
type = page AND text ~ "<feature name>" AND space = "<SPACE_KEY>"
  ORDER BY lastModified DESC

-- Release notes pages
type = page AND title ~ "Release Notes" AND space = "<SPACE_KEY>"
  ORDER BY title DESC
```

---

## Field ID Reference (Jira custom fields)

| Field Name | Standard Field ID | Common Custom Field ID |
|---|---|---|
| Acceptance Criteria | — | `customfield_10016` |
| Story Points | — | `customfield_10028` |
| Epic Link (classic) | — | `customfield_10014` |
| Epic Name | — | `customfield_10011` |
| Sprint | — | `customfield_10020` |
| Team | — | `customfield_10002` |

> **Note:** Custom field IDs vary by Jira instance. If `customfield_10016` returns empty,
> use `getJiraIssueTypeMetaWithFields` to discover the actual AC field ID for the project.

---

## Common Scoping Patterns

| Documentation type | Jira scope | Confluence scope |
|---|---|---|
| Feature user guide | All stories in epic | Space pages labelled with feature |
| API reference | Stories with label "api" or component "API" | API design pages, OpenAPI spec pages |
| Release notes | All Done issues in fixVersion | Release notes page in ENG space |
| Architecture doc | Epic + linked technical stories | Architecture space ADR + design pages |
| Onboarding guide | "Onboarding" epic or label | Getting started pages |
| ADR | Single decision story or spike | Existing ADR pages for context |
| Changelog | All issues in closed sprint or fixVersion | Changelog page |
