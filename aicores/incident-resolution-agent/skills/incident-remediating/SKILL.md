---
name: incident-remediating
description: >
  Remediation planning and execution for active incidents. Use this skill once a root cause is
  known (or probable) and the team needs to act: rollback a deployment, kill a circuit breaker,
  scale out pods, patch a CVE, generate a code fix with a regression test, or draft a hotfix PR.
  Also use when a developer asks "how do I fix X" for a known incident type — connection pool
  exhaustion, OOM, slow downstream, DB lock contention, cert expiry, etc. — even without a
  full RCA. Produces ordered remediation options (fastest to restore service → safest → most
  permanent), exact copy-pasteable commands for the developer's environment, and a complete
  PR description artifact. Trigger on: "how do I fix this", "how do I roll back", "what's
  the remediation", "help me patch this CVE", or when handing off from incident-rca.
context: fork
agent: Plan
---

# Incident Remediation Skill

Your job is to turn a confirmed (or probable) root cause into concrete action. You produce
remediation options ordered by speed and safety, generate exact commands for the developer's
actual environment, and create PR artifacts that are ready to submit.

Always lead with the **fastest path to service restoration**, even if it's not the permanent fix.
Fixing the symptom now and the root cause later is the right call when users are actively affected.

---

## Remediation Ordering Principle

For every incident, recommend options in this order:

```
1. Runtime / operational (minutes, no code change)
   → Rollback, feature flag, circuit breaker, scale out, cache flush, pool reset
   → Fastest to execute, easiest to revert, lowest risk

2. Configuration (minutes to hours, no deploy needed)
   → Timeout adjustments, pool size changes, rate limit tuning
   → Medium risk — test in staging if possible, but can be applied to prod in P0/P1

3. Code fix + hotfix deploy (hours)
   → Targeted fix in the implicated function/module
   → Higher risk — requires review, test, deploy cycle

4. Architectural remediation (days to weeks)
   → Add circuit breaker, add caching layer, refactor the problematic pattern
   → Out of scope for active incident — schedule as follow-up
```

Never skip straight to a code fix if a runtime remediation can restore service first. The code fix
can follow once the bleeding has stopped.

---

## Runtime Remediations

Read `references/remediation-patterns.md` for exact commands. The patterns covered are:

- **Kubernetes rollback** — restore prior deployment revision
- **Feature flag disable** — kill a misbehaving feature without a deploy
- **DB connection pool recovery** — kill idle connections, restart leaking pods, tune pool size
- **Circuit breaker activation** — stop sending traffic to a failing downstream
- **Horizontal scaling** — add replicas to absorb load (only if bottleneck is in this service)
- **Cache invalidation** — flush stale or corrupted cache entries
- **Secret / config rotation recovery** — propagate a newly rotated credential
- **Dependency CVE patching** — assess, upgrade, and PR

**Always infer the developer's platform from context** before producing commands — read `k8s/`,
`docker-compose.yml`, `.github/workflows/`, `Procfile`, or deployment config files. Don't generate
generic AWS commands if they're clearly running on k8s.

**State the risk and revert path** before every command. Engineers need to know what could go wrong
before they run something in production.

---

## Code Fix Generation

When root cause is localized to a specific function or module:

1. **Read the actual file** at the implicated location — never generate a fix for code you haven't read
2. **Show the problematic block** with an inline explanation of the failure mechanism (the why, not just the what)
3. **Generate the corrected code** with comments explaining the reasoning
4. **Write a regression test** that would have caught this before it reached production
5. **Note edge cases** the fix doesn't address — be explicit about the scope of the fix

Apply the fix directly with file edit tools unless the developer asks to
review first.

---

## PR Artifact

For every code fix, produce a complete PR description. Save it to `.docs/hotfix-<YYYYMMDD-HHMM>.md`

```markdown
## fix(<scope>): <imperative verb> <what>

Example title: fix(auth): resolve connection pool leak in UserService.fetchProfile

### Problem
[What broke, when it started, user impact — one paragraph. Name specific metrics or error rates.]

### Root Cause
[Full causal chain — name the specific commit, function, query, or dependency. Not "there was a bug".]

### Fix
[What changed and why it resolves the root cause. Explain the mechanism, not just the diff.]

### Testing
[How this was validated:
- Unit test added: `<test name>` covers `<specific scenario>`
- Load test: ran `<N>` requests, pool utilization stayed below `<X>%`
- Manual repro: reproduced the original failure, confirmed fix resolves it]

### Rollback
[Exact steps to revert:
- kubectl: `kubectl rollout undo deployment/<n>`
- Or: revert commit <sha> and push]

**Labels:** `incident` `hotfix` `severity-p<N>`
**Reviewers:** on-call lead + [domain owner]
**Linked issue:** [Jira ticket ID]
```

---

## Runbook Generation

For incidents that are likely to recur, generate a runbook and save to `runbooks/<incident-type>.md`.
Base the runbook on what was actually needed to diagnose and resolve this specific incident — don't
write a generic template, write what an on-call engineer at 3am would need.

```markdown
# Runbook: <Incident Type>

**Last updated:** <YYYY-MM-DD>
**Default severity:** P[N]
**Owner:** <team or service>

## When to use this runbook
[Specific observable symptoms — precise enough that an on-call engineer can pattern-match
at 3am without prior context. Name the exact error message, alert name, or metric threshold.]

## Escalate to P[N-1] if
[Conditions that push this beyond the default severity:]
- [e.g., data integrity questions arise]
- [e.g., not resolved within 30 minutes]

## Diagnosis Steps
1. Check [specific metric] in [specific dashboard URL or command]
   — expect to see [specific value or pattern] if this runbook applies
2. Run: `<exact command>` — look for: `<output pattern>`
3. Confirm: [how to know you've correctly identified the issue]

## Remediation

### Option A: Fast path ([estimated time])
[Use when: <condition>]
1. `<exact command with filled-in values>`
   Risk: [what could go wrong]
   Revert: `<revert command>`
2. Verify: [what to check to confirm it worked]

### Option B: Root fix ([estimated time])
[Use when: Option A didn't work or root cause is confirmed]
1. [Step]
2. Verify: [what to check]

## Verification
[Exact metric threshold, log pattern, or endpoint response that confirms full resolution.
Not "check if it's working" — name the specific thing to look for.]

## Escalation
If not resolved in [N] minutes: page [team] via [PagerDuty policy / Slack channel]

## Post-incident
- [ ] File Jira ticket (link to template in `incident-artifacts` skill)
- [ ] Update this runbook with anything missing or wrong
- [ ] Schedule postmortem if P0 or P1
```

---

## Reference Files

- `references/remediation-patterns.md` — 9 playbooks with exact commands, risk warnings, and
  revert paths for the most common incident remediation types. Load this whenever generating
  runtime remediation steps — use the playbook commands verbatim rather than generating from memory.
