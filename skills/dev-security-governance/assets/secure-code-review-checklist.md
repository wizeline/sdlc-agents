# Secure Code Review Checklist

## Review Metadata
- **Application:**
- **Reviewer:**
- **Date:**
- **Files / PRs Reviewed:**
- **Language / Framework:**

## Authentication & Session Management
- [ ] Passwords checked against breached credential lists
- [ ] MFA implemented for sensitive operations
- [ ] Session tokens generated with CSPRNG
- [ ] Session IDs regenerated after authentication
- [ ] Session timeout (idle and absolute) configured
- [ ] Secure cookie attributes set (Secure, HttpOnly, SameSite)

## Access Control
- [ ] Authorization enforced server-side (not just client-side)
- [ ] Deny-by-default for all resources
- [ ] No direct object references without authorization check
- [ ] Admin functions segregated from user functions
- [ ] CORS policy restrictive and explicitly configured

## Input Validation & Output Encoding
- [ ] All database queries use parameterized statements
- [ ] Input validation uses positive (allow-list) approach
- [ ] Output encoding is context-specific (HTML, JS, URL, CSS)
- [ ] File uploads validated by content type, not just extension
- [ ] File upload size limits enforced
- [ ] No user input concatenated into commands (SQL, OS, LDAP)

## Cryptography
- [ ] Sensitive data encrypted at rest (AES-256 or equivalent)
- [ ] TLS 1.2+ enforced for all data in transit
- [ ] Passwords hashed with Argon2id, bcrypt, or scrypt
- [ ] No hard-coded keys, passwords, or secrets
- [ ] Cryptographic keys stored in KMS, not in code
- [ ] No use of deprecated algorithms (MD5, SHA1, DES, RC4)

## Error Handling & Logging
- [ ] Global exception handler catches unhandled errors
- [ ] Error messages do not leak stack traces or internal details
- [ ] Security events logged (auth failures, access denials, input violations)
- [ ] Logs do not contain sensitive data (passwords, tokens, PII)
- [ ] Log format is structured (JSON) for aggregation

## Data Protection
- [ ] PII handled according to data classification policy
- [ ] Sensitive data not stored in URLs, query strings, or browser storage
- [ ] HTTP security headers configured (HSTS, CSP, X-Content-Type-Options)
- [ ] No sensitive data in client-side code or comments

## Dependency Security
- [ ] No known vulnerable dependencies (SCA scan clean)
- [ ] Dependencies pinned to specific versions
- [ ] Lockfile present and committed
- [ ] Third-party scripts use Subresource Integrity (SRI)

## Business Logic
- [ ] Rate limiting applied to sensitive operations
- [ ] Business rules enforced server-side
- [ ] Abuse cases / negative test cases considered
- [ ] No race conditions in critical workflows

## Notes & Findings
<!-- Document any findings, questions, or deferred items -->
