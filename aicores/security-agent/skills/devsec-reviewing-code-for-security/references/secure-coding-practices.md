# Secure Coding Practices — 14-Domain Technical Reference

## Table of Contents
1. [Input Validation](#1-input-validation)
2. [Output Encoding](#2-output-encoding)
3. [Authentication & Session Management](#3-authentication--session-management)
4. [Access Control](#4-access-control)
5. [Cryptographic Practices](#5-cryptographic-practices)
6. [Error Handling & Logging](#6-error-handling--logging)
7. [Data Protection & Privacy](#7-data-protection--privacy)
8. [Communication Security](#8-communication-security)
9. [System Configuration](#9-system-configuration)
10. [Database Security](#10-database-security)
11. [File & Resource Management](#11-file--resource-management)
12. [Memory Management](#12-memory-management)
13. [Business Logic Security](#13-business-logic-security)
14. [Dependency & Supply Chain Hygiene](#14-dependency--supply-chain-hygiene)

Each domain includes specific verification points and an **Automation Potential** rating
(High / Medium / Low) to help teams decide what to enforce through tooling versus
manual review.

---

## 1. Input Validation
**Automation Potential: High** — pattern-based detection

### Strategy: Allow-List Over Deny-List
Allow-list (positive) validation is the required default. Define what is permitted and
reject everything else. Deny-list validation is incomplete by design and should only
be used as defense-in-depth when allow-listing is infeasible.

### Verification Points
- All user input validated for expected data type, length, format, and range
- Validation occurs server-side on complete, canonicalized input (after all decoding)
- Strong typing used — explicit parsing with rejection of type confusion
- Maximum AND minimum length bounds enforced (byte vs. character distinction)
- Numeric inputs validated for range (prevent integer overflow)
- Regular expressions reviewed for catastrophic backtracking (ReDoS)

### Canonicalization & Encoding Normalization
Validate AFTER decoding all layers. Attack vectors include:
- URL encoding: `%2e%2e%2f` for `../` in path traversal
- HTML entities: `&#60;` for `<` in XSS
- Unicode normalization: Different byte sequences for same character → normalize to NFC
- Double encoding: `%252f` → `%2f` → `/` — iterative decoding until stable
- Base64: Encoded malicious content — decode all layers, validate innermost

### File Upload Validation
- Content type verified by magic number/file header, NOT extension or client Content-Type
- Extension checked against allow-list; no executable extensions permitted
- Content scanned for embedded scripts and malware
- Per-file and aggregate size limits enforced BEFORE processing
- Storage: outside web root, no execution permissions, generated filenames only

### Email & URL Validation
- Email: RFC-compliant parsing with practical subset + DNS verification
- URLs for display: Parse, scheme allow-list (no `javascript:` / `data:`), IDN homograph protection
- URLs for server-side fetch: DNS resolution + IP blocklist (private ranges) + port restrictions → SSRF prevention through DNS rebinding and IPv6 bypass awareness

---

## 2. Output Encoding
**Automation Potential: High** — template analysis

### Context-Specific Encoding Rules
Output encoding MUST match the context where data is rendered:

| Context | Encoding Required | Characters to Encode |
|---------|-------------------|---------------------|
| HTML body | HTML entity encoding | `<`, `>`, `&`, `"` |
| HTML attribute (quoted) | HTML entity encoding | `"`, `'`, `<`, `>`, `&` |
| HTML attribute (unquoted) | Avoid — requires space encoding too | All above + whitespace |
| CSS value | CSS escape encoding | Quotes, backslash, control characters |
| JavaScript string | JavaScript escape encoding | `'`, `"`, `\`, newlines, control chars |
| URL in attribute | URL percent-encoding THEN HTML encoding | Per URL spec, then `&` → `&amp;` |

### XSS Prevention Specifics
- Use `textContent`, never `innerHTML` for dynamic HTML insertion
- Use framework auto-escaping (React, Angular, Vue) — verify it's context-aware
- Disable CSS expressions/`-moz-binding` via CSP
- URL scheme allow-list before navigation (no `javascript:` protocol)

### Content Security Policy (CSP)
CSP is defense-in-depth, NOT a replacement for output encoding.

| Directive | Secure Configuration |
|-----------|---------------------|
| `default-src` | `'none'` or `'self'` |
| `script-src` | `'self'` + specific trusted domains + `'nonce-{random}'` or `'sha256-{hash}'` for inline |
| `style-src` | `'self'`, avoid `'unsafe-inline'` |
| `connect-src` | `'self'` + specific API endpoints |
| `frame-ancestors` | `'none'` or specific framing origins |
| `form-action` | `'self'` |

### Template Engine Safety
- Auto-escaping: verify enabled AND context-aware
- Unescape directives (`|safe`, `|raw`, `{{{}}}`): require explicit security review + documented justification
- User-defined templates: sandboxed execution or prohibition (SSTI risk)

### Serialization Safety
- JSON: Use `JSON.stringify()`, proper `Content-Type`, no `</script>` in output — never `eval()` JSON
- JSONP: Avoid entirely — use CORS instead
- XML: Disable external entities (XXE), schema validation, safe parsing libraries

---

## 3. Authentication & Session Management
**Automation Potential: Medium** — configuration review

### Password Storage

| Algorithm | Parameters | Notes |
|-----------|-----------|-------|
| Argon2id | Memory: 64+ MB, iterations: 3+, parallelism: 4+ | PHC winner, preferred |
| bcrypt | Cost factor: 10+ (calibrate to ~250ms) | Widely supported |
| PBKDF2 | 100,000+ iterations, SHA-256+ | NIST-approved |

Requirements: unique salt per password, server-side hashing only, no plaintext or reversible encryption.

### Password Policy (NIST 800-63B Aligned)
- Minimum 12-16 characters (length > complexity for entropy)
- Reject passwords found in breached credential databases (Have I Been Pwned API)
- No forced periodic rotation (unless compromise suspected)
- No arbitrary complexity rules (uppercase + number + symbol)

### Credential Recovery
- Token: cryptographically random, 128+ bits, single-use, short-lived (15-60 min)
- Delivery: verified channel only (pre-registered email/SMS), no channel change on request
- Post-reset: alert sent to all registered channels

### Session Security
- Token entropy: minimum 128 bits from CSPRNG, server-side generation only
- Regenerate session ID on authentication and privilege elevation
- Absolute timeout: 8-24 hours; idle timeout: 15-30 minutes for sensitive apps
- Concurrent session limit: 1-3 per user
- Cookie attributes: `Secure`, `HttpOnly`, `SameSite=Strict` (or Lax + CSRF tokens), `__Host-` prefix

### CSRF Protection
- Synchronizer token pattern (stateful apps, default recommendation)
- Double-submit cookie (stateless APIs, SPAs)
- SameSite cookies as defense-in-depth, not standalone

### MFA Priority (by security strength)
1. FIDO2/WebAuthn security keys, passkeys (phishing-resistant)
2. TOTP authenticator apps (time-based, no network dependency)
3. Push notification with number matching (requires app possession)
4. SMS/email OTP (lowest — vulnerable to interception, avoid if possible)

---

## 4. Access Control
**Automation Potential: Medium** — code structure analysis

### Authorization Patterns

**RBAC** — Balance role granularity: too few roles = excessive permissions, too many = role explosion. Implement role hierarchy, separation of duties (mutually exclusive roles for sensitive operations).

**ABAC** — Evaluate policies based on subject attributes (department, clearance), resource attributes (classification, owner), action attributes (read/write), and environment attributes (time, location, device posture).

### Policy Enforcement Points (layered)
1. **API gateway/edge**: Rate limiting, authentication, coarse-grained routing
2. **Application middleware**: RBAC/ABAC authorization, audit logging
3. **Service layer**: Domain-specific authorization, data filtering
4. **Data access layer**: Query modification, row-level security, result filtering

### Administrative Function Protection
- Network restriction (VPN, private segments, IP allow-lists)
- Enhanced authentication (MFA required, step-up for sensitive operations)
- Explicit admin role verification (not just "logged in")
- Comprehensive action logging with real-time alerting
- Dual authorization / change approval for critical operations
- Shorter session timeouts, re-authentication for sensitive actions

### API Endpoint Authorization
- Resource-level: verify ownership/permission for every resource instance
- Field-level: filter response fields by user permissions (critical for GraphQL)
- Scope-based: OAuth 2.0 scopes limiting permitted operations
- Rate limiting per user/resource to prevent enumeration

---

## 5. Cryptographic Practices
**Automation Potential: High** — cryptographic API detection

### Encryption at Rest
- Algorithm: AES-256-GCM (authenticated encryption) — NOT CBC without MAC, NEVER ECB
- Key management: separate from encrypted data, HSM/KMS preferred
- IV/nonce: unique per encryption, 96-bit for GCM — nonce reuse is catastrophic
- Authentication tag: verified on decryption, failure = reject

### Encryption in Transit
- TLS 1.3 preferred, 1.2 minimum — SSLv3, TLS 1.0, TLS 1.1 are deprecated
- Cipher suites: AEAD with PFS (ECDHE, DHE) — no RSA key exchange, CBC without AEAD, RC4, 3DES
- Full certificate chain validation, hostname match, no bypass
- HSTS: max-age 1+ year, includeSubDomains, preload

### Key Lifecycle
- **Generation**: CSPRNG, HSM-backed when possible
- **Storage**: HSM, KMS, encrypted key stores with separate KEK — never plaintext
- **Rotation**: Automated, graceful re-encryption, emergency procedures defined
- **Destruction**: Cryptographic erasure, physical destruction for HSM

### Cryptographic Agility
- Versioned ciphertext (algorithm identifier in encrypted data format)
- Automated re-encryption for migration to new algorithms
- Defined sunset timelines for algorithm deprecation

### Random Number Security
- Cryptographic keys/tokens/nonces: CSPRNG only (`/dev/urandom`, `getrandom()`, `CryptGenRandom`)
- NEVER use `rand()`, `Math.random()`, `Random` for security-sensitive values

---

## 6. Error Handling & Logging
**Automation Potential: Medium** — logging pattern analysis

### Secure Error Handling
- User-facing: generic acknowledgment ("An error occurred") + reference code for support
- Internal logging: full exception type, stack trace, request context, correlation ID
- System details (framework, version, configuration) NEVER exposed to users

### Structured Exception Handling
- Try-catch-finally / using / try-with-resources for guaranteed cleanup
- Global exception handlers for centralized logging + consistent generic user responses
- Circuit breakers for cascading failure prevention (fail-fast, graceful degradation)
- Custom security exception types to distinguish security failures from operational errors

### Resource Cleanup Guarantees
Every failure path must clean up: database connections (return to pool), file handles (close + delete temp), network sockets (close + deregister), cryptographic resources (clear sensitive material from memory), locks/mutexes (release in finally blocks).

### Logging Requirements
- Log all: authentication events, authorization failures, input validation rejections, data access, administrative actions, system events
- Format: structured JSON with UTC timestamps (millisecond precision), correlation IDs, session context
- NEVER log: passwords, tokens, session IDs in plaintext, PII, credit card numbers
- Integrity: append-only storage, cryptographic hashing per record, tamper detection

---

## 7. Data Protection & Privacy
**Automation Potential: Low** — requires business context

### Data Classification

| Level | Examples | Handling |
|-------|----------|---------|
| Public | Marketing materials | No special protection |
| Internal | Business processes | Access control, no external distribution |
| Confidential | Customer data, financials | Encryption, need-to-know, audit logging |
| Restricted | Credentials, keys, PII, PHI | Encryption, HSM, strict access, monitoring |

### Masking & Tokenization
- **Masking** (display): Partial display for verification (e.g., `****-****-****-1234`), irreversible
- **Tokenization** (reversible): Substitute with token, vault-based retrieval, PCI DSS compliant
- **Format-preserving encryption**: Encrypted values maintain format for legacy systems
- **Anonymization** (irreversible): K-anonymity, differential privacy for analytics

### GDPR/CCPA Technical Requirements
- Consent management platform, processing records (GDPR Art. 6)
- Self-service data access portal, export in standard formats (GDPR Art. 15/20)
- Cascading deletion with backup handling (GDPR Art. 17 / CCPA 1798.105)
- Automated breach detection + notification workflows (GDPR Art. 33-34)
- Data minimization: collect only with defined purpose, enforce retention periods, automated deletion

### Retention & Deletion
- Defined periods by data category with legal hold procedures
- Technical enforcement: database TTL, object lifecycle policies, automated deletion logging
- Backup handling: retention-aligned cycles, encryption key destruction for expired data
- Third-party propagation: data processing agreements, vendor deletion confirmation

---

## 8. Communication Security
**Automation Potential: High** — configuration analysis

### Certificate Practices
- Static pinning deprecated (HPKP) — use Certificate Transparency monitoring + Expect-CT
- mTLS for service-to-service: automated provisioning, short validity, OCSP stapling
- Certificate-to-principal identity mapping for mTLS

### OAuth 2.0 / OpenID Connect
- Authorization code flow with PKCE for ALL clients (including confidential)
- Implicit flow: DEPRECATED — use code flow + PKCE
- Token validation: signature, issuer, audience, expiration, nonce — ALL checked
- Refresh tokens: rotation, client binding, sender-constrained (DPoP)

### API Rate Limiting
- Per-identifier (user, IP, API key): token bucket or sliding window
- Per-resource: endpoint-specific limits for expensive operations
- Per-tier: different limits by subscription
- Adaptive: dynamic reduction under load
- Response headers: `X-RateLimit-*` or RFC 6585 for client visibility

---

## 9. System Configuration
**Automation Potential: High** — infrastructure-as-code scanning

### Environment Separation

| Environment | Key Characteristic | Security Control |
|-------------|-------------------|-----------------|
| Development | Verbose logging, debug enabled | No production data, isolated network |
| Testing/QA | Production-like, anonymized data | Data masking, no external access |
| Staging | Production-identical | Final security verification |
| Production | Minimal logging, debug disabled | Comprehensive monitoring, no direct access |

### Secrets Management
- Dynamic secrets: on-demand generation, automatic expiration, lease-based
- Automated rotation with graceful transition and emergency procedures
- Runtime injection (not persisted) — NEVER hardcoded in source or committed to VCS
- Comprehensive access auditing with anomaly detection
- HSM-backed encryption at rest with key hierarchy

---

## 10. Database Security
**Automation Potential: High** — query pattern analysis

### Parameterized Query Enforcement (all layers)
- ORM/framework: safe query builders, never raw SQL methods
- Database driver: prepared statement APIs, no string concatenation
- Static analysis: automated detection of query construction anti-patterns in CI
- Runtime monitoring: query pattern analysis, anomaly detection

### ORM Safety

| Safe Patterns | Unsafe Anti-Patterns |
|---------------|---------------------|
| LINQ-style query builders | `FromSqlRaw`, `createSQLQuery` |
| Criteria APIs | SQL fragments in order/sort clauses |
| Entity manager find methods | Dynamic query construction features |
| Named queries with parameter binding | String concatenation in query building |

### Stored Procedure Security
- Static SQL with parameterized inputs only — no `EXECUTE IMMEDIATE` with concatenation
- Least privilege: execute permissions on specific procedures, not direct table access
- Input validation within procedures for parameters influencing query structure

### Least Privilege Database Accounts
- Application service: EXECUTE on required procedures, DML on specific tables only
- Reporting: SELECT on specific views/columns, no direct table access
- Migration: DDL during deployment windows only, not persistent
- Connection pool: integrated with secrets management, health validation, short timeouts

---

## 11. File & Resource Management
**Automation Potential: Medium** — API usage patterns

### Path Traversal Prevention (layered)
1. Input validation: allow-list allowed paths, reject traversal sequences
2. Canonicalization: resolve to absolute path, verify within allowed base directory
3. Chroot/jail: restrict filesystem visibility
4. Sandboxing: container, seccomp, AppArmor

### Secure Temporary Files
- Unpredictable filename (cryptographically random)
- Exclusive creation (atomic create-without-replace, fail if exists)
- Restricted permissions (0600, owner-only)
- Automatic cleanup (delete on close + scheduled cleanup + secure erase)

### Resource Limits
- Upload: per-file + aggregate size, content-type allow-list, generated filenames
- Memory: heap limits (JVM `-Xmx`, container limits), stack size limits
- CPU: quotas, cgroup limits (prevent algorithmic complexity attacks)
- File descriptors: `ulimit`, per-process limits
- Network connections: connection limits, rate limiting (prevent Slowloris)

---

## 12. Memory Management
**Automation Potential: High** — for native code (C/C++/Rust)

### Buffer Safety
- C/C++: static analysis + runtime checks + safe libraries (`std::vector`, `std::string`)
- Safe replacements: `strncpy`/`strlcpy` for `strcpy`, `snprintf` for `sprintf`, `fgets` for `gets`
- Network/protocol parsing: length-prefixed, explicit bounds, fuzz tested

### Use-After-Free Prevention
- C++: smart pointers (`unique_ptr`, `shared_ptr`)
- Rust: borrow checker (compile-time lifetime verification)
- C/C++ testing: AddressSanitizer for runtime detection
- Manual C: null pointers after free, defensive programming

### Memory Initialization & Cleanup
- Initialize on allocation (`calloc`, constructor guarantees) — prevents info leaks from prior allocations
- Secure cleanup: `memset_s`, `SecureZeroMemory` — NOT plain `memset` (compiler may optimize away)
- Compiler barriers to prevent optimization of "unnecessary" clears

---

## 13. Business Logic Security
**Automation Potential: Low** — requires domain understanding

### Transaction Integrity
- State machine validation: enforce valid operation sequences, prevent skipped steps
- Invariant checking: non-negative prices, sufficient balances, transaction boundary validation
- Idempotency: unique request identifiers, outcome checking before processing
- Anomaly detection: behavioral baselines, deviation alerting

### Domain-Specific Requirements

| Domain | Key Requirements | Regulatory Drivers |
|--------|-----------------|-------------------|
| Financial | Transaction integrity, fraud prevention, audit trails | PCI DSS, SOX |
| Healthcare | Patient privacy, data integrity, access logging | HIPAA, GDPR |
| Critical Infrastructure | Availability, safety, adversarial resilience | NERC CIP, IEC 62443 |
| Government | Classification-based access, auditability, data sovereignty | FISMA, FedRAMP |

---

## 14. Dependency & Supply Chain Hygiene
**Automation Potential: High** — SCA tooling

### SBOM Requirements
- Component inventory: all direct and transitive dependencies (SPDX or CycloneDX format)
- Version identification: Package URL (PURL), CPE for vulnerability correlation
- License tracking: SPDX license expressions for compliance
- Dependency graph: transitive dependency tree for impact analysis
- Provenance: SLSA attestations for supply chain integrity

### Deprecated & EOL Management
- **Library Deprecation**: Monitor for "deprecated" status in package manager registry or official documentation.
- **End-of-Life (EOL)**: Track component release dates and EOL support windows; replace components reaching EOL at least 6 months before support ends.
- **Legacy Code Debt**: Identify and refactor code using deprecated APIs or language features that are no longer receiving security patches.
- **Vulnerability Density**: Flag components with a high history of critical CVEs, even if currently patched, as candidates for replacement.

### Scanning Integration Points
- **Development**: IDE plugins, pre-commit hooks (immediate feedback)
- **Build**: SCA in CI pipeline (Snyk, OWASP Dependency-Check, Dependabot)
- **Deployment**: Container image scanning, runtime monitoring
- **Continuous**: Vulnerability feed monitoring, automated alerts for new disclosures

### Dependency Confusion Prevention
- Reserve organization namespace prefixes in public registries
- Configure package managers to check private registries first
- Pin repository sources explicitly in package manager configs
- Checksum + signature verification for all dependencies
- Dependency firewall tools (Sonatype Nexus Firewall, JFrog Xray)
