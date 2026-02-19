# ASVS 5.0 — Verification Standard Reference

## Overview

The Application Security Verification Standard (ASVS) provides granular, testable security
requirements organized by domain. It bridges the gap between high-level awareness (OWASP
Top 10) and practical engineering verification.

## Verification Levels

### Level 1 — Opportunistic (All Applications)
- Bare minimum for every web application
- Defends against easily exploitable, common vulnerabilities
- Verifiable through automated scanning + basic manual testing
- Strategically aligned with OWASP Top 10 coverage
- **Use when:** Starting a security program, assessing low-risk internal tools

### Level 2 — Standard (Sensitive Data Applications)
- Recommended default for commercial and enterprise applications
- Handles PII, business-critical logic, financial transactions
- Requires architecture review + manual code review + rigorous testing
- Covers authorization, data protection, session management in depth
- **Use when:** Building apps that handle customer data, payment info, health records

### Level 3 — Advanced (Critical Infrastructure)
- Reserved for high-assurance environments (healthcare, finance, defense, critical infra)
- Requires full source access + architecture documentation + threat modeling
- Tests resilience against advanced persistent threats (APTs)
- Includes supply chain verification and operational security
- **Use when:** The failure of the application could cause catastrophic harm

## Key Chapters and Developer Requirements

### V1: Architecture, Design and Threat Modeling
- Conduct a formal threat model for all critical features
- Document trust boundaries between components
- Verify that all security controls are centralized and reusable
- Ensure no business logic can be bypassed through parameter manipulation
- **Level applicability:** L1 (basic), L2 (thorough), L3 (comprehensive)

### V2: Authentication
- Verify all passwords against lists of breached credentials
- Require MFA for all sensitive operations and admin access
- Implement account lockout with progressive delays
- Align with NIST 800-63b password guidelines (length-based, no arbitrary complexity)
- Store passwords with Argon2id, bcrypt, or scrypt with proper tuning parameters

### V3: Session Management
- Ensure session tokens are generated using cryptographically secure random sources
- Regenerate session IDs after authentication and privilege changes
- Set proper session timeouts (idle and absolute)
- Ensure session tokens are invalidated on logout
- Protect session cookies with Secure, HttpOnly, and SameSite attributes

### V4: Access Control
- Enforce access control rules at the server-side, never just client-side
- Implement deny-by-default for all resources
- Centralize authorization logic in a trusted service layer
- Verify that admin functions are segregated from regular user capabilities
- Ensure CORS policies are restrictive and explicitly configured

### V5: Validation, Sanitization and Encoding
- Use parameterized queries for all database interactions
- Apply positive (allow-list) input validation on all user input
- Use context-specific output encoding (HTML, JS, URL, CSS contexts)
- Validate file uploads by content type, not just extension
- Sanitize all data used in dynamic SQL, LDAP, XML, or OS commands

### V6: Stored Cryptography
- Use up-to-date cryptographic algorithms (AES-256, RSA-2048+, Ed25519)
- Store keys in hardware security modules (HSMs) or dedicated KMS
- Implement key rotation policies with defined frequencies
- Never use deprecated algorithms (MD5, SHA1, DES, RC4)
- Verify that random number generation uses CSPRNG

### V7: Error Handling and Logging
- Implement a centralized error handling mechanism
- Ensure error messages do not leak technical details to users
- Log security-relevant events with sufficient context for investigation
- Protect log integrity — prevent tampering and unauthorized access
- Never log sensitive data (credentials, tokens, PII, card numbers)

### V8: Data Protection
- Encrypt all sensitive data at rest using strong algorithms (AES-256)
- Classify data by sensitivity and apply appropriate protection levels
- Implement data retention and disposal policies
- Ensure sensitive data is not cached or stored in browser history
- Apply HTTP security headers (Strict-Transport-Security, Content-Security-Policy)

### V9: Communications
- Enforce TLS 1.2 or higher for all connections
- Validate server certificates properly (no self-signed in production)
- Implement certificate pinning for high-security mobile applications
- Use HSTS with includeSubDomains and preload directives
- Verify that fallback to insecure protocols is not possible

### V10: Malicious Code Verification
- Verify that the application does not contain time bombs, backdoors, or Easter eggs
- Verify that third-party code does not contain malicious functionality
- Review client-side code for obfuscated or suspicious behavior
- Implement Subresource Integrity (SRI) for all external scripts

### V13: API and Web Service
- Validate all API requests against a strictly defined schema
- Implement proper authentication for all API endpoints
- Apply rate limiting per-client and per-endpoint
- Use API versioning to manage breaking changes safely
- Validate Content-Type headers and reject unexpected formats
- Return proper HTTP status codes with minimal information disclosure

### V14: Configuration
- Remove all unnecessary features, documentation, and sample applications
- Verify that server configuration is hardened per vendor recommendations
- Automate configuration validation in deployment pipelines
- Ensure that all environments (dev, staging, prod) have equivalent security configurations
- Verify that security headers are present and correctly configured

## Using ASVS in Practice

### As a CI/CD Gate
Map ASVS Level 1 requirements to automated SAST/DAST tool rules. Fail builds
that introduce Level 1 violations. Use Level 2 requirements as the basis for
manual security review checklists during sprint reviews.

### As a Procurement Standard
Require vendors to self-attest ASVS Level 1 compliance for low-risk integrations,
and commission third-party ASVS Level 2 assessments for vendors handling sensitive data.

### As a Security Review Checklist
Use the chapter structure to organize code review findings. Each ASVS requirement
can be treated as a pass/fail assertion during a security review.
