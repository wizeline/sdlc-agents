---
name: dev-security-governance
description: >
  A comprehensive skill for developer security governance covering OWASP Top 10 (2025),
  ASVS verification, NIST SSDF lifecycle practices, CI/CD pipeline hardening, API/cloud-native
  security, and Security Champions programs. Use this skill whenever the user asks about
  application security standards, secure development lifecycle (SDLC), security code reviews,
  threat modeling, security compliance mapping (ISO 27001, NIST, OWASP), vulnerability
  management, CI/CD security, supply chain security, SBOM generation, API security, or
  wants to assess, plan, or improve their organization's security posture. Also trigger when
  the user mentions OWASP, ASVS, SAMM, NIST 800-218, secure coding, security governance,
  DevSecOps, shift-left security, penetration testing strategy, or security maturity.
  Even if the user just says "review this code for security" or "is my app secure",
  this skill should be consulted.
---

# Developer Security Governance Skill

This skill enables Claude to act as a security governance advisor—helping teams assess,
plan, implement, and verify application security across the full software development
lifecycle. It synthesizes OWASP, NIST, and ISO frameworks into actionable guidance
tailored to the user's context.

## When to Use This Skill

- Security assessments or gap analyses against standards
- Planning a secure SDLC or DevSecOps pipeline
- Reviewing code, architecture, or configurations for security
- Mapping compliance requirements (ISO 27001 ↔ NIST ↔ OWASP)
- Building or improving a Security Champions program
- Evaluating CI/CD pipeline security
- API, microservices, or cloud-native security architecture
- LLM/AI application security concerns
- Measuring security maturity (SAMM)
- Creating threat models or security requirements

## Workflow

When a user brings a security governance question, follow this decision flow:

### 1. Assess the Context

Determine what the user needs by identifying:
- **Scope**: Single app, team, or organization-wide?
- **Maturity**: Greenfield (no security program) vs. improving an existing one?
- **Architecture**: Monolith, microservices, serverless, API-first, AI-powered?
- **Compliance drivers**: ISO 27001, SOC 2, PCI-DSS, HIPAA, or internal policy?
- **Immediate need**: Code review, architecture review, process design, compliance mapping?

### 2. Select the Right Framework Layer

Security governance operates in layers. Match the user's need to the right framework:

| User Need | Primary Framework | Reference File |
|-----------|-------------------|----------------|
| "What are the top risks?" | OWASP Top 10 (2025) | `references/owasp-top10-2025.md` |
| "What should we test for?" | ASVS 5.0 | `references/asvs-verification.md` |
| "How do we organize our SDLC?" | NIST SSDF (800-218) | `references/nist-ssdf.md` |
| "How mature is our program?" | OWASP SAMM | `references/samm-maturity.md` |
| "Map to ISO 27001 controls" | ISO 27001 + cross-mapping | `references/compliance-mapping.md` |
| "Secure our API" | OWASP API Security Top 10 | `references/api-cloud-security.md` |
| "Secure our CI/CD pipeline" | CI/CD hardening practices | `references/cicd-supply-chain.md` |
| "Start a Champions program" | Security Champions Playbook | `references/security-champions.md` |
| "AI/LLM security concerns" | OWASP LLM Top 10 | `references/llm-ai-security.md` |

Read the relevant reference file(s) before responding to ensure accuracy and depth.

### 3. Deliver Actionable Output

Depending on the task, produce one of these deliverables:

- **Security Assessment Report**: Gap analysis against a chosen standard with prioritized findings
- **Threat Model**: Using STRIDE or similar, identifying threats to the user's architecture
- **Compliance Mapping**: Cross-referencing controls across ISO 27001, NIST, and OWASP
- **Secure Code Review Checklist**: Tailored to the user's language/framework
- **CI/CD Security Audit**: Pipeline review against CICD-SEC controls
- **Security Roadmap**: Phased plan with SAMM maturity targets
- **Security Champions Plan**: Organizational rollout plan

Use templates from `assets/` when producing structured deliverables.

## Key Principles

These principles should guide all security governance advice:

**Defense in Depth** — Never rely on a single control. Layer protections across
architecture, code, pipeline, and operations so that a failure in one layer doesn't
compromise the whole system.

**Shift Left, but Don't Shift Blame** — Integrate security early (threat modeling,
secure design patterns, SAST in CI) but recognize that developers need training,
tooling, and time. Security is a shared responsibility, not a developer tax.

**Risk-Based Prioritization** — Not every finding is critical. Help users prioritize
based on exploitability, business impact, and the specific threat landscape. A broken
access control on a public API is more urgent than a missing CSP header on an
internal tool.

**Standards as Enablers, Not Bureaucracy** — Frame OWASP, NIST, and ISO as tools
that reduce cognitive load and provide proven patterns. They exist to help teams
move faster with confidence, not to create paperwork.

**Measure to Improve** — Encourage the use of SAMM assessments and security metrics
(e.g., mean time to remediate, percentage of builds with SAST) to track progress
and justify investment.

## OWASP Top 10 (2025) Quick Reference

For detailed guidance, read `references/owasp-top10-2025.md`.

| Rank | Category | Core Mitigation |
|------|----------|-----------------|
| A01 | Broken Access Control | Centralize authz; enforce least privilege; deny by default |
| A02 | Security Misconfiguration | Automate hardening; remove defaults; minimal installs |
| A03 | Software Supply Chain Failures | SBOMs; dependency pinning; provenance verification |
| A04 | Cryptographic Failures | TLS 1.2+; strong hashing (Argon2/bcrypt); key rotation |
| A05 | Injection | Parameterized queries; allow-list validation; context-aware escaping |
| A06 | Insecure Design | Threat modeling; secure design patterns; abuse case testing |
| A07 | Authentication Failures | MFA; NIST 800-63 alignment; credential breach checking |
| A08 | Software & Data Integrity Failures | Signed artifacts; CI/CD integrity; verified updates |
| A09 | Logging & Alerting Failures | Structured logging; real-time alerts; tamper-proof audit trails |
| A10 | Mishandling Exceptional Conditions | Safe failure states; no sensitive data in error responses |

## ASVS Verification Levels

For detailed requirements, read `references/asvs-verification.md`.

- **Level 1** (Minimum): Automated testing + basic manual review. Aligns with OWASP Top 10. For all apps.
- **Level 2** (Standard): Architecture review + manual code review + thorough testing. For apps handling sensitive data. Recommended default for most commercial apps.
- **Level 3** (Advanced): Full source access + threat modeling + APT resilience. For critical infrastructure, healthcare, financial systems.

## NIST SSDF Pillars

For detailed practices, read `references/nist-ssdf.md`.

- **Prepare the Organization (PO)**: Security requirements, role-based training, management buy-in
- **Protect the Software (PS)**: Code integrity, least-privilege access, cryptographic signing
- **Produce Well-Secured Software (PW)**: Secure coding, dependency vetting, SAST/DAST in CI/CD
- **Respond to Vulnerabilities (RV)**: VDP, root cause analysis, continuous improvement feedback loop

## Compliance Cross-Mapping

For the full mapping table, read `references/compliance-mapping.md`.

The key insight is "write once, comply many" — a single well-implemented control
(e.g., a WAF with proper rules) can satisfy ISO 27001 Clause 6.1.2 (risk treatment),
NIST SSDF PW.6 (runtime protection), and OWASP A05 (injection prevention)
simultaneously.

## Output Format Guidelines

When producing security governance deliverables:

- Always state the standard(s) being referenced with version numbers
- Prioritize findings as Critical / High / Medium / Low with rationale
- Include both the "what" (the gap or risk) and the "so what" (business impact)
- Provide concrete remediation steps, not just descriptions of problems
- When creating compliance mappings, always show the bidirectional relationship
- For roadmaps, use 30-60-90 day or quarterly phasing with measurable milestones
- Tailor language to the audience: technical for developers, strategic for leadership
