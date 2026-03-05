---
name: incident-analyzing
description: >
  Root cause analysis and impact assessment for active incidents. Use this skill after triage
  has classified severity, or directly when a developer pastes a stack trace, error log, or
  describes unexpected system behavior and wants to understand WHY it's happening. Applies
  structured causal hypothesis testing — not correlation — to identify the origin of failures
  in distributed systems, monoliths, CI pipelines, or local dev environments. Reads topology
  and failure pattern references to match symptoms to known failure classes. Produces a
  confirmed causal chain and blast radius assessment ready for handoff to remediation.
  Trigger when the developer says things like "why is this happening", "what's the root cause",
  "help me debug this", or pastes a raw stack trace.
context: fork
agent: Plan
allowed-tools: Read, Grep, Bash, Run
---

# Incident Root Cause Analysis Skill

Your job is to move from symptom to cause — not just to note that two things happened at the
same time, but to establish that one caused the other. You do this through structured hypothesis
testing, reading actual code and logs, and eliminating competing explanations until one remains.

---

## RCA Methodology

Work through these four steps explicitly. Don't skip to conclusions.

### Step 1: Form a hypothesis

State the most probable root cause based on the symptom, timing, and any context provided.
Be specific — name the service, function, query, dependency, or config value you suspect.

> **Example:** "Hypothesis A: DB connection pool exhausted due to a connection leak introduced in
> `UserService.fetchProfile()` in the v2.4.1 deploy 2 hours ago."

Vague hypotheses like "it might be a DB issue" are not useful — they don't tell you what evidence
to look for next.

### Step 2: Identify discriminating evidence

For each hypothesis, specify what telemetry would **confirm** it and what would **refute** it.
Then go get that evidence — read the log file, run the diagnostic command, read the source file.

```
Hypothesis A — DB connection pool exhaustion
  Confirms: pool utilization metric at 100%; pg_stat_activity shows idle connections holding
  Refutes:  pool utilization below 80%; errors started immediately at deploy (not hours later)
```

Never theorize about code you can read directly.

### Step 3: Test and update

State explicitly whether the evidence confirms or refutes the hypothesis.

> "The pool utilization metric shows only 60% — this **refutes** Hypothesis A. The error
> timeline shows the spike began immediately at deploy, not hours later. Updating to
> **Hypothesis B: ORM lazy loading behavior changed in the SQLAlchemy 1.4→2.0 upgrade
> bundled in this deploy, causing N+1 queries on the `/api/user` endpoint.**"

Keep cycling until one hypothesis survives all available evidence.

### Step 4: Write the causal chain

Once converged, output the causal chain in this format. Every arrow must be a causal step,
not a correlation.

```
[SQLAlchemy 2.0 upgrade in deploy v2.4.1]
  → [lazy loading disabled by default — relationship queries no longer batched]
    → [N+1 queries on every /api/user call — 1 query per user record instead of 1 total]
      → [DB CPU at 100%, query latency 8s average]
        → [HTTP 504s for all authenticated endpoints]
          → [checkout flow broken for all logged-in users]
```

---

## Stack Trace Analysis

When given a stack trace or error log:

1. **Identify the exception type** — what class of error is this? (OOM, NPE, connection error, timeout, deserialization failure, deadlock)
2. **Find the origin frame** — the first frame in the developer's own code, not framework or library internals. That's where investigation starts.
3. **Classify the error pattern** — see the table below for known signatures
4. **Determine novelty** — new error (never appeared before) or regression (was working, now broken)?
5. **Extract correlation/trace IDs** — if present, use them to link log lines across services into a single timeline

**In minified JS environments:** note that `vendor.js:L:C` coordinates require source map resolution to identify the actual file and line. Flag this and ask for the `.map` file or unminified source if available.

### Known error pattern signatures

| Exception / Log Pattern | Most Likely Cause | First Diagnostic Step |
|---|---|---|
| `connection pool timeout` / `too many connections` | Connection pool exhaustion or leak | Check pool utilization metric over time; look for missing `close()` in error paths |
| `OOMKilled` / `Java heap space` / `heap out of memory` | Memory leak or undersized limit | Plot memory metric since last deploy — linear climb = leak, step change = new allocation |
| `deadlock found` / `lock wait timeout exceeded` | DB lock contention | Check for long-running transactions or missing index on a recently changed query |
| `certificate has expired` / `SSL handshake failed` | Cert expiry | `openssl s_client -connect <host>:443` — check `notAfter` field |
| `no such host` / `name resolution failed` | DNS / service discovery failure | `nslookup <service>`, check k8s service and endpoints |
| `upstream timeout` / `504 Gateway Timeout` | Slow downstream dependency | Find the slow span in distributed trace; test latency directly with `curl -w "%{time_total}"` |
| `FATAL ERROR: Allocation failed` (Node.js) | V8 heap exhausted | Check `--max-old-space-size` setting; profile heap with `--inspect` |

---

## Microservices Topology Reasoning

When the incident spans multiple services, read `references/topology-patterns.md` for detailed
failure signatures and diagnosis commands.

Core approach:
- Use correlation/trace IDs to link log lines across services into a unified timeline
- Identify the **origin service** (first to show the error) vs **propagation services** (downstream victims that appear broken but are actually just receiving failures)
- The origin service is where RCA focuses — propagation services self-heal once the origin is fixed
- Draw a simplified call graph with the failure point annotated; this helps identify the blast radius

---

## Phase 3: Impact Assessment

Before handing off to remediation, quantify the blast radius. This determines which remediation
options are acceptable (a risky rollback is warranted for P0 data loss; it's not warranted for
a P3 cosmetic issue).

```
IMPACT ASSESSMENT
─────────────────
Users Affected  : [number or % of traffic — be specific about which user segment]
Revenue Path    : [yes — [flow name] | no]
Error Budget    : [X% remaining this month | already breached | unknown]
Downstream      : [services that will fail or degrade if this continues]
Data Integrity  : [safe | at risk — [what data] | compromised — [what data]]
```

Answer this explicitly: **Is a revenue-generating flow currently broken?** This single question
drives the P0 vs P1 boundary more than anything else.

---

## Reference Files

Load these during investigation — don't try to recall failure patterns from memory:

- `references/topology-patterns.md` — Diagnostic signatures, discriminating evidence, and exact
  Bash/SQL commands for 10 common distributed systems failure classes. Read when you need to
  match a symptom to a known pattern.
