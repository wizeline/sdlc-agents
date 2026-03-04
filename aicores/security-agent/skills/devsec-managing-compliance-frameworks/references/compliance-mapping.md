# Compliance Cross-Mapping — ISO 27001, NIST, and OWASP

## Overview

This reference provides bidirectional mappings between major security frameworks.
The goal is "write once, comply many" — implement a single strong control and
satisfy requirements across multiple standards simultaneously.

## ISO 27001:2022 to OWASP/NIST Mapping

| ISO 27001:2022 Control | Focus Area | OWASP Standard | NIST SSDF Practice |
|------------------------|------------|----------------|--------------------|
| 5.2: Information Security Policy | Strategy | SAMM Governance | PO.1 |
| 5.8: Information Security in Project Mgmt | SDLC Integration | SAMM Design | PO.1, PW.1 |
| 5.21: Supplier ICT Security | Supply Chain | A03:2025 (Supply Chain) | PW.4 |
| 5.23: Information Security for Cloud Services | Cloud Security | ASVS V14, API Top 10 | PS.1, PW.8 |
| 6.3: Information Security Awareness/Training | People | Security Champions | PO.2, PO.3 |
| 8.9: Configuration Management | Hardening | A02:2025 (Misconfig) | PW.8 |
| 8.15: Logging | Observability | A09:2025 (Logging) | RV.1 |
| 8.16: Monitoring Activities | Detection | A09:2025 (Logging) | RV.1 |
| 8.24: Use of Cryptography | Data Protection | A04:2025 (Crypto), ASVS V6 | PW.5 |
| 8.25: Secure Development Lifecycle | SDLC | SAMM, Proactive Controls | PO, PS, PW, RV (all) |
| 8.26: Application Security Requirements | Design | ASVS V1, A06:2025 | PW.1, PW.2 |
| 8.28: Secure Coding | Implementation | Proactive Controls, ASVS V5 | PW.5 |
| 8.29: Security Testing | Verification | SAMM Verification, ASVS | PW.7 |
| 8.31: Separation of Environments | Infrastructure | ASVS V14 | PO.5 |
| 8.33: Test Information | Data Protection | ASVS V8 | PW.8 |

## NIST CSF Functions to OWASP Mapping

| NIST CSF Function | OWASP Top 10 Categories | OWASP Framework |
|-------------------|--------------------------|-----------------|
| **Identify** | A03 (Supply Chain), A06 (Insecure Design) | SAMM Governance, ASVS V1 |
| **Protect** | A01 (Access Control), A04 (Crypto), A05 (Injection), A07 (Auth) | ASVS V2-V6, Proactive Controls |
| **Detect** | A09 (Logging), A10 (Exceptions) | ASVS V7, SAMM Verification |
| **Respond** | A09 (Logging) | SAMM Operations, ASVS V7 |
| **Recover** | A08 (Integrity), A09 (Logging) | SAMM Operations |

## "Write Once, Comply Many" Examples

### Example 1: Web Application Firewall (WAF)
A properly configured WAF simultaneously satisfies:
- **ISO 27001** Clause 6.1.2 (Risk Treatment) — technical risk mitigation control
- **ISO 27001** Control 8.26 (Application Security Requirements) — runtime protection
- **NIST SSDF** PW.7 (Test Code for Vulnerabilities) — virtual patching capability
- **OWASP A05** (Injection) — defense-in-depth layer against injection attacks
- **OWASP A02** (Security Misconfiguration) — when WAF enforces security headers

### Example 2: SAST in CI/CD Pipeline
Integrating static analysis into the build pipeline satisfies:
- **ISO 27001** Control 8.28 (Secure Coding) — automated code quality enforcement
- **ISO 27001** Control 8.29 (Security Testing) — continuous testing
- **NIST SSDF** PW.5 (Secure Coding Practices) — tooling support
- **NIST SSDF** PW.7 (Review and Test Code) — automated vulnerability detection
- **OWASP A05** (Injection) — early detection of injection patterns
- **OWASP SAMM** Verification — automated security testing maturity

### Example 3: Multi-Factor Authentication
Deploying MFA satisfies:
- **ISO 27001** Control 8.5 (Secure Authentication) — strong authentication
- **NIST 800-63b** — identity assurance level 2
- **NIST SSDF** PO.5 (Secure Environments) — access control for dev infrastructure
- **OWASP A07** (Authentication Failures) — primary mitigation
- **ASVS V2** (Authentication) — MFA requirement

## Gap Analysis Approach

When conducting a gap analysis across these frameworks:

1. **Start with the compliance driver** — identify which standard is mandatory
   (e.g., ISO 27001 for certification, NIST SSDF for US government contracts)
2. **Map current controls** — inventory existing security controls and map them
   to the primary standard's requirements
3. **Cross-reference** — for each implemented control, check which additional
   standards it satisfies using the mapping tables above
4. **Identify gaps** — find requirements with no corresponding control
5. **Prioritize by overlap** — implement controls that satisfy the most
   requirements across the most frameworks first
6. **Document the mapping** — maintain a living document that shows control-to-requirement
   relationships for auditors and stakeholders
