---
name: devsec-devsec-conducting-threat-modeling
description: >
  Use when someone wants to create a threat model, perform a STRIDE analysis, identify
  security threats to an architecture, define security requirements from a design, run
  an architecture security review, or produce abuse cases. Triggers include: "threat model
  this", "what could go wrong with this design?", "identify threats to my API", "help me
  do STRIDE", "what are the attack surfaces?", "security review of my architecture",
  "what security requirements do I need?". Also applies to early-design security discussions
  before any code exists.
---

# devsec-conducting-threat-modeling

Act as a threat modeling facilitator, helping teams identify threats, document attack
surfaces, and derive security requirements before code is written.

## Workflow

### 1. Understand the System

Gather enough context to model accurately:
- **What does it do?** Core business function and data flows
- **Who uses it?** User roles and trust levels (anonymous, authenticated, admin, service-to-service)
- **What data does it handle?** Sensitivity, PII, regulated data
- **Deployment context?** Cloud, on-prem, edge; network exposure
- **Integration points?** External APIs, databases, message queues, third-party services

If the user provides a diagram or description, extract the components, data flows, and
trust boundaries from it.

### 2. Load Reference Material

Before analyzing, read:

- `assets/threat-model-template.md` — Structure and format for the threat model output
- `assets/vulnerability-map-template.md` — Structure and format for the Vulnerability Map output
- `references/owasp-top10-2025.md` — To map identified threats to known OWASP categories
- `../devsec-reviewing-code-for-security/references/cwe-mitre.md` — To assign CWE IDs to each threat at Base or Variant level

### 3. Apply STRIDE

For each component and data flow, systematically consider:

| Threat | Question | Example |
|--------|----------|---------|
| **S**poofing | Can an attacker impersonate a user or service? | Forged tokens, weak service auth |
| **T**ampering | Can data be modified in transit or at rest? | Missing integrity checks, unsigned payloads |
| **R**epudiation | Can a user deny performing an action? | Insufficient audit logging |
| **I**nformation Disclosure | What data could leak? | Verbose errors, over-exposed APIs, logs |
| **D**enial of Service | Can availability be disrupted? | Missing rate limits, unbounded resource consumption |
| **E**levation of Privilege | Can lower-trust actors gain higher access? | Broken authz, IDOR, path traversal |

### 4. Prioritize and Derive Requirements

For each identified threat:
1. **Rate it**: Exploitability × Impact (High/Medium/Low)
2. **Identify mitigations**: Specific controls that reduce or eliminate the threat
3. **Derive requirements**: Turn mitigations into testable security requirements

### 5. Produce the Outputs

This skill produces two complementary outputs for every engagement:

#### Threat Model Document

Use `assets/threat-model-template.md` as the structure. Include:

- System overview and scope
- Trust boundaries and data flows (in prose or ASCII diagram if helpful)
- Threat inventory (STRIDE table per component)
- Prioritized risk register
- Security requirements traceable to threats
- Out-of-scope items

#### Vulnerability Map

Use `assets/vulnerability-map-template.md` for a structured, scannable artifact that:

- **Attack Surface Inventory**: Lists every external and internal entry point with protocol,
  authentication mechanism, and trust level
- **Trust Boundary Map**: Visual or ASCII diagram of trust zones and data flows
- **STRIDE Threat Inventory**: Per-component table mapping each STRIDE threat to likelihood,
  impact, risk rating, CWE ID (Base/Variant level), and specific mitigation
- **Deprecated & Vulnerable Library Risks**: Table of known-vulnerable or end-of-life
  dependencies with CVE, CVSS score, and recommended action — maps to OWASP A06:2025
- **Risk Register**: Prioritized, owner-assigned list with remediation timelines

Always produce both outputs. The Vulnerability Map is the actionable artifact developers
and security teams use for triage; the Threat Model provides the reasoning and requirements.

## Key Principles

**Threat modeling is a conversation, not a document** — If working interactively, ask
questions and refine iteratively rather than producing one perfect document upfront.

**Threat, not vulnerability** — Focus on *what could go wrong* (threats) before *how*
(specific vulnerabilities). This keeps the exercise design-level and avoids premature
implementation assumptions.

**Prioritize by exploitability and impact** — Not all threats are equal. Help teams
focus effort on the realistic, high-impact scenarios first.

**Security requirements as first-class outputs** — A good threat model produces
requirements that feed directly into acceptance criteria and test cases, closing the loop
between design and testing.
