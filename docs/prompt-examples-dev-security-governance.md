# Developer Security Governance — Prompt Examples

Ready-to-use prompts for the **Developer Security Governance** AI Core.
This core includes **6 agents** backed by **6 skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent will automatically load the required skill references, analyze your context, and deliver actionable security guidance.

---

## Agent → Skill Quick Reference

| Agent | Skills Used |
|---|---|
| `devsec-code-review` | `managing-compliance-frameworks` |
| `devsec-threat-modeling` | `conducting-threat-modeling` |
| `devsec-architecture` | `designing-security-architecture` |
| `devsec-ops-pipeline` | `hardening-devsecops-pipelines` |
| `devsec-compliance-framework` | `managing-compliance-frameworks` |
| `devsec-program` | `building-security-programs`, `managing-compliance-frameworks` |

---

## `devsec-code-review` — Secure Code Review

```text
Review this Express.js authentication middleware for security issues.
We handle PII, so target ASVS Level 2.
```

```text
Check if our Django views are vulnerable to SQL injection, XSS, or CSRF.
Show exact line numbers and fixes.
```

```text
Give me a secure code review checklist tailored for our Java Spring Boot
REST API that processes payment data.
```

```text
How do I prevent insecure deserialization in our Python Flask service?
Show me before/after code examples.
```

```text
What ASVS verification level should we target for our healthcare patient
portal? Run a gap analysis against our current authentication implementation.
```

```text
Review our Go microservice for the OWASP Top 10 (2025). Focus on input
validation, error handling, and cryptography domains.
```

---

## `devsec-threat-modeling` — Threat Modeling & STRIDE

```text
Threat model this: a React SPA → API Gateway → 3 microservices → PostgreSQL,
deployed on AWS ECS with an ALB. Users authenticate via Cognito.
```

```text
Help me do STRIDE analysis on our new file upload feature. Users upload
documents that get processed by a Lambda function and stored in S3.
```

```text
What could go wrong with this design? We're building a multi-tenant SaaS
where tenants share a database but have row-level security.
```

```text
Identify the attack surfaces for our mobile banking API. Users authenticate
via biometrics + OTP and can transfer funds via the API.
```

```text
What security requirements do I need for a real-time notification system
that sends SMS and push notifications with user PII?
```

```text
Security review of my architecture: a Kafka-based event streaming platform
with producers in Python, consumers in Go, deployed on Kubernetes. Identify
trust boundaries and derive requirements.
```

---

## `devsec-architecture` — API, Cloud-Native & AI/LLM Security

### API & Cloud Security

```text
Secure my REST API: we use Express.js, JWT for auth, and deploy behind Nginx.
Walk me through OWASP API Top 10 gaps and fixes.
```

```text
Set up OAuth 2.0 with PKCE for our React SPA calling a .NET API. Show me
the recommended flow and configuration.
```

```text
Design a zero-trust architecture for our microservices mesh running on
Kubernetes with Istio. We need mTLS, RBAC, and network policies.
```

```text
We're experiencing broken object level authorization (BOLA) in our multi-tenant
API. Audit our authorization layer and provide fixes.
```

### AI / LLM Security

```text
Review our LLM-powered chatbot for prompt injection risks. It uses RAG with
a vector database and can call internal tools via function calling.
```

```text
Our agentic AI system uses LangChain to call external APIs and execute code.
Review it against the OWASP LLM Top 10 and suggest mitigations.
```

```text
How do I secure our RAG pipeline? Users query a knowledge base, we retrieve
documents from Pinecone, and feed them as context to GPT-4.
```

```text
Assess our AI application's supply chain security: we use fine-tuned models,
third-party plugins, and an embedding service. What risks exist?
```

---

## `devsec-ops-pipeline` — DevSecOps & CI/CD Hardening

```text
Set up SAST in our GitHub Actions pipeline for a Node.js TypeScript project.
Include the YAML config with failure thresholds.
```

```text
Secure my CI/CD pipeline end-to-end. We use GitLab CI, Docker, Terraform,
and deploy to AWS EKS. Give me a CI/CD-SEC gap analysis.
```

```text
Generate an SBOM for our Python application and integrate it into our release
process. We use GitHub Actions.
```

```text
Add secret scanning to pre-commit hooks and CI. We use a monorepo with Go,
Python, and Terraform code.
```

```text
Design security gates for our pipeline: what thresholds for Critical/High
findings, and how should the exception workflow work?
```

```text
Implement supply chain security for our project following the SLSA framework.
Include dependency pinning, provenance, and artifact signing.
```

```text
Integrate OWASP ZAP into our staging environment. We deploy via Helm to
Kubernetes and need automated DAST on every nightly build.
```

---

## `devsec-compliance-framework` — Compliance & Metrics

```text
Map our existing security controls to ISO 27001:2022 and identify gaps.
We need this for our upcoming SOC 2 Type II audit.
```

```text
Run an NIST SSDF alignment assessment for our development organization.
Map our current practices to PO/PS/PW/RV pillars.
```

```text
Design a security metrics dashboard with KPIs: MTTD, MTTR, vulnerability
density, and OWASP Top 10 detection rate. Include targets.
```

```text
What controls do I need for GDPR Article 32 compliance? We process EU
customer data including health information.
```

```text
Create a cross-framework compliance mapping between ISO 27001, NIST 800-53,
PCI-DSS, and OWASP ASVS for our fintech platform.
```

```text
Prepare audit evidence for our SOC 2 Type II: list each control, the
evidence artifact, and where to find it in our repo and infrastructure.
```

---

## `devsec-program` — Security Program & Champions

```text
Assess our security maturity using OWASP SAMM. We're a 200-person engineering
org with no formal AppSec team. Give us a scorecard and roadmap.
```

```text
Start a Security Champions program. We have 15 dev teams, no security team,
and two security-aware senior engineers. Design the launch plan.
```

```text
Build our appsec program from scratch. We're a Series B startup with 40
engineers, handling financial data, and need SOC 2 in 12 months.
```

```text
Make the case to leadership: write an executive summary that justifies
investing in a security program. Focus on risk reduction and engineering velocity.
```

```text
Design a Vulnerability Disclosure Program (VDP) for our public-facing SaaS
platform. Include the policy template and triage workflow.
```

```text
We just finished a SAMM assessment at L1. Create a 12-month roadmap to
reach L2 across Governance, Design, and Implementation functions. Include
quarterly milestones and success metrics.
```

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
