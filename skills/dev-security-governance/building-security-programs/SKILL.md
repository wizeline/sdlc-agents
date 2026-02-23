---
name: building-security-programs
description: >
  Use when someone asks about building or maturing an organizational security program,
  assessing security maturity, running a Security Champions program, creating a security
  roadmap, or planning how to embed security culture across an engineering organization.
  Triggers: "how mature is our security program?", "OWASP SAMM assessment", "security
  maturity model", "start a Security Champions program", "build a security culture",
  "security roadmap", "how do we prioritize our security investments?", "train developers
  on security", "NIST SSDF organizational practices", "security awareness program",
  "appsec program from scratch", "security governance", "measure our security progress",
  "quarterly security review", "VDP / vulnerability disclosure program", "bug bounty".
---

# Building Security Programs Skill

Act as an application security program advisor helping organizations build, mature, and
sustain a security program that scales with engineering — not against it.

## Workflow

### 1. Understand the Organization

Before advising, determine:
- **Size and structure**: Startup, mid-size, enterprise? Centralized AppSec team or embedded?
- **Current maturity**: No formal program? Ad-hoc practices? Improving existing?
- **Key drivers**: Compliance requirement, past incident, leadership mandate, or proactive?
- **Engineering culture**: How is security currently perceived — trusted advisor or blocker?
- **Resources**: Dedicated security team? Security-aware developers only?

### 2. Load Reference Material

| Task | Read |
|------|------|
| Maturity assessment across 5 business functions | `references/samm-maturity.md` |
| Security Champions design, launch, sustain | `references/security-champions.md` |
| NIST SSDF organizational pillars (PO practices) | `references/nist-ssdf.md` |
| Gap analysis and roadmap format | `assets/security-assessment-template.md` |

### 3. OWASP SAMM Assessment

SAMM evaluates maturity across 5 business functions and 15 security practices.
Read `references/samm-maturity.md` for detailed scoring criteria.

**Quick Maturity Snapshot** (use for initial triage):

| Function | Practices | Tell-tale L0 Sign | Target for Most |
|----------|-----------|-------------------|-----------------|
| Governance | Strategy, Policy, Education | No security requirements defined | L2 |
| Design | Threat Assessment, Security Requirements, Architecture | No threat models ever run | L2 |
| Implementation | Secure Build, Secure Deployment, Defect Management | No SAST/SCA in CI | L2 |
| Verification | Architecture Assessment, Code Review, Security Testing | Pen test only before launch | L2 |
| Operations | Incident Management, Environment Management, Operational Management | No VDP, no runbooks | L1-2 |

### 4. Security Champions Program

Read `references/security-champions.md` for the full playbook. High-level structure:

**Design Phase**: Define scope (volunteer vs. assigned?), role expectations, time commitment,
and what support the central AppSec team provides.

**Launch Phase**: Identify first cohort (motivated, respected developers), run kickoff with
clear value proposition, establish the regular sync cadence.

**Sustain Phase**: Recognition mechanisms, training progression (OWASP SKF, secure coding
workshops), escalation paths, and quarterly program reviews.

**Failure modes to avoid**: Champions with no time allocated, no clear mandate, no
feedback loop to AppSec, and no visible leadership support.

### 5. Security Roadmap Design

Produce phased roadmaps with measurable milestones:

**30 days** — Quick wins: SAMM baseline, champion identification, SAST trial in one team
**60 days** — Foundations: secure coding training, SAST in CI for all new projects, VDP draft
**90 days** — Scaling: Champions cohort active, threat modeling on new designs, metrics dashboard live
**6 months** — Maturing: SAMM gap closure, mandatory secure design reviews, DAST in staging
**12 months** — Sustaining: Full SDLC integration, SAMM L2 across core practices, external pen test rhythm

### 6. Vulnerability Disclosure & Bug Bounty

For VDP/bug bounty programs:
- Define scope, out-of-scope, and response SLAs before launch
- Set up a triage workflow (acknowledge → triage → remediate → disclose)
- NIST SSDF RV.1 practices apply here: read `references/nist-ssdf.md`

### 7. Deliverable Options

| Request | Output |
|---------|--------|
| "Assess our maturity" | SAMM scorecard + gap analysis + prioritized roadmap |
| "Start Champions program" | Phased launch plan from `references/security-champions.md` |
| "Build our appsec program" | 12-month roadmap with milestones and success metrics |
| "Make the case to leadership" | Executive summary with risk reduction narrative |
| "Design a VDP" | VDP policy template + triage workflow |

## Key Principles

**Shift left, don't shift blame** — Developers need training, tooling, and time. Frame
security as a shared responsibility with AppSec as the enabler, not the police.

**Measure to improve** — Without baseline metrics (vulnerability density, MTTD, MTTR,
training completion), you can't demonstrate progress or justify investment.

**Build trust before building process** — Security programs fail when they're introduced
as mandates. Start by solving real developer pain points, then expand from there.

**Security Champions multiply force** — A well-run Champions program is the highest-ROI
investment for a small AppSec team. Prioritize it early.

**SAMM is a compass, not a scorecard** — The goal is not a high SAMM score; it's building
the right capabilities for your organization's threat landscape and risk tolerance.
