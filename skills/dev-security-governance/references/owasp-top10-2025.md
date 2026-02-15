# OWASP Top 10 (2025) — Detailed Reference

## Table of Contents
1. [A01: Broken Access Control](#a01-broken-access-control)
2. [A02: Security Misconfiguration](#a02-security-misconfiguration)
3. [A03: Software Supply Chain Failures](#a03-software-supply-chain-failures)
4. [A04: Cryptographic Failures](#a04-cryptographic-failures)
5. [A05: Injection](#a05-injection)
6. [A06: Insecure Design](#a06-insecure-design)
7. [A07: Authentication Failures](#a07-authentication-failures)
8. [A08: Software and Data Integrity Failures](#a08-software-and-data-integrity-failures)
9. [A09: Logging and Alerting Failures](#a09-logging-and-alerting-failures)
10. [A10: Mishandling of Exceptional Conditions](#a10-mishandling-of-exceptional-conditions)

---

## A01: Broken Access Control

**Why it matters:** 3.73% of tested applications exhibited one or more of 40 mapped CWEs in
this category. Remains #1 because failures directly expose data and functionality. The 2025
edition consolidates SSRF into this category, recognizing that network-level access control
failures often enable privilege escalation to internal services.

**Attack vectors (granular):**

*Horizontal privilege escalation* — Users access resources belonging to other users at the
same privilege level by manipulating resource identifiers (e.g., changing `/profile/677` to
`/profile/678`). Particularly prevalent in RESTful APIs with exposed IDs and GraphQL with
nested object access that may not inherit parent-level authorization.

*Vertical privilege escalation* — Regular users access administrative capabilities through
forced browsing, client-side code analysis, or API pattern inference. API-first architectures
expand this risk when admin endpoints lack authorization middleware.

*IDOR (Insecure Direct Object References)* — Internal objects (database keys, file paths)
exposed without access control verification. OWASP API Security Top 10 identifies the API
variant "Broken Object Level Authorization" (BOLA) as the #1 API risk.

*SSRF (consolidated from 2021 A10)* — Server-side requests to internal resources via
manipulated URLs, enabling access to cloud metadata, internal services, and admin interfaces.

**Common weaknesses:**
- Authentication without authorization (user is logged in but ownership not verified)
- Client-side controls that can be bypassed by intercepting/modifying requests
- CORS misconfiguration (wildcard origins, reflected origins, credentials with permissive origins)
- Bulk operations/export endpoints enabling mass data access via single requests

**Mitigation strategy:**
- Deny by default — require explicit grants for every resource
- Centralize authorization logic in a trusted service layer; never rely on client-side enforcement
- Implement rate limiting on API and controller access to minimize automated attack impact
- Log access control failures and alert administrators on repeated failures
- Disable web server directory listing and ensure file metadata (.git) is not present in web roots

**JWT token security (specific requirements):**

| Requirement | Implementation | Common Failure |
|-------------|---------------|----------------|
| Signature verification | Strong algorithms (RS256, ES256), explicit algorithm spec | Algorithm confusion ("none", alg substitution) |
| Token scope | Minimal claims, short expiration | Excessive permissions, long-lived tokens |
| Invalidation | Token blacklists or short expiry + refresh rotation | Stateless tokens that cannot be revoked |
| Secure transmission | TLS, Secure/HttpOnly/SameSite cookies | Token interception, XSS-based theft |

**ASVS alignment:** V4 (Access Control), particularly V4.1 (General Access Control Design)
**CWE mapping:** CWE-639, CWE-22, CWE-918, CWE-284, CWE-285

---

## A02: Security Misconfiguration

**Why it matters:** Surged from #5 (2021) to #2 (2025). Data shows 100% of tested
applications had some form of misconfiguration, with 3.00% having serious security-relevant
misconfigurations across 16 mapped CWEs. The explosion of cloud-native, highly configurable
architectures has dramatically expanded the configuration surface area.

**Common weaknesses:**
- Default credentials on production systems (application servers, cloud consoles, container orchestration dashboards)
- Unnecessary features enabled (sample apps, admin interfaces, debugging endpoints, unused protocols)
- Error handling exposing stack traces, database schema, internal paths, framework versions
- Security headers missing or misconfigured (CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy)
- CORS misconfiguration (now consolidated into A01 for access control implications, but config root cause is here)
- Cloud storage buckets with public access
- Container images with development tools, package managers, shell access in production

**HTTP Security Headers (specific):**

| Header | Protection | Common Misconfiguration |
|--------|-----------|----------------------|
| Content-Security-Policy | XSS mitigation, injection prevention | Missing, or `unsafe-inline`/`unsafe-eval` defeating protection |
| Strict-Transport-Security | HTTPS enforcement, downgrade prevention | Missing, or short max-age |
| X-Content-Type-Options | MIME sniffing prevention | Missing `nosniff` |
| X-Frame-Options | Clickjacking prevention | Missing, or ALLOW-FROM with untrusted origins |
| Referrer-Policy | Information leakage control | Missing, or `unsafe-url` |

**Kubernetes-specific hardening:**

| Security Context | Secure Config | Risk of Misconfiguration |
|-----------------|--------------|------------------------|
| runAsNonRoot | `true` | Root execution enables container escape |
| readOnlyRootFilesystem | `true` | Writable root enables persistence, malware |
| allowPrivilegeEscalation | `false` | Bypasses container isolation |
| capabilities | Drop all, add only required | Excessive capabilities enable kernel exploitation |
| seccompProfile | RuntimeDefault or custom restricted | Unrestricted syscalls enable container escape |

Enforce Pod Security Standards (restricted, baseline, privileged) via admission controllers.

**Mitigation strategy:**
- Implement repeatable, automated hardening processes (Infrastructure-as-Code with security scanning)
- Deploy minimal platform — remove or do not install unused features and frameworks
- Use distroless/Alpine base images, multi-stage builds, non-root execution for containers
- Review and update configurations as part of patch management
- Automate configuration verification in CI/CD (checkov, tfsec, kics, cfn-nag)
- Risk-based patch prioritization with emergency response for actively exploited vulns

**ASVS alignment:** V14 (Configuration)
**CWE mapping:** CWE-16, CWE-2, CWE-388

---

## A03: Software Supply Chain Failures

**Why it matters:** A significant expansion from the previous "Vulnerable and Outdated Components." Reflects the SolarWinds-era reality that the software supply chain itself is a primary attack vector.

**Common weaknesses:**
- Using components with known vulnerabilities
- No inventory of component versions (no SBOM)
- Transitive dependency vulnerabilities not tracked
- Build pipeline fetches unsigned or unverified dependencies
- Lack of provenance verification for third-party artifacts

**Mitigation strategy:**
- Generate and maintain a Software Bill of Materials (SBOM) for every release
- Pin dependency versions using lockfiles; avoid floating version ranges
- Automate SCA (Software Composition Analysis) in CI/CD
- Verify artifact integrity using checksums and signatures
- Subscribe to security advisories for all dependencies
- Evaluate the health/maintenance status of open-source dependencies before adoption

**ASVS alignment:** V14.2 (Dependency), NIST SSDF PW.4

---

## A04: Cryptographic Failures

**Why it matters:** Rebranded from "Sensitive Data Exposure" to focus on root causes. The flaw isn't that data was exposed — it's that cryptographic protections were absent, weak, or misconfigured.

**Common weaknesses:**
- Data transmitted in clear text (HTTP, SMTP, FTP)
- Weak or deprecated cryptographic algorithms (MD5, SHA1, DES)
- Default or hard-coded cryptographic keys
- Improperly managed key rotation
- Insufficient randomness for cryptographic operations
- Certificates not validated or expired

**Mitigation strategy:**
- Classify data by sensitivity; identify which data needs encryption at rest and in transit
- Enforce TLS 1.2+ for all data in transit; use HSTS
- Store passwords with strong adaptive salted hashing (Argon2id, bcrypt, scrypt)
- Use authenticated encryption (AES-256-GCM) for data at rest
- Rotate keys regularly and store them in dedicated key management systems (KMS)
- Avoid rolling your own cryptography — use well-vetted libraries

**ASVS alignment:** V6 (Stored Cryptography), V9 (Communications)

---

## A05: Injection

**Why it matters:** Includes XSS. Dropped from #3 to #5 as modern frameworks provide built-in protections, but remains among the most dangerous and frequently exploited vulnerabilities — especially in legacy code and dynamic query construction.

### SQL Injection
**Parameterized queries are the primary defense** — they separate SQL code from data so
user input is treated as literal values, never executable code.

| Requirement | Implementation | Verification |
|-------------|---------------|-------------|
| Complete parameterization | All variable query components use parameter binding | Static analysis for string concatenation in queries |
| Framework-safe APIs | ORM query builders, criteria APIs, entity manager find methods | Code review of ORM usage patterns |
| Avoid dangerous methods | Prohibit raw query methods, SQL fragments in order clauses | Automated detection of unsafe ORM patterns |
| Server-side enforcement | Parameter binding through database driver APIs | Driver-level verification |

**ORM safety matrix:**

| Safe Patterns | Unsafe Anti-Patterns |
|---------------|---------------------|
| LINQ-style query builders | Raw query methods (FromSqlRaw, createSQLQuery) |
| Criteria APIs | SQL fragments in order/sort clauses |
| Entity manager find methods | Dynamic query construction features |
| Named queries with parameter binding | String concatenation in query building |

**Stored procedure security:** Static SQL with parameterized inputs only — no EXECUTE IMMEDIATE with concatenation. Least privilege: execute permissions on specific procedures, not direct table access.

### Command Injection
Avoid shell command execution with user-influenced parameters entirely when possible. Use native language APIs and libraries instead.

When shell execution is unavoidable:
- Allow-list validation of permitted characters and patterns
- Array-style argument passing (separates command from arguments)
- Reject shell metacharacters: `;`, `|`, `&`, `` ` ``, `$()`, `${}`
- Context-appropriate validation specific to expected input type

Defense-in-depth: restricted user contexts, container/sandbox isolation, network segmentation, read-only filesystems, execution timeouts.

### LDAP and NoSQL Injection

| Database | Injection Vector | Prevention |
|----------|-----------------|-----------|
| LDAP | Special characters `*`, `(`, `)`, `\`, NUL | Parameterized LDAP queries or rigorous escaping |
| MongoDB | JavaScript `$where` clauses, operator injection | Disable JS execution, validate query structure, use official drivers |
| CouchDB | JavaScript map/reduce functions | Input validation, sandboxed execution |
| Redis | Command injection via concatenated arguments | Official client libraries with proper argument encoding |

### XSS (Cross-Site Scripting)
Context-specific output encoding is the primary defense. See `references/secure-coding-practices.md`
Section 2 (Output Encoding) for detailed encoding rules by context (HTML body, attributes,
JavaScript, CSS, URL).

**Mitigation strategy:**
- Parameterized queries for all database interactions — strongly typed
- Server-side positive allow-list input validation on complete, canonicalized input
- Context-specific output encoding (HTML, JS, URL, CSS contexts)
- Content Security Policy (CSP) as defense-in-depth (not replacement for encoding)
- Use modern frameworks with auto-escaping by default (React, Angular, Vue)
- Use LIMIT and other SQL controls to prevent mass disclosure in case of injection

**ASVS alignment:** V5 (Validation, Sanitization and Encoding)
**CWE mapping:** CWE-79, CWE-89, CWE-78, CWE-90, CWE-943

---

## A06: Insecure Design

**Why it matters:** New in 2021, reinforced in 2025. Even perfectly coded applications can be fatally flawed if the architecture itself doesn't account for security. This is a different category from implementation bugs — it's about missing or ineffective control design.

**Common weaknesses:**
- No threat modeling performed during design
- Business logic flaws (e.g., a cinema booking system that doesn't prevent bulk ticket purchases by scalpers)
- Password recovery using "security questions" whose answers are publicly discoverable
- Lack of abuse case / misuse case testing
- Missing rate limiting on sensitive operations

**Mitigation strategy:**
- Integrate threat modeling (STRIDE, PASTA, Attack Trees) into the design phase
- Establish and use a library of secure design patterns
- Write abuse stories / misuse cases alongside user stories
- Integrate plausibility checks at the business logic level
- Limit resource consumption by user or service (rate limiting, quotas)
- Use reference architectures for common patterns (authentication flows, payment processing)

**ASVS alignment:** V1 (Architecture, Design and Threat Modeling)

---

## A07: Authentication Failures

**Why it matters:** Identity is the new perimeter. Failures in authentication systems provide attackers with the keys to the kingdom.

**Common weaknesses:**
- Permits brute force or credential stuffing attacks
- Permits default, weak, or well-known passwords
- Uses weak credential recovery processes (knowledge-based answers)
- Missing or ineffective multi-factor authentication
- Exposes session identifier in the URL
- Does not properly invalidate sessions on logout or timeout

**Mitigation strategy:**
- Align with NIST 800-63b Digital Identity Guidelines
- Implement MFA for all user-facing and admin authentication
- Check passwords against lists of breached credentials (e.g., Have I Been Pwned API)
- Enforce password complexity minimums (length ≥ 8, not in breach lists) — not arbitrary complexity rules
- Limit failed login attempts with progressive delays and account lockout
- Use server-side, secure, high-entropy session management; regenerate session IDs after login

**ASVS alignment:** V2 (Authentication), V3 (Session Management)

---

## A08: Software and Data Integrity Failures

**Why it matters:** Addresses integrity of the software delivery pipeline — if an attacker can modify code, dependencies, or build artifacts without detection, all other security controls become irrelevant.

**Common weaknesses:**
- Application relies on plugins, libraries, or modules from untrusted sources or CDNs without integrity checks (e.g., no Subresource Integrity)
- Insecure CI/CD pipeline without verification of code or artifact integrity
- Auto-update functionality without adequate signature verification
- Insecure deserialization where serialized objects from untrusted sources are processed

**Mitigation strategy:**
- Use digital signatures to verify software and data are from the expected source
- Ensure libraries and dependencies are consumed from trusted repositories
- Use a software supply chain security tool (e.g., SLSA framework) to verify build provenance
- Implement code review processes to minimize the chance of malicious code being introduced
- Ensure the CI/CD pipeline has proper segregation, access control, and integrity checking
- Do not send unprotected serialized data to untrusted clients without integrity checks

**ASVS alignment:** V14 (Configuration), V10 (Malicious Code)

---

## A09: Logging and Alerting Failures

**Why it matters:** Without adequate logging and monitoring, breaches cannot be detected. The average time to detect a breach is still measured in months — effective logging cuts this dramatically.

**Common weaknesses:**
- Auditable events (logins, failed logins, high-value transactions) are not logged
- Warnings and errors generate no, inadequate, or unclear log messages
- Logs are only stored locally and not aggregated centrally
- Alerting thresholds and response escalation processes are not in place
- Penetration testing and DAST scans do not trigger alerts
- Application cannot detect, escalate, or alert for active attacks in real time

**Mitigation strategy:**
- Log all access control failures, input validation failures, and authentication failures with sufficient context
- Ensure logs are generated in a format easily consumed by centralized log management (structured JSON)
- Ensure high-value transactions have an audit trail with integrity controls to prevent tampering
- Establish effective monitoring and alerting so suspicious activities are detected and responded to promptly
- Adopt an incident response and recovery plan (NIST 800-61)
- Never log sensitive data (passwords, tokens, PII) in plaintext

**ASVS alignment:** V7 (Error Handling and Logging)

---

## A10: Mishandling of Exceptional Conditions

**Why it matters:** New category in 2025, replacing standalone SSRF (consolidated into A01).
Reflects growing recognition that application behavior under error conditions, resource
exhaustion, and abnormal inputs creates significant security vulnerabilities that traditional
testing often overlooks. The central theme is **"failing open"** — security checks that
cannot complete due to errors default to allowing access.

**Information disclosure through error handling:**

| Information Type | Exposure Risk | Example |
|-----------------|-------------|---------|
| Stack traces | Code structure, framework versions | `at com.example.UserDao.getUser(UserDao.java:47)` |
| Database errors | Schema, query structure | `ORA-00942: table or view does not exist "ADMIN_USERS"` |
| System paths | Directory structure, deployment layout | `/var/www/app/releases/20240115/config/database.yml` |
| Framework identifiers | Known vulnerability targeting | `X-Powered-By: PHP/7.4.3` |

**Fail-open vs. fail-secure scenarios:**

| Failure Scenario | Fail-Open Risk (DANGEROUS) | Fail-Secure Alternative |
|-----------------|--------------------------|----------------------|
| Database error during authz check | Access granted | Access denied + logged error + alert |
| Authentication service timeout | Anonymous access permitted | Request queued, fallback auth, or denied |
| Encryption key unavailable | Plaintext transmission | Connection refused, service degradation |
| Audit logging failure | Unlogged operation proceeds | Operation blocked until logging restored |

**Common weaknesses:**
- Stack traces or internal error details exposed to users
- Application enters undefined or insecure state after exception
- Error messages reveal database type, query structure, or file paths
- Lack of fallback mechanisms for external service failures
- Inconsistent error handling (some paths fail open, others fail closed)

**Mitigation strategy:**
- Implement global exception handlers returning safe, generic messages to users
- **Fail-secure by default**: all security-critical operations deny access on any error condition
- **Explicit failure handling**: every security check has a defined error path, no implicit continuation
- Log detailed error information server-side while showing generic messages client-side
- Design for graceful degradation: reduced functionality, never security bypass
- Test error handling paths explicitly — fault injection testing, chaos engineering
- Review error handling in code reviews with same rigor as business logic
- Comprehensive resource cleanup guarantees (try-finally, using statements, destructors)
- Custom security exception types to distinguish security failures from operational errors

**Real-world illustration:** The CrowdStrike incident (July 2024) — a single content update caused
>8.5 million system crashes with >$5 billion estimated losses, representing a failure where
automated processes became catastrophic due to insufficient validation and monitoring of
their own critical components.

**ASVS alignment:** V7 (Error Handling and Logging)
**CWE mapping:** CWE-754, CWE-755, CWE-209, CWE-391
