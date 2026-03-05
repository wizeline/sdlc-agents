---
name: incident-triaging
description: >
  Incident intake and severity classification for SDLC engineers. Use this skill the moment
  an incident is reported — before any analysis begins. Triggers on: stack traces, error logs,
  mentions of outage, degraded service, spiking error rate, failed deploy, alert firing, or any
  "something is broken" signal across all SDLC stages (local, CI, staging, prod). Produces a
  structured Triage Block with P0–P3 severity, blast radius estimate, SLA risk, and the minimum
  context needed to hand off to root cause analysis. Trigger even if the developer just pastes
  a raw log line with no explanation — that's always a triage conversation.
---

# Incident Triage Skill

Your job is to take the first, often incomplete report of something broken and rapidly produce two
things: a **severity classification** and the **minimum viable context** needed to begin root cause
analysis. Speed matters here — every minute of ambiguity at the triage stage costs time downstream.

This skill covers all SDLC stages: local dev, CI/CD, staging/QA, and production.

---

## Phase 0: Intake

When an incident is first reported, extract what you can immediately from the raw input — don't
ask questions you can already answer from what's been provided.

Minimum context to establish before handing off:

| Field | What to look for |
|---|---|
| **Stage** | Keywords: local, CI, staging, prod, pipeline, review app |
| **Symptom** | The observable signal: error message, metric name, user complaint, alert title |
| **When** | Onset time, or correlation with a deploy/config change |
| **Scope** | Users affected, % of traffic, which services, which endpoints |
| **System context** | Language, framework, infra (k8s, ECS, Lambda), key services involved |

If the developer pastes a stack trace, error log, or raw error message — **begin analysis immediately**
and ask for missing context in parallel. Never make them wait while you ask for things you don't yet need.

If you have enough to classify severity, do it — you can refine later.

---

## Phase 1: Triage & Severity Classification

Classify every incident using this matrix. Severity determines urgency, escalation path, and which
remediation options are on the table. Getting this wrong in either direction is costly: over-triaging
burns the team; under-triaging delays the response.

```
P0 — Critical / SEV1
  Conditions (any one is sufficient):
  - Complete outage of a customer-facing system
  - Data loss or corruption actively in progress
  - Active security breach or unauthorized access
  - SLA breach imminent in <15 minutes
  Response: Immediate — page everyone now, all hands, no async

P1 — High / SEV2
  Conditions (any one is sufficient):
  - Major feature broken for a significant percentage of users
  - Performance degraded >2x baseline latency on a critical path
  - Payment, auth, or billing flows impaired (even partially)
  - Error rate >5% above baseline and climbing
  Response: 30-minute SLA, on-call engineer takes ownership

P2 — Medium / SEV3
  Conditions:
  - Non-critical feature broken, workaround exists
  - Error rate elevated but below SLA threshold
  - Staging environment broken, blocking releases
  - CI pipeline broken, blocking merges
  Response: 4-hour SLA, owned in current sprint

P3 — Low / SEV4
  Conditions:
  - Cosmetic issue, no functional impact
  - Single flaky test in CI (not a pattern)
  - Developer local environment issue
  - Low-CVSS vulnerability with no active exploit
  Response: Backlog — Jira ticket, no paging
```

### Escalation triggers to watch for during triage

Even before RCA, flag these conditions immediately:
- Any mention of **data being written** to a system that's behaving incorrectly → potential data integrity risk → escalate
- **Auth or session tokens** involved in the error path → security implication, treat as P1 minimum
- **Third-party payment processor** in the error path → revenue impact, treat as P1 minimum
- Incident started **during a maintenance window or migration** → coordinated failure, higher severity

---

## Output: Triage Summary Block

Produce this block for every incident before handing off to RCA. It is the contract between triage
and the next phase — fill every field, even if with "unknown."

```
INCIDENT TRIAGE
───────────────
Severity    : P[0-3]
Stage       : [local | ci | staging | prod]
Symptom     : [one precise sentence — not "it's broken", e.g., "HTTP 503s on /api/checkout for ~30% of users"]
Blast Radius: [e.g., "~2,000 users, checkout flow only" or "all CI builds blocked" or "unknown"]
SLA Risk    : [yes — breach in ~N min | no | unknown]
Started     : [HH:MM UTC or "unknown — no timestamp in report"]
Trigger     : [deployment | config change | traffic spike | dependency outage | unknown]
Next Step   : [immediate escalation | begin RCA | monitor for 10min | backlog]
```

---

## Handoff

After producing the Triage Block:

- **P0/P1**: Pass the full context to `incident-analyzing` immediately. State: "Handing off to RCA — time is critical."
- **P2**: Pass to `incident-analyzing` with a note that the 4-hour window gives room for methodical investigation.
- **P3**: Pass to `incident-documenting` to generate a Jira ticket. No RCA needed unless pattern repeats.

If the developer needs to page someone right now (P0/P1), also invoke `incident-documenting` in parallel
to generate an escalation brief they can send immediately.
