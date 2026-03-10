---
name: incident-ingesting
description: >
  Reads a Jira ticket that describes a production incident or discovered issue, extracts all
  relevant context, and triggers the Incident Commander with a structured intake package.
  Use this skill when a developer shares a Jira ticket key or URL (e.g., "OPS-412 is a P1",
  "look at PROD-88", "there's a Jira for this incident") instead of pasting raw logs or
  symptoms. Requires the Atlassian MCP to be configured. Produces the same Triage-ready
  context block that a manual incident report would produce, then hands off to the full
  incident response flow.
tools: Bash, Read, Write, Task
mcp_servers:
  - name: atlassian
    url: https://mcp.atlassian.com/v1/mcp
---

# Jira Incident Intake Skill

Your job is to bridge an existing Jira ticket to the Incident Commander's response flow.
A ticket already exists — meaning someone already identified the problem. Your goal is to
extract everything the incident response pipeline needs so it can proceed **without asking
the developer to repeat information that's already in Jira**.

---

## Phase 0: Resolve the Ticket Reference

Accept any of these input forms:
- Bare key: `OPS-412`, `PROD-88`, `INC-7`
- Full URL: `https://company.atlassian.net/browse/OPS-412`
- Natural language: "the Jira for the checkout outage", "look at the incident ticket"

If a bare key or URL is provided, extract the issue key and proceed directly to fetch.
If the reference is ambiguous (e.g., "the checkout Jira"), use Jira search to find the
most likely match before fetching.

---

## Phase 1: Fetch the Ticket

Use the Atlassian MCP (`getJiraIssue`) to retrieve the full ticket. Request these fields:

| Field | Purpose |
|---|---|
| `summary` | Short incident title |
| `description` | Full symptom description, error messages, logs pasted by the reporter |
| `priority` | Jira priority → use to inform initial severity hint |
| `status` | Current workflow state (helps assess if incident is active or resolved) |
| `issuetype` | Bug, Incident, Task — affects routing |
| `labels` | May include `incident`, `severity-p0`, `production`, `security`, etc. |
| `components` | Affected services/systems |
| `assignee` | Who currently owns the ticket |
| `reporter` | Who filed it |
| `created` | When the ticket was created (proxy for incident onset if no better signal) |
| `updated` | Last activity — signals if investigation is recent or stale |
| `comment` | Comments thread — often contains diagnostic steps already taken, stack traces, and timeline |
| `attachment` | Log files, screenshots, exported metrics |
| `customfield_*` | Any custom fields: environment, affected region, linked runbook, SLA fields |

Also fetch linked issues (`getJiraIssueRemoteIssueLinks`) to identify:
- Linked PRs (hotfixes in progress)
- Duplicate or related incidents
- Parent epics (for service context)

---

## Phase 2: Extract Incident Context

Map ticket data to the standard incident intake fields. Fill every field — use "unknown"
rather than leaving anything blank.

### Priority mapping

Use the Jira priority as a **hint only** — do not blindly trust it. Jira priorities are
often set by reporters who may not follow the severity matrix. The Incident Commander will
classify severity properly during triage.

| Jira Priority | Severity Hint |
|---|---|
| Blocker / Critical | Likely P0 or P1 — flag immediately |
| Major | Likely P1 or P2 |
| Minor | Likely P2 or P3 |
| Trivial | Likely P3 |
| Not set | Unknown — defer to triage |

### Status mapping

| Jira Status | Signal |
|---|---|
| Open / To Do | Incident may not yet have an active responder |
| In Progress | Someone is actively working it — check assignee and comments for context |
| Resolved / Done | Incident is over — route to `incident-documenting` for postmortem/summary only |
| Reopened | Regression — treat as new active incident |

### Context extraction rules

- **Symptom**: Pull from `summary` + first paragraph of `description`. If description contains
  raw error messages, stack traces, or log excerpts — include them verbatim in the handoff.
- **When**: Check `created` timestamp, but also scan `description` and early comments for
  phrases like "started at", "began around", "first noticed at". Use the most specific time.
- **Scope**: Look for user counts, error rates, affected regions, or service names in the
  description and comments. Extract them explicitly.
- **Trigger**: Scan for mentions of deploys, config changes, dependency upgrades, traffic
  spikes, or cron jobs that correlate with onset.
- **Actions taken**: Read the full comment thread. Extract any diagnostic steps, attempted
  fixes, and their outcomes into a bullet list. This prevents the team from retrying failed
  approaches.
- **Attachments**: Note any attached log files or exports. If the AI assistant can read them,
  extract key error signals and include in the handoff package.

---

## Phase 3: Build the Intake Package

Produce a structured intake package that the Incident Commander consumes as if a developer
had reported it directly. Do not summarize or lose fidelity — include the raw error messages
and stack traces verbatim.

```
JIRA INCIDENT INTAKE
────────────────────
Source Ticket   : [KEY] — [title]
Ticket URL      : [https://...]
Status          : [Jira status]
Reporter        : [name] at [timestamp]
Assignee        : [name or "unassigned"]
Priority Hint   : [Jira priority] → estimated [P0/P1/P2/P3/unknown]

Stage           : [prod | staging | ci | local | unknown]
Symptom         : [precise observable symptom extracted from description]
Raw Error       :
  [Verbatim error messages, stack traces, or log excerpts from ticket — or "none in ticket"]

When            : [Onset time from ticket — or "unknown, ticket created at HH:MM UTC"]
Scope           : [Users/services/endpoints affected — or "unknown"]
Trigger         : [Deployment, config change, dependency, traffic spike — or "unknown"]

Actions Already Taken:
  [Bullet list from comment thread — or "none documented in ticket"]
  - [action] → [outcome]
  - [action] → [outcome]

Linked Context  :
  - PRs: [links or "none"]
  - Related incidents: [links or "none"]
  - Runbook: [link or "none referenced"]

Attachments     : [list filenames or "none"]
```

---

## Phase 4: Route to Incident Commander

After building the intake package, pass it to the Incident Commander with a clear handoff note.

**If the ticket status is Resolved/Done:**
> "Ticket [KEY] is already resolved. Routing to `incident-documenting` to generate postmortem
> and close the documentation loop. Skipping triage and RCA."
→ Invoke `incident-documenting` directly.

**If the ticket status is active (Open, In Progress, Reopened):**
> "Ticket [KEY] is an active incident. Handing full context to the Incident Commander to begin
> the response flow."
→ Pass the intake package to the Incident Commander. The Commander will invoke `incident-triaging`
as the first step, using the extracted context instead of asking the developer to re-enter it.

**If priority hint is Blocker/Critical:**
> Flag immediately: "Jira priority is [Blocker/Critical] — treating as potential P0/P1. Proceeding
> to triage at high urgency."

---

## Phase 5: Jira Ticket Hygiene (optional, if MCP write is available)

After the response flow completes, offer to update the Jira ticket with:
- Severity classification from triage (add label `severity-p<N>`)
- Link to escalation brief, runbook, or postmortem artifact written to disk
- Status transition if appropriate (e.g., move to "In Progress" if it was "Open")

Always ask before writing back to Jira — do not update tickets automatically.

---

## Constraints

- **Never ask for information already in the ticket.** Read the full description and comment
  thread before determining what context is missing.
- **Preserve raw error messages verbatim.** Summarizing stack traces loses signal. Include
  them in the intake package exactly as they appear.
- **Do not classify severity yourself.** Provide a hint based on Jira priority, but defer
  final severity classification to `incident-triaging`.
- **Comments are part of the incident record.** A ticket with 20 comments may have the root
  cause already identified. Read them all before handing off.
- **Stale tickets need a freshness check.** If the ticket was last updated >24 hours ago and
  is still "In Progress", flag this to the developer before proceeding — the incident state
  may have changed without the ticket being updated.
