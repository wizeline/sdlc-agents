# Developer Security Governance — Prompt Examples

Ready-to-use prompts for the **Developer Security Governance** AI Core.
This core includes **6 agents** backed by **6 skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent will automatically load the required skill references, analyze your context, and deliver actionable security guidance.

---

## Agent → Skill Quick Reference

| Agent | Skills Used |
| --- | --- |
| `devsec-code-review` | `managing-compliance-frameworks` |
| `devsec-threat-modeling` | `conducting-threat-modeling` |
| `devsec-architecture` | `designing-security-architecture` |
| `devsec-ops-pipeline` | `hardening-devsecops-pipelines` |
| `devsec-compliance-framework` | `managing-compliance-frameworks` |
| `devsec-program` | `building-security-programs`, `managing-compliance-frameworks` |

---

## `devsec-code-review` — Secure Code Review

> **Persona:** Developer reviewing or writing code

```text
Review this Express.js authentication middleware for security vulnerabilities. We handle PII, so target ASVS Level 2.
```

```text
Is this SQL query safe from injection? Here's the code. Check if our Django views are also vulnerable to XSS or CSRF.
```

```text
Check my file upload handler for path traversal and dangerous file type issues. Show exact line numbers and fixes.
```

```text
What ASVS Level 2 requirements does this login flow not meet? Run a gap analysis against our current authentication implementation.
```

```text
Review this React component for XSS vulnerabilities. Show me before/after code examples of the fix.
```

```text
Review our Go microservice for the OWASP Top 10 (2025). Focus on input validation, error handling, and cryptography domains.
```

---

## `devsec-threat-modeling` — Threat Modeling & STRIDE

> **Persona:** Architect or designer at the design phase

```text
Threat model this checkout flow — here's the architecture diagram. Users authenticate via Cognito and data is stored in PostgreSQL.
```

```text
Run a STRIDE analysis on our new user authentication system. Users upload documents processed by a Lambda and stored in S3.
```

```text
What could go wrong with this design? We're building a multi-tenant SaaS where tenants share a database but have row-level security.
```

```text
What are the attack surfaces for this public-facing REST API? Identify security threats to this design before we start coding.
```

```text
What could go wrong with this data pipeline that handles PII? Identify trust boundaries and derive security requirements.
```

```text
Security review of my architecture: a Kafka-based event streaming platform with producers in Python and consumers in Go.
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
Implement supply chain security for our project including dependency pinning, provenance, and artifact signing.
```

---

## `devsec-compliance-framework` — Compliance & Metrics

> **Persona:** Compliance/GRC professional, pre-audit

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

```text
Prepare audit evidence for our SOC 2 Type II: list each control, the evidence artifact, and its repository location.
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

*— This triggers `conducting-threat-modeling` + `reviewing-code-for-security` + `authoring-architecture-docs` in sequence.*

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
   (Protected Health Information). Produce prioritized findings with
   OWASP category, CWE ID, and before/after code fixes. Also generate a
   secure code review checklist tailored to our Python/FastAPI stack.

2. Threat Modeling (devsec-threat-modeling)
   Threat model the full architecture: React SPA → Auth0 → API Gateway →
   FastAPI services → PostgreSQL + Pinecone → GPT-4 (external). Apply
   STRIDE per component. Identify trust boundaries, prioritize threats by
   exploitability × impact, and derive testable security requirements.
   Produce the threat model document using the standard template.

3. Security Architecture Review (devsec-architecture)
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
   - Prepare an audit evidence checklist for SOC 2 Type II: each control,
     the evidence artifact, and where to find it.

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

| Step | Agent | Skill(s) |
| ---- | ----- | -------- |
| 1. Code Review | `devsec-code-review` | `managing-compliance-frameworks` (OWASP Top 10, ASVS, secure coding practices) |
| 2. Threat Model | `devsec-threat-modeling` | `conducting-threat-modeling` (STRIDE, threat model template) |
| 3a. API & Cloud | `devsec-architecture` | `designing-security-architecture` (API/cloud security patterns) |
| 3b. AI/LLM | `devsec-architecture` | `designing-security-architecture` (LLM/AI security, OWASP LLM Top 10) |
| 4. Pipeline | `devsec-ops-pipeline` | `hardening-devsecops-pipelines` (CI/CD-SEC, SAST/DAST, SBOM) |
| 5. Compliance | `devsec-compliance-framework` | `managing-compliance-frameworks` (compliance mapping, NIST SSDF, ASVS, KPIs) |
| 6. Program | `devsec-program` | `building-security-programs` + `managing-compliance-frameworks` (SAMM, Champions, roadmap) |

> **💡 Tip**: This is a very large prompt that spans all 6 agents. Depending on your AI assistant's capabilities, it may process domains sequentially or ask you to confirm before proceeding. You can also extract individual numbered sections as standalone prompts — each one maps to a specific agent.

### Short Version — Same Coverage, Fewer Words

If you prefer a concise prompt that still triggers every agent and skill:

```text
Our "HealthBridge" SaaS (Python/FastAPI, React, Auth0, PostgreSQL, AWS EKS,
GitHub Actions) handles PHI and has a RAG+GPT-4 clinical summarizer. SOC 2
audit in 6 months, no AppSec team. Perform: (1) secure code review of our
auth middleware and tenant isolation, ASVS L2, with fixes and a checklist;
(2) STRIDE threat model of the full architecture with security requirements;
(3) OWASP API Top 10 audit, OAuth/JWT review, zero-trust recommendations,
plus OWASP LLM Top 10 review of the RAG pipeline; (4) GitHub Actions pipeline
hardening — SAST, SCA, secrets, container scanning, SBOM, security gates with
YAML configs; (5) compliance mapping to ISO 27001, NIST SSDF, HIPAA, ASVS L2
gap analysis, metrics dashboard, and SOC 2 evidence checklist; (6) SAMM
assessment, Security Champions launch plan, 12-month roadmap, VDP policy,
and executive summary for the CTO.
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
to track progress; (6) design a security program for us: assess where we
are today, propose a champions network, build a 12-month plan, draft a
policy for handling reported vulnerabilities, and write a one-pager for
our CTO explaining why this investment matters.
```

---

## Tips for Better Results

1. **Name your stack** — Mention languages, frameworks, cloud provider, and CI platform so the agent gives concrete configs.
2. **State data sensitivity** — "We handle PII / financial data / health records" helps the agent calibrate severity and ASVS levels.
3. **Specify compliance targets** — "We need SOC 2 / PCI-DSS / GDPR" triggers the right reference material.
4. **Describe your current state** — "No scanning today" vs. "Replacing Veracode" leads to very different recommendations.
5. **Ask for deliverables** — "Give me a gap analysis", "Produce a roadmap", "Generate the YAML config" ensures actionable output.
