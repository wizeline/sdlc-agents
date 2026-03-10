# Distributed Systems Failure Patterns

## Table of Contents
1. [Connection Pool Exhaustion](#connection-pool-exhaustion)
2. [Cascading Failure / Thundering Herd](#cascading-failure)
3. [Memory Leak / OOM](#memory-leak-oom)
4. [Deployment Regression](#deployment-regression)
5. [Slow Downstream Dependency](#slow-downstream-dependency)
6. [Database Lock Contention](#database-lock-contention)
7. [Config Drift](#config-drift)
8. [DNS / Service Discovery Failure](#dns-service-discovery)
9. [Certificate Expiry](#certificate-expiry)
10. [Message Queue Backlog](#message-queue-backlog)

---

## Connection Pool Exhaustion {#connection-pool-exhaustion}

**Signature**
- HTTP 500/503 errors starting gradually then accelerating
- Error message: `connection pool timeout`, `too many connections`, `FATAL: remaining connection slots are reserved`
- Pool utilization metric: hits 100% and stays there
- Often starts ~1–4 hours after a deployment (slow leak)

**Causal Chain**
`connection leak in code → pool fills slowly → new requests queue → queue fills → timeout errors → cascading failures`

**Diagnosis**
```bash
# PostgreSQL — check active connections
SELECT count(*), state FROM pg_stat_activity GROUP BY state;
# MySQL
SHOW STATUS LIKE 'Threads_connected';
# Redis
redis-cli INFO clients | grep connected_clients
```

**Discrimination**
- If connections are high AND pool is at 100%: exhaustion confirmed
- If pool is below 100% but errors persist: look at query latency instead (lock contention)
- If connections drop after restart but creep back: connection leak in application code

---

## Cascading Failure / Thundering Herd {#cascading-failure}

**Signature**
- Multiple services failing simultaneously or in rapid succession
- Error spike starts in one service, propagates outward
- Retry storms: request rate increases as services retry failing calls
- CPU/memory spikes across services with no new deployment

**Causal Chain**
`origin service degrades → retries amplify load → origin service worsens → downstream services timeout → all services fail`

**Diagnosis**
- Identify the **first** service to degrade in the timeline (check timestamp precision)
- Look for retry configuration — are services retrying with backoff?
- Check for circuit breakers — are any open?

**Key Differentiators**
- True cascading: origin fails first, others follow in dependency order
- Config change: multiple services fail simultaneously → shared config or infra
- Traffic spike: all services fail simultaneously, no origin → upstream overload

---

## Memory Leak / OOM {#memory-leak-oom}

**Signature**
- Memory metric climbs steadily (sawtooth pattern if GC runs, linear if GC can't keep up)
- `OOMKilled` in k8s pod events, or process crash with `Killed` in logs
- Performance degrades gradually before crash (GC pressure)
- JVM: `java.lang.OutOfMemoryError: Java heap space` or `GC overhead limit exceeded`
- Node.js: `FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - JavaScript heap out of memory`

**Diagnosis**
```bash
# k8s — check OOMKilled events
kubectl describe pod <pod-name> | grep -A5 OOMKilled
kubectl get events --field-selector reason=OOMKilling

# Process memory over time
# Check if memory grew steadily since last deploy — that's a leak
```

**Discrimination**
- Gradual climb since deployment → likely leak introduced in that deploy
- Sudden spike to limit → traffic spike or one large object allocation
- Repeated OOM with no growth pattern → limit set too low for workload

---

## Deployment Regression {#deployment-regression}

**Signature**
- Incident starts within 30 minutes of a deployment
- Error rate or latency change correlates precisely with deploy timestamp
- Rollback restores service

**Diagnosis**
1. Confirm deploy timestamp vs. incident start — are they within 15 minutes?
2. Check diff for: dependency upgrades, config changes, DB migrations, new code paths in hot functions
3. Look for feature flags that were enabled with the deploy

**High-Risk Diff Patterns**
- ORM upgrade or query builder change → N+1 query regression
- Timeout value changes → previously-hidden slow queries now surfacing
- New index removed or migration not run → full table scans
- Dependency with known CVE or breaking change in patch version

---

## Slow Downstream Dependency {#slow-downstream-dependency}

**Signature**
- Latency increase in your service, but your own CPU/memory is normal
- Trace spans show time accumulating in an outbound HTTP or DB call
- `504 Gateway Timeout` or `upstream timeout` errors
- Third-party API or internal service is the slow span

**Diagnosis**
- Isolate using distributed traces — find the slow span
- Check if the downstream has its own status page or incident
- Test latency directly: `curl -w "%{time_total}" <endpoint>`

**Remediation Options (in order)**
1. Circuit breaker: stop sending traffic, return fallback response
2. Timeout reduction: fail faster, don't exhaust your own thread pool waiting
3. Cache: serve stale data if acceptable
4. Async: move the dependency call off the critical path if possible

---

## Database Lock Contention {#database-lock-contention}

**Signature**
- Queries taking much longer than usual (seconds vs. milliseconds)
- `lock wait timeout exceeded`, `deadlock found`, `could not obtain lock`
- CPU on DB server is low, but connections are high and queries queue
- Often triggered by: long-running migration, bulk update, or new query without proper index

**Diagnosis**
```sql
-- PostgreSQL: find blocking queries
SELECT pid, now() - pg_stat_activity.query_start AS duration, query, state
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 minutes';

-- Find lock holders
SELECT * FROM pg_locks pl LEFT JOIN pg_stat_activity psa
  ON pl.pid = psa.pid WHERE NOT granted;
```

---

## Config Drift {#config-drift}

**Signature**
- Incident affects only a subset of instances/pods despite same code
- Error is environment-specific (only prod, only region X, only specific pods)
- No recent code deployment but behavior changed

**Diagnosis**
- Compare env vars across affected vs. healthy instances
- Check if secret rotation or config management system (Vault, Parameter Store) pushed a change
- Look at instance launch times — newly launched instances may have different config

---

## DNS / Service Discovery Failure {#dns-service-discovery}

**Signature**
- `connection refused`, `no such host`, `name resolution failed`
- Service was working, then started failing with connection errors (not application errors)
- Multiple services failing to reach the same endpoint

**Diagnosis**
```bash
nslookup <service-name>
dig <service-name> @<dns-server>
# In k8s:
kubectl exec -it <pod> -- nslookup <service>.<namespace>.svc.cluster.local
```

---

## Certificate Expiry {#certificate-expiry}

**Signature**
- `certificate has expired`, `SSL handshake failed`, `x509: certificate signed by unknown authority`
- Exact timestamp of failure often corresponds to cert expiry time
- Affects all clients equally (not percentage-based)

**Diagnosis**
```bash
echo | openssl s_client -connect <host>:443 2>/dev/null | openssl x509 -noout -dates
```

---

## Message Queue Backlog {#message-queue-backlog}

**Signature**
- Consumer lag metric climbing
- Events processing with increasing delay
- Downstream systems showing stale data
- Consumer CPU normal but throughput dropping → consumer is blocked

**Diagnosis**
- Check consumer lag (Kafka: `kafka-consumer-groups.sh --describe`, SQS: `ApproximateNumberOfMessages`)
- Is the consumer erroring? Check DLQ message count
- Is the producer producing faster than usual (traffic spike) or is the consumer slower (regression)?
