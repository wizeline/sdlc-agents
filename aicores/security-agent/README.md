# Security Agent — AI Core

> **AI Core** | Developer Security Governance | From code review to compliance, threat modeling to DevSecOps pipelines.

The **Security Agent** is a comprehensive developer security governance AI Core. It covers the full spectrum of application security: reviewing code for vulnerabilities, modeling threats, designing secure architectures, hardening CI/CD pipelines, managing compliance frameworks, and building organization-wide security programs.

---

## Architecture Overview

```
security-agent/
├── agents/
│   ├── devsec-architecture.md       ← Secure API, cloud-native & AI/LLM system design
│   ├── devsec-code-review.md        ← OWASP/ASVS-aligned secure code review
│   ├── devsec-compliance-framework.md ← ISO 27001, SOC 2, PCI-DSS, NIST SSDF mapping
│   ├── devsec-ops-pipeline.md       ← SAST/DAST/SCA, CI/CD security gates, SBOM
│   ├── devsec-program.md            ← AppSec program, SAMM maturity, Security Champions
│   └── devsec-threat-modeling.md    ← STRIDE threat modeling, attack surface analysis
└── skills/
    ├── devsec-reviewing-code-for-security/     ← 14 secure coding domains, OWASP Top 10
    ├── devsec-conducting-threat-modeling/      ← STRIDE, risk register, security requirements
    ├── devsec-designing-security-architecture/ ← API/cloud/LLM security patterns
    ├── devsec-hardening-devsecops-pipelines/   ← CI/CD security tooling integration
    ├── devsec-managing-compliance-frameworks/  ← Cross-framework control mapping
    └── devsec-building-security-programs/      ← SAMM, Security Champions, roadmaps
```

---

## MCP Connections

> The Security Agent does not require MCP connections by default. All skills operate on code, architecture descriptions, and documentation you provide directly.

---

## Agents

### `devsec-code-review`

**Description:** Security-focused code reviewer with deep knowledge of OWASP Top 10 (2025), ASVS 5.0, and 14 secure coding domains. Reviews code for vulnerabilities, produces prioritized findings with in-language code fixes, and generates secure code review checklists. When the request lacks code or security scope, runs a structured **Phase 0 Intake** — asking what to review, the trust boundary, data sensitivity, ASVS target level, and specific concerns — all in one prompt before proceeding.

**Triggers:** `"review this code for security"`, `"is this code secure?"`, `"check for XSS/SQLi/CSRF"`, `"scan for deprecated libraries"`, `"identify EOL components"`, `"what ASVS level should I target?"`

**Skills loaded:** `devsec-reviewing-code-for-security`

**Examples:**

```
"Review this authentication module for security vulnerabilities"
"Is this SQL query vulnerable to injection? Show me the fix"
"Generate a secure code review checklist for our Node.js API"
"What ASVS level should we target for our healthcare app?"
"Scan this requirements.txt for deprecated or vulnerable libraries"
"Review this JWT implementation — check for common weaknesses"
```

---

### `devsec-threat-modeling`

**Description:** Threat modeling facilitator using STRIDE methodology. Identifies threats to systems and architectures, documents attack surfaces, and derives testable security requirements before code is written.

**Triggers:** `"threat model this"`, `"what could go wrong with this design?"`, `"identify threats to my API"`, `"help me do STRIDE"`, `"what are the attack surfaces?"`, `"security review of my architecture"`

**Skills loaded:** `devsec-conducting-threat-modeling`

**Examples:**

```
"Threat model this microservices architecture diagram"
"Identify attack surfaces for our new payment API"
"Run a STRIDE analysis on this authentication flow"
"What security requirements do I need for this user registration feature?"
"Produce a vulnerability map for our checkout service"
"Do an architecture security review before we start coding"
```

---

### `devsec-architecture`

**Description:** Security architect specializing in secure API design, cloud-native systems, and AI/LLM application security. Provides concrete patterns (OAuth 2.0, mTLS, zero-trust, OWASP API Top 10) — not abstract advice.

**Triggers:** `"secure my REST API"`, `"OAuth 2.0 setup"`, `"zero-trust architecture"`, `"LLM security"`, `"prompt injection prevention"`, `"OWASP LLM Top 10"`, `"RAG security"`, `"JWT security"`, `"BOLA/BFLA"`

**Skills loaded:** `devsec-designing-security-architecture`

**Examples:**

```
"Secure this REST API against the OWASP API Top 10"
"Set up OAuth 2.0 with PKCE for our mobile app"
"Design a zero-trust service mesh for our Kubernetes cluster"
"Review this LLM application for prompt injection risks"
"How do I secure a RAG pipeline to prevent unauthorized content exposure?"
"What mTLS configuration do I need for service-to-service auth?"
```

---

### `devsec-ops-pipeline`

**Description:** DevSecOps engineer specializing in integrating security tooling into CI/CD pipelines. Sets up SAST, DAST, SCA, secret scanning, container scanning, IaC scanning, SBOM generation, and security gates.

**Triggers:** `"add security to my CI/CD"`, `"set up SAST in GitHub Actions"`, `"secure my pipeline"`, `"supply chain security"`, `"generate an SBOM"`, `"scan my Dockerfile"`, `"detect secrets in commits"`, `"identify deprecated libraries"`

**Skills loaded:** `devsec-hardening-devsecops-pipelines`

**Examples:**

```
"Add SAST to our GitHub Actions pipeline — we use Python"
"Set up SCA scanning and fail the build on critical CVEs"
"Integrate OWASP ZAP into our GitLab CI staging stage"
"Generate an SBOM for our container image"
"Secure our supply chain — pin dependencies and add provenance verification"
"Set up secret scanning with Gitleaks as a pre-commit hook"
```

---

### `devsec-compliance-framework`

**Description:** Security compliance advisor mapping controls to standards (ISO 27001, SOC 2, PCI-DSS, HIPAA, NIST SSDF, OWASP ASVS, GDPR, CCPA). Produces gap analyses, control mapping tables, compliance logs, and security metrics dashboards.

**Triggers:** `"map our controls to ISO 27001"`, `"what controls do I need for SOC 2?"`, `"NIST SSDF alignment"`, `"security gap analysis"`, `"GDPR Article 32"`, `"MTTD/MTTR metrics"`, `"audit preparation"`

**Skills loaded:** `devsec-managing-compliance-frameworks`

**Examples:**

```
"Perform a gap analysis against SOC 2 Type II requirements"
"Map our existing security controls to ISO 27001:2022"
"What NIST SSDF tasks apply to our CI/CD pipeline?"
"Create a compliance log for our Q1 security activities"
"Show me a security metrics dashboard — MTTD, MTTR, and vulnerability density"
"What GDPR Article 32 technical measures do we need for EU data processing?"
```

---

### `devsec-program`

**Description:** Application security program advisor helping organizations build, mature, and sustain security programs. Runs OWASP SAMM assessments, designs Security Champions programs, creates security roadmaps, and establishes VDPs.

**Triggers:** `"how mature is our security program?"`, `"OWASP SAMM assessment"`, `"start a Security Champions program"`, `"build a security culture"`, `"security roadmap"`, `"appsec program from scratch"`, `"bug bounty"`, `"VDP"`

**Skills loaded:** `devsec-building-security-programs`, `devsec-managing-compliance-frameworks`

**Examples:**

```
"Assess our security maturity using OWASP SAMM"
"Design a Security Champions program for our 200-person engineering org"
"Build us a 12-month AppSec roadmap starting from scratch"
"Make the case to our CTO for investing in application security"
"Design a vulnerability disclosure program for our public APIs"
"How do we prioritize our security investments given limited budget?"
```

---

## Skills

### `devsec-reviewing-code-for-security`

**Description:** Structured secure code review capability covering OWASP Top 10 (2025), ASVS 5.0, and 14 secure coding domains.

**14 Secure Coding Domains:** Input validation, output encoding, authentication/session, access control, cryptography, error handling, data protection, communication security, system configuration, database security, file/resource management, memory management, business logic, dependency management.

**Output types:**

| Request                   | Output                                                         |
| ------------------------- | -------------------------------------------------------------- |
| `"Review this code"`    | Real-Time Report: prioritized findings with code fixes         |
| `"Give me a checklist"` | Tailored Secure Code Review Checklist                          |
| `"What ASVS level?"`    | Level recommendation + gap list                                |
| `"How do I prevent X?"` | Domain-specific guidance with examples                         |
| `"Step-by-step fix"`    | Remediation Guide (5 steps: Scope, Fix, Test, Prevent, Verify) |

---

### `devsec-conducting-threat-modeling`

**Description:** STRIDE-based threat modeling methodology. Identifies threats per component and data flow, prioritizes by exploitability and impact, and derives testable security requirements.

**Outputs:**

- **Threat Model Document** — System overview, trust boundaries, STRIDE inventory, risk register, security requirements
- **Vulnerability Map** — Attack surface inventory, trust boundary diagram, STRIDE table, deprecated library risks

---

### `devsec-designing-security-architecture`

**Description:** Security patterns for APIs, microservices, cloud-native, and AI/LLM systems. Covers OAuth 2.0, JWT, mTLS, PKCE, zero-trust, OWASP API Top 10, and OWASP LLM Top 10 (2025).

**Key areas:**

- **API Security** — BOLA, BFLA, rate limiting, data exposure, OWASP API Top 10
- **Cloud-Native** — Zero-trust, secrets management, least-privilege IAM
- **AI/LLM** — Prompt injection (LLM01), supply chain (LLM03), agentic systems (LLM08), RAG security

---

### `devsec-hardening-devsecops-pipelines`

**Description:** Security tooling integration across the full CI/CD pipeline lifecycle. Covers pre-commit, PR/build, staging, and release gate stages.

**Pipeline Stages:**

| Stage        | Controls                                                           |
| ------------ | ------------------------------------------------------------------ |
| Pre-commit   | Gitleaks, TruffleHog secret scanning; security linting             |
| PR / Build   | SAST, SCA (dependency + license), container scanning, IaC scanning |
| Pre-deploy   | DAST (OWASP ZAP, Nuclei), API security testing                     |
| Release Gate | Fail/warn thresholds, SBOM generation, SLSA provenance             |

---

### `devsec-managing-compliance-frameworks`

**Description:** Cross-framework control mapping and compliance tracking. Implements the "write once, comply many" principle — one control mapped to all applicable frameworks.

**Supported frameworks:** ISO 27001, SOC 2, PCI-DSS, HIPAA, NIST 800-218 (SSDF), NIST 800-53, OWASP ASVS, GDPR, CCPA, CIS Benchmarks

**Outputs:** Gap analysis, control mapping table, security metrics dashboard (MTTD, MTTR, vulnerability density), NIST SSDF alignment report, compliance log (audit trail)

---

### `devsec-building-security-programs`

**Description:** Organizational security program design using OWASP SAMM and NIST SSDF. Covers maturity assessment, Security Champions programs, security roadmaps, and VDP design.

**SAMM Functions:** Governance, Design, Implementation, Verification, Operations
**Roadmap horizons:** 30 days, 60 days, 90 days, 6 months, 12 months

---

## Installation

### Install the full AI Core (all agents)

```bash
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/security-agent -a gemini
```

### Install a single agent

```bash
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/security-agent/agents/devsec-code-review -a gemini
```

### Install a single skill

```bash
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/security-agent/skills/devsec-reviewing-code-for-security -a gemini
```

## ToDos

- [ ] Add new skill to bilboard documentation in Confluence
- [ ] Include CWE MITTRE
- [X] Make the principal agent behave interactive with the user, so it can ask for what to document and provide questions as instructions to execute
