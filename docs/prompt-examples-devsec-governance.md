# Developer Security Governance — Prompt Examples

Ready-to-use prompts for the **Developer Security Governance** AI Core.
This core includes **6 agents** backed by **7 skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent will automatically load the required skill references, analyze your context, and deliver actionable security guidance.

---

## Agent → Skill Quick Reference

| Agent | Skills Used |
| --- | --- |
| `devsec-code-review` | `devsec-reviewing-code-for-security` |
| `devsec-threat-modeling` | `devsec-conducting-threat-modeling` |
| `devsec-architecture` | `devsec-designing-security-architecture` |
| `devsec-ops-pipeline` | `devsec-hardening-devsecops-pipelines` |
| `devsec-compliance-framework` | `devsec-managing-compliance-frameworks`, `devsec-publishing-compliance-report` |
| `devsec-program` | `devsec-building-security-programs`, `devsec-managing-compliance-frameworks` |

---

## Skill Outputs Quick Reference

Each skill produces one or more named output artifacts. Use the prompts below to request them explicitly.

| Output Type | Produced By | What It Contains |
| --- | --- | --- |
| **Real-Time Report** | `devsec-code-review` | Instant prioritized findings with OWASP/ASVS/CWE mapping and in-language code fixes |
| **Vulnerability Map** | `devsec-threat-modeling` | Attack surface inventory, STRIDE + CWE threat table, deprecated/CVE library risks, risk register |
| **Remediation Guide** | `devsec-code-review` | Step-by-step fix instructions: scope → fix → test → prevent → verify |
| **Compliance Log** | `devsec-compliance-framework` | Audit-ready ledger of control activities, finding SLAs, KPIs, and evidence index |
| **Confluence Page** | `devsec-compliance-framework` | Security artifact published to Confluence with proper metadata, CWE/OWASP refs preserved |

---

## `devsec-code-review` — Secure Code Review

> **Persona:** Developer reviewing or writing code

### Real-Time Report

```text
Review this Express.js authentication middleware for security vulnerabilities. We handle PII, so target ASVS Level 2.
Produce a Real-Time Report: prioritized findings (Critical → Low), each mapped to an OWASP Top 10 (2025) category, a CWE ID at Base or Variant level, and an ASVS 5.0 requirement, with before/after code fixes in Express.js.
```

```text
Review our Go microservice for the OWASP Top 10 (2025). Focus on input validation, error handling, and cryptography domains.
Output a Real-Time Report with the full OWASP Top 10 coverage table and CWE IDs for each finding.
```

```text
Is this SQL query safe from injection? Here's the code. Check if our Django views are also vulnerable to XSS or CSRF.
```

```text
Check my file upload handler for path traversal and dangerous file type issues. Show exact line numbers, the CWE for each finding, and the fixes.
```

```text
What ASVS Level 2 requirements does this login flow not meet? Run a gap analysis against our current authentication implementation.
```

```text
Review this React component for XSS vulnerabilities. Map findings to CWE-79 or related CWEs and show me before/after code examples of the fix.
```

```text
Scan our backend for deprecated libraries, old or outdated code, and any EOL components. Produce a Real-Time Report with a plan to refactor or replace these legacy parts.
```

### Remediation Guide

```text
Generate a Remediation Guide for the SQL injection finding in our order service. Walk me through: scope the affected queries, show the parameterized query fix in Python/SQLAlchemy, provide a security test that confirms the injection is closed, and list the SAST rule to add so this can't recur.
```

```text
We found a missing authorization check on our /api/users/{id} endpoint (OWASP A01, CWE-862). Produce a step-by-step Remediation Guide with the fix in FastAPI, a test for the IDOR attack vector, and a prevention checklist.
```

---

## `devsec-threat-modeling` — Threat Modeling & STRIDE

> **Persona:** Architect or designer at the design phase

### Threat Model Document

```text
Threat model this checkout flow — here's the architecture diagram. Users authenticate via Cognito and data is stored in PostgreSQL. For each threat, assign a CWE ID at Base or Variant level.
```

```text
Run a STRIDE analysis on our new user authentication system. Users upload documents processed by a Lambda and stored in S3. Map each threat to a CWE ID.
```

```text
What could go wrong with this design? We're building a multi-tenant SaaS where tenants share a database but have row-level security.
```

```text
What could go wrong with this data pipeline that handles PII? Identify trust boundaries and derive security requirements.
```

```text
Security review of my architecture: a Kafka-based event streaming platform with producers in Python and consumers in Go.
```

### Vulnerability Map

```text
Produce a Vulnerability Map for our public-facing REST API. List every external and internal entry point with protocol and trust level, draw the trust boundary map, run STRIDE per component with CWE IDs, and include a table of any deprecated or CVE-affected dependencies with recommended actions.
```

```text
What are the attack surfaces for this architecture? Produce a Vulnerability Map covering the full attack surface inventory, STRIDE threat table with CWE IDs, and a prioritized risk register with owner and remediation timelines.
```

---

## `devsec-architecture` — API, Cloud-Native & AI/LLM Security

> **Persona:** Architect designing APIs, cloud-native, or AI systems

### API & Cloud Security

```text
Design a zero-trust architecture for our internal microservices mesh running on Kubernetes. We need mTLS, RBAC, and network policies.
```

```text
Set up OAuth 2.0 with PKCE for our mobile app's API. Show me the recommended flow and configuration.
```

```text
Design rate limiting and BOLA prevention for our REST API. Walk me through OWASP API Top 10 gaps and fixes.
```

```text
We're experiencing broken object level authorization (BOLA) in our multi-tenant API. Audit our authorization layer and provide fixes.
```

### AI / LLM Security

```text
How do I prevent prompt injection in our customer-facing LLM chatbot? It uses RAG with a vector database.
```

```text
What are the OWASP LLM Top 10 risks for our RAG app and how do we mitigate them? Assess our AI supply chain security.
```

```text
Review our agentic AI system that uses LangChain to call external APIs. What risks exist regarding data leakage between tenants?
```

```text
How do I secure our RAG pipeline? Users query Pinecone and we feed results to GPT-4. Provide concrete mitigations.
```

---

## `devsec-ops-pipeline` — DevSecOps & CI/CD Hardening

> **Persona:** DevOps or platform engineer

```text
Add SAST and dependency scanning to our GitHub Actions pipeline — here's the Node.js YAML config with failure thresholds.
```

```text
How do I detect hardcoded secrets in commits before they reach the repo? Add secret scanning to pre-commit hooks and CI.
```

```text
Set up container image scanning in our GitLab CI pipeline. We deploy via Helm to Kubernetes and need automated DAST.
```

```text
Generate an SBOM for our Node.js application and integrate it into our release process following the SLSA framework.
```

```text
What security gates should we add to our deployment pipeline for SOC 2? Design thresholds for Critical/High findings.
```

```text
Implement supply chain security for our project including dependency pinning, provenance, artifact signing, and automated scanning for deprecated or EOL components.
```

```text
Help me set up a security gate in GitHub Actions that identifies and blocks any new PRs that introduce libraries or frameworks that are officially deprecated or reaching End-of-Life (EOL).
```

---

## `devsec-compliance-framework` — Compliance, Metrics & Confluence Publishing

> **Persona:** Compliance/GRC professional, pre-audit

### Gap Analysis & Control Mapping

```text
Map our existing security controls to SOC 2 Type II and ISO 27001:2022 and identify gaps.
```

```text
What controls do we need for HIPAA compliance on our patient portal? Surface cross-framework overlaps with ASVS.
```

```text
Generate a compliance gap analysis against ISO 27001 for our startup. Map our current practices to NIST SSDF pillars.
```

```text
What security KPIs should we track for our quarterly board report? Include MTTD, MTTR, and vuln density.
```

```text
How do we satisfy GDPR Article 32 for our EU user data storage? List technical measures required for health information.
```

### Compliance Log

```text
Produce a Compliance Log for our Q1 audit. Record every security control activity this quarter (SAST scans, pen test, training, access review) with evidence links and map each activity to ISO 27001:2022, SOC 2, NIST SSDF, and OWASP ASVS in a single table. Include the vulnerability finding log with SLA compliance rates and an evidence index.
```

```text
Create a Compliance Log that tracks all findings from our last pen test and SAST scan cycle. For each finding include severity, OWASP category, CWE ID, SLA target, actual remediation date, and evidence link. Calculate our MTTD and MTTR against the targets in our compliance framework.
```

### Publish to Confluence

```text
Publish this compliance gap analysis to our Confluence security space. The parent page is "Security & Compliance" and the title should be "Q1 2025 ISO 27001 Gap Analysis".
```

```text
Create a Confluence page from the threat model we just produced. Target the "Engineering" space under the "Architecture Decisions" page. Create it as a draft so I can review before publishing.
```

```text
Update the existing "Security Metrics Dashboard" page in our Confluence compliance space with the latest MTTD and MTTR data from this quarter's scan results.
```

```text
Produce a compliance gap analysis against SOC 2 Type II, then publish it as a Confluence page in our "Audit Readiness" space so the team can review it before the auditor call.
```

---

## `devsec-program` — Security Program & Champions

> **Persona:** AppSec lead or CISO building a program

```text
We're a 200-person startup with no formal security program — where do we start? Propose a roadmap and prioritization strategy.
```

```text
Run an OWASP SAMM maturity assessment for our engineering org. Give us a scorecard across Governance, Design, and Implementation.
```

```text
How do we launch a Security Champions program across 10 engineering teams? Design the cohort selection and training plan.
```

```text
Build a 12-month security roadmap based on these current gaps. Include quarterly milestones leading to SOC 2 readiness.
```

```text
How should we prioritize security investments while preparing for SOC 2? Focus on risk reduction and engineering velocity.
```

```text
Design a Vulnerability Disclosure Program (VDP) policy including scope, SLAs, and triage workflow.
```

---

## Universal Prompts

General-purpose prompts that work across multiple security tasks:

```text
Act as a [technical writer / security architect] and help me [task].
Here's the context: [paste code / architecture / requirements]
```

```text
Review [this document / code / architecture] and give me:
1. Issues or gaps found
2. Recommended fixes
3. Priority order
```

```text
I'm a [developer / compliance officer / AppSec lead / writer] working on [project].
Help me [specific goal] — I'll share the relevant files now.
```

```text
Use industry best practices to produce [deliverable] for [audience].
Keep it practical — give me something I can use today.
```

**Cross-agent chaining example:**

```text
Threat model this API design, then review the existing code for security issues that match those threats, and finally document the findings as an architecture decision record.
```

*— This triggers threat modeling + secure code review + architecture documentation in sequence.*

---

## Full Suite — All Agents & Skills in One Prompt

The following prompt is designed to exercise **every agent and skill** in the Developer Security Governance AI Core in a single call. Use it as a starting point and adapt the project name, tech stack, and compliance targets to your own context.

```text
We are preparing for our first SOC 2 Type II audit in 6 months. Our platform
"HealthBridge" is a multi-tenant SaaS for healthcare providers, built with:
- Backend: Python (FastAPI), deployed on AWS EKS via Helm
- Frontend: React SPA, authenticates via OAuth 2.0 + PKCE against Auth0
- Database: PostgreSQL with row-level security for tenant isolation
- AI Feature: an LLM-powered clinical notes summarizer using RAG with
  Pinecone and GPT-4, exposed via a FastAPI endpoint
- CI/CD: GitHub Actions, Docker images pushed to ECR
- Team: 60 engineers across 8 teams, no dedicated AppSec team today

Please perform a complete security assessment covering every domain below:

1. Secure Code Review (devsec-code-review)
   Review our FastAPI authentication middleware and the tenant isolation
   layer for security issues. Target ASVS Level 2 given we handle PHI
   (Protected Health Information).
   → Produce a Real-Time Report: prioritized findings (Critical → Low)
     each mapped to an OWASP Top 10 (2025) category, CWE ID at Base or
     Variant level, and ASVS 5.0 requirement, with before/after code
     fixes in Python/FastAPI. Include the full OWASP Top 10 coverage
     table and automation notes.
   → For every Critical finding, also produce a Remediation Guide with
     step-by-step fix, security test, and prevention mechanism.
   → Generate a secure code review checklist tailored to Python/FastAPI.

2. Threat Modeling (devsec-threat-modeling)
   Threat model the full architecture: React SPA → Auth0 → API Gateway →
   FastAPI services → PostgreSQL + Pinecone → GPT-4 (external). Apply
   STRIDE per component. Identify trust boundaries, assign CWE IDs to
   each threat, prioritize by exploitability × impact, and derive testable
   security requirements.
   → Produce the Threat Model Document using the standard template.
   → Also produce a Vulnerability Map: full attack surface inventory,
     trust boundary diagram, STRIDE + CWE threat table per component,
     deprecated and CVE-affected dependency table (maps to OWASP A06),
     and a prioritized risk register with owner and remediation timelines.

3. Security Architecture Review
   a) API & Cloud Security: Audit our REST API against the OWASP API Top
      10. We suspect BOLA issues in our multi-tenant endpoints. Review
      our OAuth 2.0/PKCE flow and JWT lifecycle. Recommend zero-trust
      patterns for service-to-service communication within the EKS mesh.
   b) AI/LLM Security: Review our RAG-based clinical notes summarizer
      against the OWASP LLM Top 10 (2025). Assess prompt injection risks,
      model supply chain security, and data leakage between tenants in
      the RAG pipeline. Provide concrete mitigations.

4. DevSecOps Pipeline Hardening (devsec-ops-pipeline)
   Secure our GitHub Actions CI/CD pipeline end-to-end:
   - Add SAST for Python (with YAML config and failure thresholds)
   - Add SCA for dependency vulnerability scanning
   - Add automated scanning for deprecated libraries and EOL components
   - Add secret scanning to pre-commit hooks and CI
   - Add container image scanning for our Docker images
   - Generate an SBOM and integrate artifact signing (SLSA framework)
   - Design security gates with Critical/High thresholds and an
     exception workflow
   Provide ready-to-paste GitHub Actions YAML for each step.

5. Compliance Framework Mapping (devsec-compliance-framework)
   - Map our controls to ISO 27001:2022, NIST SSDF (PO/PS/PW/RV), and
     HIPAA Security Rule requirements. Surface cross-framework overlaps.
   - Run an ASVS Level 2 gap analysis for our authentication and data
     protection controls.
   - Design a security metrics dashboard: MTTD, MTTR, vulnerability
     density, OWASP Top 10 detection rate, training completion. Include
     quarterly targets.
   - List GDPR Article 32 technical measures required for EU patient data.
   → Produce a Compliance Log: record every security control activity
     (scans, reviews, training) with evidence links mapped to ISO 27001,
     SOC 2, NIST SSDF, ASVS, and GDPR Article in a single audit-ready
     table. Include the vulnerability SLA log, KPI dashboard, and an
     evidence index organized for SOC 2 Type II auditors.
   → Publish the Compliance Log to our Confluence "Audit Readiness" space
     under the parent page "SOC 2 Preparation".

6. Security Program Design (devsec-program)
   - Run an OWASP SAMM assessment based on the context provided. Produce
     a scorecard across all 5 business functions.
   - Design a Security Champions program: first cohort selection criteria,
     launch plan, training progression, and recognition model.
   - Build a 12-month security roadmap with 30/60/90-day milestones and
     quarterly goals leading to SOC 2 readiness.
   - Draft a Vulnerability Disclosure Program (VDP) policy with scope,
     SLAs, and triage workflow.
   - Write an executive summary for our CTO justifying the security
     program investment, framed around risk reduction, audit readiness,
     and engineering velocity.

Deliverables should be saved as separate markdown files. Use concrete
recommendations for our specific stack (Python/FastAPI, Auth0, AWS EKS,
GitHub Actions) — not generic advice.
```

### What This Prompt Activates

| Step | Agent | Skill(s) | Output Type |
| --- | --- | --- | --- |
| 1. Code Review | `devsec-code-review` | `devsec-reviewing-code-for-security` (OWASP Top 10, ASVS, CWE Top 25, secure coding practices) | **Real-Time Report** + **Remediation Guides** |
| 2. Threat Model | `devsec-threat-modeling` | `devsec-conducting-threat-modeling` (STRIDE, CWE mapping, threat model template) | Threat Model Doc + **Vulnerability Map** |
| 3a. API & Cloud | `devsec-architecture` | `devsec-designing-security-architecture` (API/cloud security patterns) | Architecture review |
| 3b. AI/LLM | `devsec-architecture` | `devsec-designing-security-architecture` (LLM/AI security, OWASP LLM Top 10) | Architecture review |
| 4. Pipeline | `devsec-ops-pipeline` | `devsec-hardening-devsecops-pipelines` (CI/CD-SEC, SAST/DAST, SBOM) | Pipeline config (YAML) |
| 5. Compliance | `devsec-compliance-framework` | `devsec-managing-compliance-frameworks` + `devsec-publishing-compliance-report` | **Compliance Log** + **Confluence Page** |
| 6. Program | `devsec-program` | `devsec-building-security-programs` + `devsec-managing-compliance-frameworks` (SAMM, Champions, roadmap) | Roadmap + SAMM scorecard |

> **Tip**: This is a very large prompt that spans all 6 agents. Depending on your AI assistant's capabilities, it may process domains sequentially or ask you to confirm before proceeding. You can also extract individual numbered sections as standalone prompts — each one maps to a specific security domain.

### Short Version — Same Coverage, Fewer Words

If you prefer a concise prompt that still triggers every agent and skill:

```text
Our "HealthBridge" SaaS (Python/FastAPI, React, Auth0, PostgreSQL, AWS EKS,
GitHub Actions) handles PHI and has a RAG+GPT-4 clinical summarizer. SOC 2
audit in 6 months, no AppSec team. Perform: (1) secure code review of our
auth middleware and tenant isolation, ASVS L2, CWE-mapped findings, with
fixes and a checklist; (2) STRIDE threat model of the full architecture with
CWE IDs per threat and security requirements; (3) OWASP API Top 10 audit,
OAuth/JWT review, zero-trust recommendations, plus OWASP LLM Top 10 review
of the RAG pipeline; (4) GitHub Actions pipeline hardening — SAST, SCA,
secrets, container scanning, SBOM, EOL checks, and security gates with YAML
configs; (5) compliance mapping to ISO 27001, NIST SSDF, HIPAA, ASVS L2 gap
analysis, metrics dashboard, and SOC 2 evidence checklist — then publish the
compliance log to our Confluence "Audit Readiness" space; (6) SAMM assessment,
Security Champions launch plan, 12-month roadmap, VDP policy, and executive
summary for the CTO.
```

### For Non-Technical Users

A plain-language prompt that still triggers every agent and skill:

```text
We're a healthcare company launching a patient portal that uses AI to
summarize clinical notes. We have an audit coming up in 6 months and no
dedicated security team. Please: (1) check our code for security problems
and give us a checklist to follow; (2) tell us what could go wrong with
our system design and what we should require to prevent it; (3) review
whether our login system and AI features are safe, especially against the
latest known attack types; (4) set up automatic security scanning in our
build process and give us the configuration files ready to use; (5) map
what we're doing today against the standards our auditors will check
(ISO 27001, HIPAA, SOC 2) and tell us what's missing, with a dashboard
to track progress — then post the results to our team wiki; (6) design a
security program for us: assess where we are today, propose a champions
network, build a 12-month plan, draft a policy for handling reported
vulnerabilities, and write a one-pager for our CTO explaining why this
investment matters.
```

---

## Tips for Better Results

1. **Name your stack** — Mention languages, frameworks, cloud provider, and CI platform so the agent gives concrete configs.
2. **State data sensitivity** — "We handle PII / financial data / health records" helps the agent calibrate severity and ASVS levels.
3. **Specify compliance targets** — "We need SOC 2 / PCI-DSS / GDPR" triggers the right reference material.
4. **Describe your current state** — "No scanning today" vs. "Replacing Veracode" leads to very different recommendations.
5. **Ask for deliverables** — "Give me a gap analysis", "Produce a roadmap", "Generate the YAML config" ensures actionable output.
6. **Request Confluence publishing** — Add "publish to our [Space Name] Confluence space" to any compliance or assessment request to get a formatted page created automatically.
