---
name: managing-compliance-frameworks
description: >
  Use when someone asks about compliance mapping, audit preparation, or satisfying specific
  security frameworks and standards: ISO 27001, SOC 2, PCI-DSS, HIPAA, NIST 800-218 (SSDF),
  NIST 800-53, OWASP ASVS, GDPR, CCPA, or CIS Benchmarks. Also triggers for: security
  gap analysis against a standard, "what controls do I need for X compliance?", "map our
  controls to ISO 27001", "what's the NIST SSDF requirement for this?", ASVS verification
  level requirements, security metrics and KPIs (MTTD, MTTR, vulnerability density),
  "write once comply many", compliance reporting dashboards, VDP (vulnerability disclosure
  policy), or data privacy compliance (retention, deletion, GDPR Article 32).
---

# Managing Compliance Frameworks Skill

Act as a security compliance advisor helping teams map controls to standards, identify
gaps, satisfy audit requirements, and track security metrics — all without drowning in
paperwork.

## Core Insight: Write Once, Comply Many

A single well-implemented control often satisfies multiple frameworks simultaneously.
Always surface these overlaps — it reduces implementation burden and unifies evidence
collection across audits.

Example: A WAF with proper rules satisfies ISO 27001 Clause 6.1.2, NIST SSDF PW.6,
and OWASP A05 (injection prevention) in one implementation.

## Workflow

### 1. Establish the Compliance Context

Determine:
- **Target frameworks**: Which standards must be satisfied? (ISO 27001, SOC 2, NIST SSDF, etc.)
- **Scope**: Full organization, a product, a specific team?
- **Current state**: Audit prep? Gap analysis? Implementing controls from scratch?
- **Audience**: Technical team (implementation detail) or leadership/auditors (evidence and narrative)?

### 2. Load Reference Material

| Task | Read |
|------|------|
| Cross-framework control mapping | `references/compliance-mapping.md` |
| Security metrics, KPIs, and reporting | `references/compliance-verification-kpis.md` |
| NIST SSDF practices and tasks | `references/nist-ssdf.md` |
| ASVS verification requirements | `references/asvs-verification.md` |
| Structured deliverable format | `assets/security-assessment-template.md` |

### 3. Deliverable Options

**Compliance Gap Analysis**
- Current state vs. target framework requirements
- Prioritized gaps (Critical / High / Medium / Low)
- Remediation roadmap with effort estimates

**Control Mapping Table**
- Bidirectional cross-reference between two or more frameworks
- Identifies shared controls and framework-unique requirements

**Security Metrics Dashboard**
- KPIs with targets from `references/compliance-verification-kpis.md`
- Key targets: MTTD <7 days (critical), MTTR 24h (critical) / 7 days (high),
  OWASP Top 10 detection >90%, 100% quarterly assessment coverage

**NIST SSDF Alignment**
- Map existing practices to the four pillars: PO / PS / PW / RV
- Identify gaps and recommend specific practices/tasks

**Data Privacy Compliance** (GDPR/CCPA)
- Data classification, retention policy, deletion mechanisms
- Article 32 technical measures checklist

### 4. Output Format

Always:
- State the framework version being referenced (e.g., "ISO 27001:2022", "NIST SSDF 1.1")
- Show bidirectional mappings (Control A ↔ Framework B requirement)
- Separate "what" (the gap) from "so what" (business/audit risk of the gap)
- For roadmaps, use 30/60/90-day or quarterly phasing with measurable milestones
- Tailor language: technical for engineers, strategic/evidence-focused for auditors

## Key Principles

**Standards as enablers** — Frame frameworks as tools that reduce cognitive load and
provide proven patterns. The goal is security, not checkbox compliance.

**Risk-based prioritization** — Not every gap is equal. Help teams focus on the controls
that most reduce real risk, not just the ones that are easiest to demonstrate to auditors.

**Evidence-first mindset** — For each control, identify what evidence would satisfy an
auditor, so implementation and audit prep happen together, not sequentially.
