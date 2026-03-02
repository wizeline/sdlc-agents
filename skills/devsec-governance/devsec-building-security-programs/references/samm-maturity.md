# OWASP SAMM — Software Assurance Maturity Model Reference

## Overview

SAMM provides a measurable, risk-driven framework for analyzing and improving
an organization's software security posture. It is technology-agnostic and
designed for evolutionary adoption — you don't need to be at Level 3 everywhere
to be secure; you need to be at the right level for your risk profile.

## Five Business Functions

### 1. Governance
Focuses on strategy, policy, and training.
- **Strategy & Metrics:** Define security strategy aligned to business risk; track KPIs
- **Policy & Compliance:** Establish security policies; map to regulatory requirements
- **Education & Guidance:** Security training programs; secure coding guidelines

### 2. Design
Ensures security is designed in, not bolted on.
- **Threat Assessment:** Threat modeling; risk analysis per application
- **Security Requirements:** Derive security requirements from threats and compliance needs
- **Security Architecture:** Reference architectures; approved technology choices

### 3. Implementation
Covers building and deploying securely.
- **Secure Build:** CI/CD security; dependency management; build hardening
- **Secure Deployment:** Deployment verification; environment hardening; secrets management
- **Defect Management:** Vulnerability tracking; remediation SLAs; trending

### 4. Verification
Confirms that security controls work as intended.
- **Architecture Assessment:** Periodic architecture reviews against threat models
- **Requirements-Driven Testing:** Test cases derived from security requirements
- **Security Testing:** SAST, DAST, penetration testing; tool integration

### 5. Operations
Ensures security is maintained post-deployment.
- **Incident Management:** Response plans; tabletop exercises; lessons learned
- **Environment Management:** Production hardening; patch management; monitoring
- **Operational Management:** Data protection; legacy system security; asset management

## Maturity Levels

Each security practice within each business function has three maturity levels:

### Level 1: Initial
- Practices are ad-hoc and inconsistently applied
- Individual developers may follow good practices, but it's not standardized
- No formal measurement or tracking
- **Indicators:** Some training exists; SAST runs occasionally; threat modeling happens
  sometimes for high-risk features

### Level 2: Managed
- Practices are standardized and integrated across the organization
- Clear processes, tooling, and expectations exist
- Metrics are tracked and reported to management
- **Indicators:** All teams follow secure coding standards; SAST runs in CI for every build;
  threat models required for all new features; vulnerability SLAs enforced

### Level 3: Optimized
- Practices are continuously improved based on data and feedback
- Automation is maximized; manual intervention is minimized
- Organization actively contributes to industry standards and community
- **Indicators:** Security metrics drive architectural decisions; automated remediation
  for common vulnerability patterns; predictive analytics on security debt

## Conducting a SAMM Assessment

### Self-Assessment Process
1. **Select scope:** Assess the entire organization, a business unit, or a single application
2. **Gather evidence:** Collect artifacts (policies, tool configs, training records, metrics)
3. **Score each practice:** Rate each practice against the three maturity levels
4. **Identify gaps:** Compare current state to target state based on risk profile
5. **Build a roadmap:** Prioritize improvements that deliver the most risk reduction

### Scoring Guidelines
- Score based on *consistent, verified practice* — not aspirational goals
- A practice is at Level N only if it meets ALL criteria for that level
- Partial implementation counts as the lower level
- Evidence should be documented and auditable

## Building a Security Roadmap with SAMM

### Phase 1 (0-90 days): Quick Wins
- Enable SAST/SCA in CI pipelines (Implementation → Secure Build, Level 1)
- Establish vulnerability tracking and SLAs (Implementation → Defect Management, Level 1)
- Deploy basic security training (Governance → Education, Level 1)
- Identify and nominate Security Champions (Governance → Education, Level 1)

### Phase 2 (90-180 days): Foundation
- Implement threat modeling for all new features (Design → Threat Assessment, Level 2)
- Standardize secure coding guidelines per language (Governance → Education, Level 2)
- Establish security requirements for critical applications (Design → Security Requirements, Level 2)
- Deploy DAST against staging environments (Verification → Security Testing, Level 2)

### Phase 3 (180-365 days): Maturation
- Integrate security metrics into executive reporting (Governance → Strategy & Metrics, Level 2)
- Conduct architecture assessments for critical applications (Verification → Architecture Assessment, Level 2)
- Build incident response playbooks and run tabletop exercises (Operations → Incident Management, Level 2)
- Establish formal security review gates in the SDLC (Governance → Policy & Compliance, Level 2)

### Phase 4 (Year 2+): Optimization
- Automate common remediation patterns (Implementation → Defect Management, Level 3)
- Implement predictive security metrics (Governance → Strategy & Metrics, Level 3)
- Conduct red team exercises (Verification → Security Testing, Level 3)
- Benchmark against industry peers using SAMM Benchmark Project

## Key Metrics to Track

| Metric | What It Measures | Target Direction |
|--------|-----------------|-----------------|
| MTTR (Mean Time to Remediate) | Speed of vulnerability resolution | ↓ Lower is better |
| Vulnerability Escape Rate | Vulns found in prod vs. pre-prod | ↓ Lower is better |
| SAST Coverage | % of repos with SAST enabled | ↑ Higher is better |
| Threat Model Coverage | % of features with threat models | ↑ Higher is better |
| Training Completion Rate | % of developers completing security training | ↑ Higher is better |
| Security Debt Ratio | Open security findings / total findings | ↓ Lower is better |
| Champion Coverage | % of teams with active Security Champion | ↑ Higher is better |
