# Incident Support Agent — AI Core

> **AI Core** | Incident Management & Rapid Response | From triage to resolution, escalation briefs to postmortems.

The **Incident Support Agent** is a comprehensive incident management AI Core. It covers the full incident lifecycle: rapid triage and severity classification, systematic root cause analysis, remediation planning and execution, and production-ready documentation artifacts.

---

## Architecture Overview

```
incident-support-agent/
├── agents/
│   └── incident-commander-agent.md      ← Multi-phase incident orchestration
└── skills/
    ├── incident-triaging/                ← Intake + P0–P3 classification
    ├── incident-analyzing/               ← Root cause analysis & hypothesis testing
    ├── incident-remediating/             ← Remediation options + PR artifacts
    └── incident-documenting/             ← Escalation briefs, Jira, postmortems
```

---

## MCP Connections

The Incident Support Agent integrates with external systems via MCP servers:

- **Atlassian** — Create/update Jira tickets, Confluence pages, postmortems
- **GitHub** — Link PRs, commits, and remediation artifacts to incidents

All skills operate on logs, stack traces, code, and architecture descriptions you provide directly. MCP connections are optional for advanced integrations.

---

## Agents

### `incident-commander`

**Description:** Multi-phase incident orchestrator. Takes the first report of something broken (stack trace, error log, outage report) and routes it through specialized skills in the right sequence: triage → RCA → remediation → documentation. When the report is too vague to triage (e.g., "something is broken" with no error or context), runs a structured **Phase 0 Intake** — asking for the affected system, symptoms, timing, recent changes, and user impact — all in one prompt. Chains skill outputs together and maintains clarity about what's been done and what's next.

**Triggers:** `"we have an incident"`, `"prod is down"`, `"something is broken"`, `"there's an outage"`, `"SEV1"`, or any description of system failure

**Skills loaded:** `incident-triaging`, `incident-analyzing`, `incident-remediating`, `incident-documenting`

**Example workflows:**
```
# Pasted stack trace → full incident flow
incident-triaging → incident-analyzing → incident-remediating → incident-documenting

# P0 outage — time critical
incident-triaging → incident-analyzing + incident-documenting [escalation brief] → incident-remediating

# Known issue, need the fix
incident-remediating (directly — skip triage/RCA)

# Post-incident documentation only
incident-documenting (directly)

# P3 low-priority issue
incident-triaging → incident-documenting [Jira ticket only]
```

---

## Skills

### `incident-triaging`

**Description:** Incident intake and rapid severity classification for all SDLC stages (local dev, CI, staging, prod). Produces a structured Triage Block with P0–P3 severity, blast radius estimate, SLA risk, and minimum context for handoff to root cause analysis.

**Severity Levels:**
| Level | Name | Response SLA | Condition |
|-------|------|--------------|-----------|
| P0 | Critical / SEV1 | Immediate | Complete outage, data loss, active breach, or SLA breach in <15 min |
| P1 | High / SEV2 | 30 minutes | Major feature broken, 2x latency degradation, payment/auth flows impaired, >5% error rate |
| P2 | Medium / SEV3 | 4 hours | Non-critical feature broken (workaround exists), elevated error rate, staging/CI pipeline broken |
| P3 | Low / SEV4 | Backlog | Cosmetic issue, flaky test, local environment issue, low-CVSS vulnerability with no active exploit |

**Output:** Structured triage block containing severity, stage, symptom, blast radius, SLA risk, trigger event, and next step

**Reference files:** None (intake only)

---

### `incident-analyzing`

**Description:** Systematic root cause analysis using structured hypothesis testing. Moves from symptom to confirmed cause through evidence gathering and hypothesis validation. Identifies the origin service vs. propagation services in distributed systems, draws causal chains, and produces impact assessment.

**RCA Methodology:**
1. **Form hypothesis** — Name the specific service, function, query, or config that likely caused the failure
2. **Identify discriminating evidence** — What telemetry would confirm vs. refute each hypothesis
3. **Test and update** — Gather evidence from logs, metrics, code, and update hypothesis accordingly
4. **Write causal chain** — Every arrow represents a causal step, not correlation

**Error Pattern Recognition:**
| Pattern | Most Likely Cause | Diagnostic Step |
|---------|-------------------|-----------------|
| `connection pool timeout` / `too many connections` | Connection pool exhaustion/leak | Check pool utilization metric; look for missing `close()` |
| `OOMKilled` / `heap out of memory` | Memory leak or undersized limit | Plot memory metric since last deploy |
| `deadlock found` | DB lock contention | Check long-running transactions and missing indexes |
| `certificate has expired` | Cert expiry | `openssl s_client -connect <host>:443` |
| `no such host` / `name resolution failed` | DNS / service discovery failure | `nslookup <service>`, check k8s service/endpoints |
| `upstream timeout` / `504 Gateway Timeout` | Slow downstream dependency | Find slow span in distributed trace |
| `FATAL ERROR: Allocation failed` (Node.js) | V8 heap exhausted | Check `--max-old-space-size`, profile heap with `--inspect` |

**Output:** Confirmed causal chain with supporting evidence and impact assessment (users affected, revenue path, data integrity, downstream services)

**Reference files:**
- `references/topology-patterns.md` — Diagnostic signatures and commands for 10+ common distributed systems failure classes

---

### `incident-remediating`

**Description:** Remediation planning and execution. Turns a confirmed (or probable) root cause into concrete action. Produces remediation options ordered by speed-to-recovery and safety, generates exact copy-pasteable commands for the developer's environment, and creates complete PR artifacts.

**Remediation Ordering:**
```
1. Runtime / operational (minutes, no code change)
   → Rollback, feature flag, circuit breaker, scale out, cache flush, pool reset
   → Fastest, easiest to revert, lowest risk

2. Configuration (minutes to hours, no deploy needed)
   → Timeout adjustments, pool size changes, rate limit tuning
   → Medium risk — test in staging if possible

3. Code fix + hotfix deploy (hours)
   → Targeted fix in implicated function/module
   → Higher risk — requires review, test, deploy

4. Architectural remediation (days/weeks)
   → Add circuit breaker, caching layer, refactor pattern
   → Out of scope for active incident — schedule as follow-up
```

**Covered Remediations:**
- Kubernetes rollback and deployment recovery
- Feature flag disable (kill feature without deploy)
- DB connection pool recovery (kill idle connections, restart pods, tune pool size)
- Circuit breaker activation and downstream failure mitigation
- Horizontal scaling (add replicas)
- Cache invalidation (flush stale/corrupted entries)
- Secret/config rotation recovery
- Dependency CVE patching and supply chain remediation

**Output artifacts:**
- Ordered remediation options with risk/revert guidance for each
- Exact copy-pasteable commands (inferred from platform: k8s, Docker, AWS, etc.)
- Complete PR description with problem, root cause, fix, testing, rollback, and labels
- Runbook for recurring incident types (saved to `runbooks/`)

**Reference files:**
- `references/remediation-patterns.md` — 9 playbooks with exact commands, risk warnings, and revert paths for common incident types

---

### `incident-documenting`

**Description:** Produces production-ready incident artifacts for communication and organizational learning. Generates escalation briefs (for immediate stakeholder notification), Jira tickets (for tracking and audit), postmortem drafts (for blameless learning), and incident summary messages (for async communication).

**Artifacts produced:**

| Artifact | When | Audience | Saved to |
|----------|------|----------|----------|
| **Escalation Brief** | P0/P1 incidents, immediate escalation needed | On-call leads, managers, escalation targets | `.docs/escalation-<YYYYMMDD-HHMM>.md` |
| **Jira Ticket** | After any incident (P0–P3) | Engineering team, audit trail | `.docs/jira-<YYYYMMDD-HHMM>.md` |
| **Postmortem Draft** | After P0/P1 incidents | Entire engineering org, learning | `docs/postmortem-<YYYYMMDD>-<slug>.md` |
| **Incident Summary** | Async communication needed | Stakeholders, status page, email | `.docs/summary-<YYYYMMDD-HHMM>.md` |

**Escalation Brief includes:** Incident title, severity, time, duration, what's broken, what's been tried, current hypothesis, specific escalation needs

**Jira Ticket includes:** Issue type, priority, title, components, labels, summary, root cause, timeline, resolution, follow-up actions, prevention measures

**Postmortem includes:** Impact quantification, timeline, root cause, contributing factors, what went well, what went poorly, action items with owners and due dates, lessons learned

**Incident Summary includes:** Status, duration, impact, what happened, what was done, current state, next steps

**Output discipline:** All artifacts saved to disk immediately; every field filled (even "unknown" is better than blank); timelines in UTC with explicit timezone; postmortem action items have owners and due dates

---

## Installation

### Install the full AI Core (all agents)
```bash
npx aicore-cli add https://github.com/wizeline/sdlc-agents/tree/main/aicores/incident-support-agent
```

### Install a single agent
```bash
npx aicore-cli add https://github.com/wizeline/sdlc-agents/tree/main/aicores/incident-support-agent/agents/incident-commander-agent
```

### Install a single skill
```bash
npx aicore-cli add https://github.com/wizeline/sdlc-agents/tree/main/aicores/incident-support-agent/skills/incident-triaging
```

---

## Typical Usage

<<<<<<< HEAD
### An engineer reports a vague incident with no context

```
incident-commander will:
  1. Run Phase 0 Intake → Ask for affected system, symptoms, timing, recent changes, user impact
  2. Once user responds, invoke incident-triaging → Classify severity
  3. Invoke incident-analyzing → Form hypothesis, gather evidence, write causal chain
  4. Invoke incident-remediating → Show fast remediation options + code fix if needed
  5. Invoke incident-documenting → Generate escalation brief (if P0/P1) + Jira ticket
```

### An engineer pastes a stack trace with no context
```
incident-commander will (stack trace is sufficient context — no intake needed):
=======
### An engineer pastes a stack trace with no context
```
incident-commander will:
>>>>>>> 5bcf311 (docs: created a comprehensive README.md for the incident-support-agent AI Core that mirrors the structure and detail level of the security-agent README.)
  1. Invoke incident-triaging → Classify severity (likely P1 or P2)
  2. Invoke incident-analyzing → Form hypothesis, gather evidence, write causal chain
  3. Invoke incident-remediating → Show fast remediation options + code fix if needed
  4. Invoke incident-documenting → Generate escalation brief (if P0/P1) + Jira ticket
```

### Production is down, time is critical (P0)
```
incident-commander will:
  1. Invoke incident-triaging → Confirm P0 immediately
  2. Invoke incident-analyzing + incident-documenting (in parallel)
     - RCA proceeds to identify root cause
     - Escalation brief is drafted and ready to send NOW
  3. Once RCA converges, invoke incident-remediating → Fast remediation options
  4. After restore, invoke incident-documenting → Full postmortem and Jira ticket
```

### Developer knows the issue, needs the fix
```
incident-remediating directly:
  "Our DB connections are exhausted again, what do I do?"
  → Remediation skill produces the exact commands to fix it
  → No need for full triage/RCA if you already understand the problem
```

---

## Key Design Principles

1. **Triage everything** — False positives are cheap; missed incidents are not. When in doubt, classify severity.
2. **Speed to restoration before permanent fix** — Fast runtime remediation now, root cause code fix later.
3. **No ghost artifacts** — Every document produced (escalation brief, Jira ticket, postmortem) is written to disk immediately. Nothing stays only in chat.
4. **Evidence-based RCA** — Never theorize about code you can read. Gather discriminating evidence, test hypotheses, converge on cause.
5. **Escalation clarity** — Be explicit about phase transitions (triage → RCA → remediation). Incident management is stressful; clarity reduces cognitive load.
6. **Blameless learning** — Postmortems focus on systemic conditions, not individual blame. The goal is preventing future incidents, not assigning fault.

## ToDos

- [ ] Add new skill to read a JIRA ticket so the incident-commander can be triggered
- [x] Make the principal agent behave interactive with the user, so it can ask for what to document and provide questions as instructions to execute
