# Agent Skill Implementation Framework — Automated Security Analysis Reference

## Table of Contents
1. [SAST Integration](#sast-integration)
2. [DAST Coordination](#dast-coordination)
3. [Detection Rules by OWASP Category](#detection-rules-by-owasp-category)
4. [CWE Correlation & Severity Scoring](#cwe-correlation--severity-scoring)
5. [CI/CD Gate Enforcement](#cicd-gate-enforcement)
6. [Developer Workflow Integration](#developer-workflow-integration)
7. [Context-Aware Recommendations](#context-aware-recommendations)

---

## SAST Integration

### Source Code Pattern Recognition
When performing static analysis, apply these techniques in order of sophistication:

| Technique | What It Detects | How |
|-----------|----------------|-----|
| Syntactic pattern matching | Dangerous function calls, suspicious strings | Regex, AST traversal |
| Taint analysis | Untrusted data flowing from sources to sinks | Data flow graphs, path-sensitive analysis |
| Control flow analysis | Authorization bypass paths, unreachable code | CFG construction, dominance analysis |
| Semantic analysis | Framework-specific vulnerability patterns | Framework modeling, API behavior knowledge |

Technology-specific calibration is essential — vulnerability patterns vary significantly
across languages and frameworks. Always identify the stack before selecting rules.

### Configuration File Analysis

| Config Domain | What to Analyze | Security Checks |
|---------------|----------------|-----------------|
| Application config | Framework settings, feature flags | Debug mode, weak crypto, verbose errors |
| Infrastructure-as-Code | Terraform, CloudFormation, ARM | Public exposure, weak IAM, missing encryption |
| Container configs | Dockerfiles, K8s manifests | Privileged execution, secret exposure, image sources |
| CI/CD pipelines | GitHub Actions, GitLab CI | Secret handling, privilege escalation, supply chain |

---

## DAST Coordination

### Runtime Behavior Monitoring

| Monitoring Dimension | Data Source | Security Insight |
|---------------------|------------|-----------------|
| Authentication anomalies | Login success/failure patterns | Credential stuffing, brute force |
| Authorization failures | Access denied events | Broken access control probing |
| Input validation triggers | Rejected input patterns | Attack reconnaissance, fuzzing |
| Error rate anomalies | Application + infra errors | Exceptional condition exploitation |
| Resource consumption | CPU, memory, I/O, network | Resource exhaustion, cryptomining |

### Vulnerability Exploitability Verification
Use safe techniques to confirm vulnerabilities without causing harm:

| Approach | Method | Confidence |
|----------|--------|-----------|
| Safe payload injection | Confirms parser evaluation without harmful execution | High for injection |
| Timing analysis | Detects conditional processing without data exfiltration | Moderate (needs baseline) |
| Differential response | Identifies behavioral differences between inputs | Moderate (false positive risk) |
| Out-of-band detection | DNS/HTTP callbacks confirm external interaction | High for SSRF |

---

## Detection Rules by OWASP Category

Coverage targets reflect realistic automation limits — some categories inherently
require manual review and architecture understanding.

| 2025 Category | Detection Rule Focus | Coverage Target |
|---------------|---------------------|-----------------|
| A01: Broken Access Control | Authorization check presence, IDOR patterns, JWT validation, SSRF vectors | 95%+ of CWE-639, CWE-22, CWE-918 |
| A02: Security Misconfiguration | Default credentials, header analysis, cloud config | 90%+ of common misconfigs |
| A03: Supply Chain Failures | Dependency vuln correlation, SBOM validation, signature verification | 95%+ of known vulnerable deps |
| A04: Cryptographic Failures | Algorithm detection, key management patterns, TLS config | 90%+ of weak crypto usage |
| A05: Injection | Query construction patterns, interpreter invocation, encoding | 95%+ of SQLi, command injection |
| A06: Insecure Design | Architecture pattern detection, threat model coverage gaps | 70%+ (manual review essential) |
| A07: Auth Failures | MFA implementation, session security, password storage | 90%+ of common auth weaknesses |
| A08: Integrity Failures | Signing verification, build provenance, deserialization | 85%+ of integrity violations |
| A09: Logging Failures | Log generation verification, sensitive data in logs, integrity | 80%+ of logging gaps |
| A10: Exceptional Conditions | Error handling patterns, resource limits, fail-open detection | 75%+ (runtime analysis needed) |

---

## CWE Correlation & Severity Scoring

### Composite Scoring Model
When prioritizing findings, combine multiple factors rather than relying on CVSS alone:

| Scoring Factor | Data Source | Weight |
|---------------|------------|--------|
| CVSS base score | NVD, vendor advisories | 30% |
| Exploit availability | Exploit-DB, Metasploit, threat intel | 25% |
| Active exploitation | CISA KEV, incident reports, honeypot data | 20% |
| Asset criticality | Business context, data classification | 15% |
| Compensating controls | Security control inventory, effectiveness | 10% |

### Classification Systems Integration
- **CWE** (Common Weakness Enumeration): Granular vulnerability taxonomy for detailed identification
- **CAPEC** (Common Attack Pattern Enumeration): Attack methodology understanding for test generation
- **CVSS** (Common Vulnerability Scoring System): Severity quantification, adjusted for context

### CAPEC Attack Pattern Examples

| CAPEC ID | Pattern | Application |
|----------|---------|-------------|
| CAPEC-66 | SQL Injection | Detect query construction anti-patterns |
| CAPEC-108 | Command Injection | Detect shell invocation with user input |
| CAPEC-112 | Brute Force | Test rate limiting on auth endpoints |
| CAPEC-593 | Session Hijacking | Verify session token entropy and binding |

---

## CI/CD Gate Enforcement

### Pre-Commit Hook Validation
Designed for speed — developers won't tolerate slow hooks.

| Validation Type | Scope | Performance Target |
|----------------|-------|-------------------|
| Secret detection | Entropy analysis, pattern matching for keys/tokens | <1 second |
| Basic pattern matching | Dangerous function calls, obvious misconfigs | <2 seconds |
| Linting integration | Security-focused lint rules | <3 seconds |
| Lightweight SAST | Changed file analysis, incremental scanning | <10 seconds |

### Build Gate Stages

| Gate Stage | Security Checks | On Failure |
|-----------|----------------|------------|
| Compile/unit test | Dependency vulnerability scan, license check | Block build, notify developer |
| Integration test | Full SAST, container image scan, IaC validation | Block staging deployment, create ticket |
| Staging deployment | DAST baseline, configuration verification | Block production promotion, require security review |
| Production promotion | Final policy verification, emergency change validation | Require CAB approval for exceptions |

### False Positive Management
False positives erode developer trust, so manage them actively:

| Activity | Implementation | Key Metric |
|----------|---------------|-----------|
| Triage process | Security review of uncertain findings | Time to triage, reviewer agreement rate |
| Developer feedback | Reported FPs fed back into rule tuning | Feedback response time |
| Rule refinement | Detection logic adjusted based on evidence | FP rate trend (must decrease) |
| Exception tracking | Documented accepted risks with renewal review | Exception aging |

---

## Developer Workflow Integration

### IDE Plugin Capabilities

| Capability | What It Does | UX |
|-----------|-------------|-----|
| Real-time analysis | As-you-type security issue detection | Inline highlighting, quick-fix suggestions |
| Contextual documentation | Security explanation + secure code examples | Hover info, dedicated security panel |
| Secure code completion | Framework-specific secure pattern suggestions | Safe alternatives to dangerous APIs |
| One-click remediation | Automated fix application where safe | Preview changes, apply with undo |

### Pull Request Automated Review

| Feature | Implementation | Value |
|---------|---------------|-------|
| Findings as PR comments | Line-specific security annotations | Contextual, immediately actionable |
| Risk scoring in PR description | Aggregate security delta summary | Prioritization for reviewers |
| Required check status | Blocking merge for critical findings | Policy enforcement |
| Remediation guidance links | Direct to relevant docs and examples | Education, self-service resolution |

---

## Context-Aware Recommendations

### Technology Stack Identification
Adapt guidance to the actual stack by detecting these signals:

| Detection Method | Signal | Guidance Adaptation |
|-----------------|--------|-------------------|
| Build file analysis | `pom.xml`, `package.json`, `requirements.txt`, `go.mod` | Language-specific secure coding patterns |
| Import analysis | Framework and library usage | Framework-specific security config |
| Runtime observation | Container images, process execution | Deployment environment hardening |
| Config inspection | `application.yml`, `web.xml`, K8s manifests | Platform-specific security settings |

### Maturity-Based Prioritization
Don't overwhelm beginners with Level 3 advice; don't bore experts with basics.

| Maturity Level | Characteristics | Recommended Focus |
|---------------|----------------|------------------|
| Initial (1) | Ad hoc security, reactive | Foundational controls, quick wins, awareness |
| Managed (2) | Defined processes, basic automation | Process consistency, tool integration, metrics |
| Defined (3) | Standardized, comprehensive | Optimization, advanced techniques, threat modeling |
| Quantitatively managed (4) | Measured, data-driven | Predictive analytics, automated response |
| Optimizing (5) | Continuous innovation | Zero-day response, advanced automation, research |
