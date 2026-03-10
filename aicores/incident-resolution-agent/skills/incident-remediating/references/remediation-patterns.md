# Remediation Playbooks

## Table of Contents
1. [Kubernetes Rollback](#kubernetes-rollback)
2. [Feature Flag Disable](#feature-flag-disable)
3. [Database Connection Pool Recovery](#db-connection-pool-recovery)
4. [Circuit Breaker Activation](#circuit-breaker-activation)
5. [Horizontal Pod Autoscaling / Manual Scale-Out](#horizontal-scaling)
6. [Cache Invalidation](#cache-invalidation)
7. [Hotfix PR Fast Path](#hotfix-pr-fast-path)
8. [Secret / Config Rotation Recovery](#secret-rotation-recovery)
9. [Dependency CVE Patching](#dependency-cve-patching)

---

## Kubernetes Rollback {#kubernetes-rollback}

Use when: A deployment is the likely root cause and rollback is faster than a hotfix.

```bash
# Check rollout history
kubectl rollout history deployment/<name> -n <namespace>

# Roll back to previous revision
kubectl rollout undo deployment/<name> -n <namespace>

# Roll back to specific revision
kubectl rollout undo deployment/<name> --to-revision=<N> -n <namespace>

# Monitor rollback
kubectl rollout status deployment/<name> -n <namespace>

# Verify pods are healthy
kubectl get pods -n <namespace> -l app=<name>
```

**Risk**: If the previous version had a schema migration that the old code can't handle, rollback will not restore service. Check migrations before rolling back.

**Revert the revert**: `kubectl rollout undo deployment/<name>` a second time goes forward again.

---

## Feature Flag Disable {#feature-flag-disable}

Use when: The broken behavior is behind a feature flag.

Most feature flag systems (LaunchDarkly, Unleash, Flagsmith, custom) support:
1. Targeting rules — disable for all users or narrow to internal users first
2. Kill switch — boolean flag override that takes priority over targeting rules

**LaunchDarkly**
- Dashboard → Feature Flags → [flag name] → toggle OFF for production environment
- Or via API: `PATCH /api/v2/flags/<projKey>/<flagKey>` with `{"on": false}`

**Unleash**
- Dashboard → Toggles → [toggle name] → Disable for production

**Custom env var flags**: Update the environment variable in your config store (Parameter Store, Vault, k8s ConfigMap) and restart affected pods if not using hot-reload.

---

## Database Connection Pool Recovery {#db-connection-pool-recovery}

Use when: Connection pool is exhausted, queries are timing out.

**Immediate — kill idle connections**
```sql
-- PostgreSQL: kill idle connections older than 5 minutes
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE state = 'idle'
  AND query_start < now() - interval '5 minutes'
  AND pid <> pg_backend_pid();
```

**Immediate — restart application pods** (leaking connections will be released)
```bash
kubectl rollout restart deployment/<name> -n <namespace>
```

**Stopgap — increase pool size temporarily**
Change `max_pool_size` or equivalent in your app config/env var and redeploy.
This buys time but does not fix the leak.

**Root fix**: Audit connection lifecycle in code — look for:
- Missing `connection.close()` in error paths
- Missing `finally` / `defer` blocks
- ORM sessions not being properly committed/rolled back

---

## Circuit Breaker Activation {#circuit-breaker-activation}

Use when: A downstream service is slow or failing and you need to stop cascading failures.

**Hystrix / Resilience4j (Java)**
- Force open via actuator: `POST /actuator/circuitbreakers/{name}/state` with body `{"state": "FORCED_OPEN"}`

**Istio Service Mesh**
```yaml
# Apply outlier detection (circuit breaker at mesh level)
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: <service>-circuit-breaker
spec:
  host: <service>
  trafficPolicy:
    outlierDetection:
      consecutive5xxErrors: 5
      interval: 30s
      baseEjectionTime: 30s
```

**Manual / Emergency**: Route traffic away from the downstream service at the load balancer or API gateway level.

---

## Horizontal Scaling {#horizontal-scaling}

Use when: CPU or memory saturation is causing timeouts; more replicas will distribute load.

```bash
# Manual scale-out
kubectl scale deployment/<name> --replicas=<N> -n <namespace>

# Temporarily override HPA min replicas
kubectl patch hpa <name> -n <namespace> -p '{"spec":{"minReplicas":<N>}}'

# Check current HPA status
kubectl get hpa -n <namespace>
```

**Warning**: Scaling out does not help if the bottleneck is a shared downstream resource (DB, external API). Confirm the bottleneck is in the service itself before scaling.

---

## Cache Invalidation {#cache-invalidation}

Use when: Stale or corrupted data in cache is causing incorrect responses.

```bash
# Redis — flush a specific key pattern
redis-cli --scan --pattern "user:*" | xargs redis-cli del

# Redis — flush entire DB (nuclear option)
redis-cli FLUSHDB

# Memcached
echo "flush_all" | nc <host> 11211
```

**Risk**: Cache flush will cause a cache stampede if many keys are invalidated simultaneously. Consider a grace period or gradual invalidation if traffic is high.

---

## Hotfix PR Fast Path {#hotfix-pr-fast-path}

Use when: Root cause is identified, fix is narrow, and you need to ship it fast.

**Branch strategy**
```bash
git checkout main && git pull
git checkout -b hotfix/<incident-id>-<short-description>
# Make fix
git commit -m "fix(<scope>): <imperative description>

Fixes: <Jira ticket>
Incident: <incident-id>
Root cause: <one sentence>"
git push origin hotfix/<incident-id>-<short-description>
```

**PR requirements for hotfix**
- Minimum: 1 reviewer (ideally on-call lead)
- Must include: regression test or test plan
- Must include: rollback plan in PR description
- Merge strategy: prefer squash to keep history clean

**Post-merge**
- Tag the release with incident ID
- Ensure deploy pipeline runs and verify metrics normalize
- Update Jira ticket with fix SHA

---

## Secret / Config Rotation Recovery {#secret-rotation-recovery}

Use when: A secret or config value was rotated externally and services aren't aware of the new value.

1. Confirm the new secret value in your secret store (Vault, AWS Secrets Manager, etc.)
2. Update the k8s Secret or ConfigMap:
```bash
kubectl create secret generic <name> \
  --from-literal=<KEY>=<new-value> \
  --dry-run=client -o yaml | kubectl apply -f -
```
3. Restart affected deployments to pick up new secret:
```bash
kubectl rollout restart deployment/<name> -n <namespace>
```
4. Verify pods are reading the new value (check startup logs or a health endpoint that reflects config)

---

## Dependency CVE Patching {#dependency-cve-patching}

Use when: A critical CVE has been announced in a library you depend on.

**Assessment**
1. Confirm you're on the affected version range
2. Check if a patched version exists
3. Assess exploitability: Is the vulnerable code path reachable in your usage?

**npm**
```bash
npm audit
npm audit fix
# For major version bumps requiring manual review:
npm audit fix --force
```

**pip**
```bash
pip-audit  # or: safety check
pip install --upgrade <package>
```

**Gradle/Maven**
```bash
# Use OWASP dependency check
./gradlew dependencyCheckAnalyze
# Then update in build.gradle / pom.xml
```

**PR for CVE patch**
- Title: `chore(deps): patch CVE-XXXX-XXXX in <package>`
- Description must include: CVE ID, affected version, fixed version, exploitability assessment
- Label: `security`, `dependency`
- Priority: P0 if CVSS ≥ 9.0, P1 if 7.0–8.9, P2 otherwise
