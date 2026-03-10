# Incident Resolution Agent — Prompt Examples

Ready-to-use prompts for the **Incident Support** AI Core.
This core includes **1 agent** backed by **4 skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent automatically classifies severity, runs root cause analysis, generates remediation
> commands for your environment, and saves all artifacts to disk.

---

## Agent → Skill Quick Reference

| Agent | Skills Coordinated |
| --- | --- |
| `incident-commander` | `incident-triaging`, `incident-analyzing`, `incident-remediating`, `incident-documenting` |

### Skill Scope

| Skill | What It Does |
| --- | --- |
| `incident-triaging` | Intake + P0–P3 severity classification, blast radius estimate, SLA risk |
| `incident-analyzing` | Hypothesis-driven root cause analysis, causal chain, impact assessment |
| `incident-remediating` | Runtime fixes, code fix generation, PR artifact, runbook |
| `incident-documenting` | Escalation briefs, Jira tickets, postmortem drafts, async summaries |

---

## Artifact Outputs Quick Reference

| Artifact | Saved To |
| --- | --- |
| Triage Block | Chat (inline) |
| Causal Chain + Impact Assessment | Chat (inline) |
| Escalation Brief | `.docs/escalation-<YYYYMMDD-HHMM>.md` |
| Hotfix PR Description | `.docs/hotfix-<YYYYMMDD-HHMM>.md` |
| Jira Ticket Draft | `.docs/jira-<YYYYMMDD-HHMM>.md` |
| Runbook | `runbooks/<incident-type>.md` |
| Postmortem Draft | `docs/postmortem-<YYYYMMDD>-<slug>.md` |
| Incident Summary | `.docs/summary-<YYYYMMDD-HHMM>.md` |

---

## Triage Only

> **Persona:** On-call engineer with a raw signal who needs an immediate severity call before doing anything else.

```text
We just got an alert — checkout is returning 503s. Started about 15 minutes ago.
No deploys today. What severity is this and what do I need to establish before escalating?
```

```text
Is this a P0?

Error rate on /api/payments spiked to 18% 4 minutes ago. This is the revenue-critical path.
All other endpoints look normal. DB metrics look fine.
```

```text
CI is completely broken — every pipeline is failing with the same error:

  ERROR: Cannot connect to Docker daemon at tcp://docker:2375

Affects all PRs. No one can merge. What's the severity here?
```

```text
We have a partial outage. Authentication is failing for ~40% of users.
Payment and checkout appear fine. Started 8 minutes ago. No recent deploys.
What P level is this and what's the escalation path?
```

---

## Root Cause Analysis

> **Persona:** Engineer with a triage complete or a stack trace in hand who needs to understand *why* something broke.

### From a stack trace

```text
Help me find the root cause. This started 20 minutes ago in production:

[paste stack trace]
```

```text
Why is this happening? I've been staring at this for an hour:

[paste error log or stack trace]
```

```text
Our error rate spiked right after the v2.4.1 deploy. Here's what I'm seeing in the logs.
Walk me through what's actually failing and why:

[paste log output]
```

### From a symptom description

```text
Memory on the UserService pods is climbing linearly since the last deploy.
OOMKilled twice in the past hour. No memory limit was changed. What's causing this?
```

```text
DB queries on /api/orders are running at 8 seconds average. Baseline was 120ms.
The query hasn't changed. What failure classes should I rule out and how?
```

```text
We upgraded SQLAlchemy from 1.4 to 2.0 this morning and now every authenticated
endpoint is timing out. Error rate is 100% on those endpoints. What changed in that
upgrade that could cause this, and what evidence do I look for to confirm?
```

### Microservices / distributed system

```text
We have a cascade across three services. OrderService, InventoryService, and
NotificationService all started failing at the same time. I have distributed trace IDs
but I don't know which service is the origin. Here are the logs from all three:

[paste logs with trace IDs]
```

```text
Service mesh is showing elevated latency from UserService → PaymentService.
Both services look healthy independently. Trace ID: abc123. Help me find the origin.
```

---

## Remediation

> **Persona:** Engineer who knows (or suspects) the root cause and needs exact steps to restore service.

### Known failure classes

```text
Our DB connection pool is maxed out again — this is the third time this week.
We're on PostgreSQL 15, k8s, with a Node.js backend. Give me the fastest path
to restore service and what I need to fix so this stops recurring.
```

```text
We need to roll back the last deployment immediately. We're on Kubernetes.
The deployment is called user-service. Walk me through the exact commands
and tell me what to check after the rollback completes.
```

```text
A downstream payment processor dependency is timing out and taking our
checkout flow down with it. We need to stop sending traffic there now
without a redeploy. What are my options?
```

```text
We have a certificate expiry — SSL handshake is failing on api.example.com.
Our cert is managed in AWS ACM. Give me the exact steps to diagnose,
rotate, and verify the fix.
```

```text
CVE-2024-21626 affects runc, which we use on EKS. Assess the severity
for our setup and walk me through patching it with minimal downtime.
```

### Code fix

```text
The root cause is a connection leak in UserService.fetchProfile() — it opens
a DB connection in the error path but never closes it. Here's the function:

[paste source code]

Generate the fix, a regression test that would have caught this, and a
hotfix PR description ready to submit.
```

```text
N+1 queries are killing our DB. The issue is in this ORM relationship:

[paste source code]

Fix it, add a test that validates the query count, and generate the PR artifact.
```

### Runbook generation

```text
We just resolved a connection pool exhaustion incident for the third time.
Write a runbook for this failure class so the next on-call engineer can
diagnose and fix it without context — include exact commands, what metrics
to check, and escalation criteria.
```

---

## Documentation & Artifacts

> **Persona:** Engineer at the end of an incident who needs to communicate, track, and learn from what happened.

### Escalation brief (before root cause is known)

```text
We have a P1 — checkout is down for 30% of users and we've been at it for 45 minutes
without a root cause. I need to escalate to the platform team right now.
Draft an escalation brief I can send immediately with what we know so far.

What we've tried:
- Restarted the checkout pods — no change
- Checked DB connections — utilization at 60%, not the issue
- Rolled back the last deploy — error rate unchanged
```

### Jira ticket

```text
The incident is resolved. Here's what happened:

- Connection leak in UserService introduced in deploy v2.4.1
- Affected 2,400 users for 1h 20min, checkout fully down
- Fixed by patching fetchProfile() and deploying hotfix v2.4.2
- Resolved at 14:32 UTC

Create a Jira ticket with full timeline, root cause, resolution steps, and follow-up actions.
```

### Postmortem draft

```text
We just wrapped a P1 incident. I need a postmortem draft ready for tomorrow's review meeting.

Incident summary:
- What broke: connection pool exhaustion in UserService caused 100% checkout failure
- Duration: 1h 20min
- Users affected: ~2,400
- Root cause: missing connection close in error path of fetchProfile(), introduced in v2.4.1
- Resolution: hotfix + pod restart
- What went well: alert fired within 2 min, runbook existed (but was missing DB metrics)
- What went poorly: 45 min to identify root cause, rollback didn't work (connection leak persisted)

Draft the full postmortem with contributing factors, timeline, and action items with owners.
```

### Async status update

```text
The incident is resolved. Write a Slack/email update I can send to stakeholders.
Keep it non-technical — the audience is product and leadership, not engineers.

What happened: Checkout was down for ~80 minutes due to a database connection issue.
Fixed: deployed a patch. All systems normal now. Postmortem scheduled for Thursday.
```

---

## Full Pipeline — All Skills End to End

These prompts trigger the complete flow: triage → root cause analysis → remediation → documentation.

```text
Prod is down. Payment API is returning 500s for 100% of requests.
Started 6 minutes ago. Last deploy was 2 hours ago. Here's the error log:

[paste error log or stack trace]
```

```text
We have an active incident. Here's everything I know:

- Service: checkout-service (Node.js, ECS Fargate)
- Symptom: 504s on POST /checkout for ~60% of requests
- Started: 14:05 UTC (12 minutes ago)
- Last change: config update to downstream payment service timeout values at 13:50 UTC
- Error rate climbing: 12% → 35% → 61% over 12 minutes

Triage it, find the root cause, give me remediation options, and draft the escalation brief —
I need to page the platform team in the next 5 minutes.
```

```text
Something broke during the migration. Here's what I'm seeing:

[paste logs, error messages, or stack trace]

Walk me through the full response: severity classification, what's actually failing and why,
how to fix it, and what documentation I need to file once it's resolved.
```

---

## Short Version — Full Coverage in One Prompt

The following prompt is designed to exercise **every skill** in the Incident Support AI Core.
Adapt the service names, stack, and incident details to your environment.

```text
We have an active production incident. I need the full response.

System context:
- Service: user-auth-service (Python/FastAPI, Kubernetes on EKS)
- Database: PostgreSQL 15 via RDS
- Monitoring: Datadog + PagerDuty
- Last deploy: v3.1.2 — 2 hours ago, included an ORM version bump (SQLAlchemy 1.4 → 2.0)

Incident:
- Alert fired at 09:42 UTC: "auth-service 5xx error rate > 10%"
- Current error rate: 73% and climbing on POST /auth/login and GET /auth/profile
- All other endpoints normal
- DB CPU at 95%, query latency at 8s average (baseline: 120ms)
- No config changes, no traffic spike, no downstream service issues
- Error started ~2 min after deploy completed

Error log sample:
  [09:42:11] ERROR sqlalchemy.exc.TimeoutError: QueuePool limit of size 10 overflow 20
             reached, connection timed out, timeout 30
  [09:42:11] ERROR GET /auth/profile → 503 Service Unavailable (request_id: req_abc123)
  [09:42:12] ERROR POST /auth/login → 503 Service Unavailable (request_id: req_def456)

Please do the following in order:

1. Classify severity (P0–P3) and estimate blast radius. State SLA risk.
   I need to know within 60 seconds whether to page leadership.

2. Find the root cause. Form hypotheses, identify what evidence confirms or refutes each,
   and produce the full causal chain. Read the codebase if you need to.

3. Give me remediation options ordered by speed. Start with the fastest path to restore
   service — runtime/operational first, code fix second. Include exact kubectl commands
   for our k8s environment and the rollback path for each option.
   If a code fix is needed, generate it with a regression test.

4. While remediation is in progress, draft an escalation brief I can send to the
   platform team now — before root cause is fully confirmed.

5. After resolution, generate:
   - Hotfix PR description (saved to .docs/)
   - Jira ticket with full timeline and follow-up actions
   - Postmortem draft with contributing factors, what went well/poorly, and action items
   - A runbook for this failure class so the next on-call engineer can resolve it in under 15 minutes
```

### What This Prompt Activates

| Step | Skill | Output |
| ---- | ----- | ------ |
| 1. Severity classification | `incident-triaging` | Triage Block (P0–P3, blast radius, SLA risk) |
| 2. Root cause analysis | `incident-analyzing` | Causal chain + Impact Assessment |
| 3. Remediation | `incident-remediating` | Runtime commands + code fix + regression test |
| 4. Escalation brief | `incident-documenting` | `.docs/escalation-<timestamp>.md` |
| 5a. Hotfix PR | `incident-remediating` | `.docs/hotfix-<timestamp>.md` |
| 5b. Jira ticket | `incident-documenting` | `.docs/jira-<timestamp>.md` |
| 5c. Postmortem | `incident-documenting` | `docs/postmortem-<date>-<slug>.md` |
| 5d. Runbook | `incident-remediating` | `runbooks/<incident-type>.md` |

---

## Tips for Better Results

1. **Paste the raw signal immediately** — stack traces, error logs, alert names, metric values.
   The agent starts working from whatever you give it and asks for missing context in parallel,
   so don't wait until you've gathered everything.

2. **Name your infrastructure** — mention Kubernetes, ECS, Lambda, RDS, Redis, etc.
   Remediation commands are generated for your actual platform, not a generic one.
   "We're on k8s" gets you `kubectl rollout undo`; without it you get a question.

3. **Anchor the timeline** — "started 8 minutes ago" or "right after the v2.4.1 deploy at 14:05 UTC"
   is the single most useful piece of context for root cause analysis. Correlation with a deploy
   or config change dramatically narrows the hypothesis space.

4. **State revenue impact explicitly** — "this is the checkout flow" or "payment processing is affected"
   triggers immediate P1 classification. If you don't say it, the agent may need to ask.

5. **Say what you already tried** — listing failed remediation attempts (e.g., "restarted the pods,
   no change") skips hypothesis paths that have already been ruled out and keeps RCA focused.

6. **Ask for what you need at the end** — "generate the PR artifact", "write the postmortem",
   "save a runbook" produces those artifacts. Without explicit asks for documentation,
   you'll get chat output rather than files on disk.

7. **For P3s, say so** — "this is a low-priority cosmetic bug, just give me a Jira ticket"
   skips root cause analysis and remediation entirely, going straight to the ticket.

8. **CVE reports get full treatment** — paste the CVE identifier and your runtime environment
   (e.g., "CVE-2024-21626 — we run runc on EKS") to get severity assessment,
   patching commands, and a Jira ticket in one pass.
