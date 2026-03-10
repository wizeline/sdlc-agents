---
name: incident-documenting
description: >
  Generates production-ready incident documentation artifacts: escalation briefs, Jira tickets,
  postmortem drafts, and incident timeline summaries. Use this skill when a developer needs to
  communicate an incident to stakeholders, create a tracking ticket, escalate to another team,
  or document what happened after resolution. Trigger on: "draft the Jira ticket", "write the
  postmortem", "I need to escalate this", "write up what happened", "create an incident report",
  or at the end of any P0/P1 resolution flow. Also invoked in parallel with triage for P0/P1
  incidents to generate an escalation brief before root cause is known. Artifacts are saved to
  disk as markdown files ready for copy-paste into Jira, Confluence, PagerDuty, or email.
---

# Incident Artifacts Skill

Your job is to produce the written artifacts that turn an incident from a chaotic event into
a documented, trackable, learnable organizational record. These artifacts serve three audiences:
the team resolving the incident right now (escalation brief), the engineering org tracking it
(Jira ticket), and the broader organization learning from it (postmortem).

Every artifact produced by this skill is saved to disk — nothing stays only in chat.

---

## Artifact 1: Escalation Brief

**When to produce:** Any time a P0 or P1 incident needs to be handed to another team, a manager
needs to be informed, or the on-call engineer needs to page someone. Generate this *before* root
cause is confirmed if severity warrants it.

Save to: `.docs/escalation-<YYYYMMDD-HHMM>.md`

```
ESCALATION BRIEF
─────────────────
Incident    : [Short title — specific enough to distinguish from other incidents]
Severity    : P[N] / SEV[N]
Time        : [HH:MM UTC — when incident started]
Duration    : [How long this has been active]

What's broken:
  [One paragraph: observable symptom, affected users/services, business impact]

What's been tried:
  [Bullet list of actions taken and their outcomes — be specific, not "we looked at things"]
  - Checked DB connection pool → utilization at 98%, not the cause
  - Rolled back deploy v2.4.1 → no change in error rate
  - Restarted UserService pods → temporary relief, error rate returned after ~10min

Current hypothesis:
  [Best current theory about root cause — or "unknown" if no hypothesis yet]

What we need:
  [Specific ask from the escalation target — not "help", but "we need X to do Y"]
  - DBA access to run `pg_locks` queries on prod DB
  - AWS support to check for RDS-level connection limits

Escalate to: [on-call lead | platform team | security | DBA | vendor support]
Contact via: [PagerDuty policy name | Slack channel | phone]
```

---

## Artifact 2: Jira Ticket

**When to produce:** After any incident (P0–P3) once the symptom is understood well enough to
describe it clearly. For P0/P1, create during or immediately after resolution. For P2/P3,
create when triaged.

Save to: `.docs/jira-<YYYYMMDD-HHMM>.md`

```markdown
**Issue Type:** Bug / Incident
**Priority:** Blocker / Critical / Major / Minor
**Title:** [Specific and searchable — avoid "prod is down"
           Good: "UserService connection pool leak causes checkout 503s after ~2h uptime"
           Bad: "Production incident 2024-01-15"]
**Components:** [Service name(s)]
**Labels:** incident, severity-p<N>[, postmortem-needed][, security][, data-integrity]
**Fix Version:** [hotfix tag or next release]
**Linked PRs:** [if known]

---

## Summary
[2–3 sentences: what broke, when, who was affected, business impact.
No jargon that a PM couldn't parse.]

## Root Cause
[The full causal chain — be specific. Name the commit, function, query, or dependency.
"A bug in authentication" is not a root cause. "Connection leak in UserService.fetchProfile()
introduced in commit a3f92b1 (deploy v2.4.1) caused pool exhaustion after ~2 hours of uptime"
is a root cause.]

## Timeline
| Time (UTC) | Event |
|---|---|
| HH:MM | Incident started (first error / alert fired) |
| HH:MM | On-call engineer paged / developer noticed |
| HH:MM | [Key diagnostic step] |
| HH:MM | Root cause identified |
| HH:MM | Fix deployed / rollback executed |
| HH:MM | Error rate returned to baseline |
| HH:MM | Incident resolved |

## Resolution
[What was done to restore service — the actual commands or changes, not just "we fixed it".]

## Follow-up Actions
- [ ] Add regression test: [specific scenario that was missing coverage]
- [ ] Add monitoring alert: [specific signal that would have caught this earlier]
- [ ] Audit [related code/service] for the same class of issue
- [ ] Update runbook: [path to runbook]
- [ ] Schedule postmortem (required for P0/P1)

## Prevention
[What architectural or process change prevents this class of incident from recurring.
Be specific — "add more tests" is not prevention. "Add connection pool utilization alert
at 80% threshold with PagerDuty integration" is prevention.]
```

---

## Artifact 3: Postmortem Draft

**When to produce:** After any P0 or P1 incident, or any incident where significant user or
revenue impact occurred. Generate the draft immediately after resolution — details fade fast.

Save to: `docs/postmortem-<YYYYMMDD>-<slug>.md`

The goal of a postmortem is **blameless learning** — understanding the systemic conditions that
allowed the incident to occur, not assigning fault to individuals.

```markdown
# Postmortem: <Incident Title>

**Date:** YYYY-MM-DD
**Severity:** P[N]
**Duration:** [N hours N minutes — from first alert to resolution]
**Author(s):** [Names]
**Status:** Draft / In Review / Final
**Reviewed by:** [Names — fill in after review meeting]

---

## Impact
[Quantified user and business impact:
- N users affected for N hours
- Revenue impact: estimated $N (or "unknown, under investigation")
- SLA: [met / breached by N minutes]
- Error budget: consumed N% of monthly budget]

## Timeline
[Same timeline as Jira ticket — copy/expand with more detail here]

## Root Cause
[Full causal chain — more detailed than the Jira version. Explain the technical mechanism
in enough depth that an engineer unfamiliar with the system understands it.]

## Contributing Factors
[Systemic conditions that allowed this to happen — these are the things to fix:
- No alert existed for connection pool utilization above 80%
- The deployment checklist didn't include DB migration review
- Source map files weren't deployed alongside minified JS, slowing stack trace resolution
These are not excuses — they're the actual targets for prevention work.]

## What Went Well
[Actions that limited the impact or accelerated resolution:
- On-call engineer was paged within 2 minutes of the alert firing
- Feature flag was in place, enabling partial mitigation without a rollback
- Runbook existed for this failure class and was accurate]

## What Went Poorly
[Honest assessment — blameless, but specific:
- 45 minutes elapsed before root cause was identified because DB metrics weren't in the runbook
- Rollback procedure required manual steps not documented anywhere
- Escalation path was unclear — three engineers each thought another was the primary owner]

## Action Items
| Action | Owner | Due | Priority |
|---|---|---|---|
| Add connection pool alert at 80% | [Name] | YYYY-MM-DD | P1 |
| Update deployment checklist | [Name] | YYYY-MM-DD | P2 |
| Add regression test for fetchProfile | [Name] | YYYY-MM-DD | P2 |
| Update runbook with DB metric steps | [Name] | YYYY-MM-DD | P2 |

## Lessons Learned
[2–3 transferable insights for the broader engineering org — things other teams might apply.]
```

---

## Artifact 4: Incident Summary (async communication)

**When to produce:** When the developer needs to communicate incident status to stakeholders
asynchronously — Slack, email, status page.

Save to: `.docs/summary-<YYYYMMDD-HHMM>.md`

```markdown
**[RESOLVED] <Service Name> <Symptom> — <Date>**

**Status:** Resolved at HH:MM UTC
**Duration:** N hours N minutes
**Impact:** [Users/services affected, what was broken]

**What happened:**
[2–3 sentences: root cause in plain language]

**What we did:**
[Brief list of resolution steps]

**Current state:**
[All systems normal / monitoring for recurrence / follow-up work in progress]

**Next steps:**
- Jira ticket: [link]
- Postmortem scheduled: [date]
```

---

## Output Discipline

- Save every artifact to disk immediately — use file write tools
- Check for existing `docs/`, `runbooks/` directories before creating new ones
- Fill every field, even with "unknown" — blank fields in incident documentation are worse than
  approximate information
- Timelines must be in UTC with explicit timezone — "around 3pm" is not acceptable in incident docs
- Postmortem action items must have an owner and a due date — unowned action items don't get done
