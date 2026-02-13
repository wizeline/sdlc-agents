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

**Why it matters:** 94% of tested applications show some form of access control weakness. This remains the #1 risk because failures here directly expose data and functionality to unauthorized users.

**Common weaknesses:**
- Bypassing access control checks by modifying the URL, application state, or HTML page
- Allowing the primary key to be changed to another user's record (IDOR)
- Elevation of privilege — acting as a user without being logged in, or acting as an admin when logged in as a user
- CORS misconfiguration allowing access from unauthorized origins
- Force browsing to authenticated pages or accessing API endpoints without proper tokens

**Mitigation strategy:**
- Deny by default — require explicit grants for every resource
- Centralize authorization logic in a trusted service layer; never rely on client-side enforcement
- Implement rate limiting on API and controller access to minimize automated attack impact
- Invalidate JWT tokens on the server side on logout; use short-lived tokens
- Log access control failures and alert administrators on repeated failures
- Disable web server directory listing and ensure file metadata (.git) is not present in web roots

**ASVS alignment:** V4 (Access Control), particularly V4.1 (General Access Control Design)

---

## A02: Security Misconfiguration

**Why it matters:** Rose from #5 (2021) to #2 (2025). As applications move to cloud and container-based deployments, the configuration surface area has exploded, and defaults are rarely secure.

**Common weaknesses:**
- Unnecessary features enabled (ports, services, pages, accounts, privileges)
- Default accounts and passwords unchanged
- Error handling reveals stack traces or overly informative error messages
- Security headers missing or misconfigured (CSP, HSTS, X-Frame-Options)
- Software is out of date or has known vulnerabilities in configurations
- Cloud storage buckets with public access

**Mitigation strategy:**
- Implement a repeatable hardening process; use infrastructure-as-code (IaC) to enforce baselines
- Deploy a minimal platform — remove or do not install unused features and frameworks
- Review and update configurations as part of the patch management process
- Implement a segmented application architecture with separation between components
- Send security directives to clients (e.g., security headers)
- Automate configuration verification in CI/CD using tools like checkov, tfsec, or kics

**ASVS alignment:** V14 (Configuration)

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

**Why it matters:** Includes XSS. Dropped in rank as modern frameworks provide built-in protections, but remains prevalent — especially in legacy code and dynamic query construction.

**Common weaknesses:**
- User-supplied data not validated, filtered, or sanitized
- Dynamic queries or non-parameterized calls used without context-aware escaping
- Hostile data used within ORM search parameters to extract additional records
- Hostile data directly used or concatenated in SQL, OS commands, LDAP, XPath, or NoSQL queries

**Mitigation strategy:**
- Use parameterized queries (prepared statements) for all database interactions
- Use server-side positive allow-list input validation
- Escape special characters using context-specific output encoding
- Use LIMIT and other SQL controls within queries to prevent mass disclosure in case of injection
- Apply Content Security Policy (CSP) as a defense-in-depth against XSS
- Use modern frameworks that auto-escape output by default (React, Angular, etc.)

**ASVS alignment:** V5 (Validation, Sanitization and Encoding)

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

**Why it matters:** New category in 2025. Applications that don't handle errors safely can leak information, enter undefined states, or become exploitable when things go wrong.

**Common weaknesses:**
- Stack traces or internal error details exposed to users
- Application enters an undefined or insecure state after an exception
- Error messages reveal database type, query structure, or file paths
- Lack of fallback mechanisms for external service failures
- Inconsistent error handling across the application (some paths fail open, others fail closed)

**Mitigation strategy:**
- Implement global exception handlers that catch unhandled errors and return safe, generic messages
- Ensure all failure paths result in a "fail closed" (deny) state, not "fail open" (allow)
- Log detailed error information server-side while showing generic messages client-side
- Design for graceful degradation when external dependencies are unavailable
- Test error handling paths explicitly — include negative test cases and chaos engineering
- Review error handling patterns in code reviews with the same rigor as business logic

**ASVS alignment:** V7 (Error Handling and Logging)
