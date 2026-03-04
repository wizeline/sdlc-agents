# NIST SSDF (SP 800-218) — Secure Software Development Framework Reference

## Overview

The NIST Secure Software Development Framework defines *how* to organize people and
processes to build software securely. It is outcome-based and technology-agnostic,
providing high-level practices that apply regardless of programming language, platform,
or methodology.

## Core Pillars

### PO: Prepare the Organization

The foundation of secure development is organizational readiness. You can't code
your way out of a culture problem.

**PO.1 — Define Security Requirements for Software Development**
- Document all security requirements for the development infrastructure
- Include requirements from laws, regulations, standards, and internal policies
- Make requirements accessible to all personnel involved in software development

**PO.2 — Implement Roles and Responsibilities**
- Assign security roles across the SDLC (architects, developers, testers, operations)
- Ensure management support with budget and authority for security activities
- Integrate security responsibilities into job descriptions and performance reviews

**PO.3 — Implement Supporting Toolchains**
- Deploy automated security testing tools (SAST, DAST, SCA, secrets scanning)
- Integrate security tooling into the CI/CD pipeline
- Provide developers with approved libraries and secure coding guidelines

**PO.4 — Define and Use Criteria for Security Checks and Audits**
- Establish security gates at key SDLC milestones (design review, code review, pre-release)
- Define pass/fail criteria for each gate
- Track and report on gate pass rates as a metric

**PO.5 — Implement and Maintain Secure Environments**
- Harden development, build, and staging environments
- Separate development, testing, and production environments
- Apply principle of least privilege to all infrastructure access

### PS: Protect the Software

Ensure the integrity of code and the development environment. A compromised build
pipeline makes every other security control meaningless.

**PS.1 — Protect All Forms of Code from Unauthorized Access and Tampering**
- Store source code in access-controlled repositories with audit logging
- Apply branch protection rules (require reviews, signed commits, status checks)
- Protect build scripts, IaC templates, and configuration-as-code with same rigor as application code

**PS.2 — Provide a Mechanism for Verifying Software Release Integrity**
- Cryptographically sign all release artifacts
- Publish checksums alongside releases
- Implement SLSA (Supply-chain Levels for Software Artifacts) framework practices
- Enable consumers to verify the authenticity and integrity of software they receive

**PS.3 — Archive and Protect Each Software Release**
- Maintain archives of all released versions
- Protect archives from tampering
- Enable the ability to reproduce builds for incident investigation

### PW: Produce Well-Secured Software

This is where secure coding meets process discipline. The goal is to catch flaws
early, when they're cheapest to fix.

**PW.1 — Design Software to Meet Security Requirements**
- Conduct threat modeling during the design phase
- Use secure design patterns from established references
- Document security-relevant design decisions and their rationale
- Validate that the design satisfies all requirements identified in PO.1

**PW.2 — Review the Software Design**
- Perform architecture security reviews before implementation begins
- Identify trust boundaries and validate that controls exist at each boundary
- Review data flow diagrams for sensitive data handling

**PW.4 — Reuse Existing, Well-Secured Software When Feasible**
- Maintain an approved component registry
- Vet new dependencies for known vulnerabilities, license compliance, and maintenance health
- Track all components in an SBOM (Software Bill of Materials)
- Monitor dependencies for newly discovered vulnerabilities post-deployment

**PW.5 — Create Source Code by Adhering to Secure Coding Practices**
- Follow language-specific secure coding standards (e.g., CERT Secure Coding Standards)
- Use static analysis tools configured for the project's language and risk profile
- Conduct peer code reviews with security as an explicit review criterion
- Address all SAST findings above the team's defined severity threshold

**PW.6 — Configure the Compilation, Interpreter, and Build Processes**
- Enable compiler security features (stack canaries, ASLR, DEP/NX, PIE)
- Use hardened build configurations; disable debug symbols in release builds
- Ensure reproducible builds where possible

**PW.7 — Review and Test Code for Vulnerabilities**
- Integrate SAST in the CI pipeline (run on every commit or PR)
- Integrate DAST against deployed applications (run in staging)
- Perform SCA to identify vulnerable dependencies
- Conduct manual penetration testing for critical releases
- Use fuzz testing for parsing and protocol-handling code

**PW.8 — Configure Software to Have Secure Settings by Default**
- Ship software with secure defaults (HTTPS, minimal permissions, logging enabled)
- Require explicit opt-in for features that weaken security
- Document all security-relevant configuration options

**PW.9 — Remove All Vulnerable Dependencies**
- Patch or replace vulnerable dependencies within defined SLAs
- Define maximum acceptable timeframes: Critical (24-48 hours), High (1 week), Medium (30 days)
- Track remediation velocity as a team metric

### RV: Respond to Vulnerabilities

No process is perfect. This pillar closes the feedback loop, ensuring that
discoveries in production drive improvements in development.

**RV.1 — Identify and Confirm Vulnerabilities on an Ongoing Basis**
- Monitor vulnerability feeds and advisories for all components
- Maintain a Vulnerability Disclosure Program (VDP) for external reporters
- Correlate runtime monitoring data with known vulnerability patterns

**RV.2 — Assess, Prioritize, and Remediate Vulnerabilities**
- Triage vulnerabilities using CVSS and contextual business impact
- Define remediation SLAs by severity
- Track mean time to remediate (MTTR) as a key metric

**RV.3 — Analyze Vulnerabilities to Identify Root Causes**
- Perform root cause analysis on every Critical/High vulnerability
- Determine if the flaw was introduced by design, coding, configuration, or dependency
- Feed findings back into training (PO), secure coding practices (PW.5), and design reviews (PW.2)

## Implementing SSDF in Practice

### For Small Teams
Start with PW (Produce) — integrate SAST and SCA into your CI/CD pipeline.
This gives immediate visibility into code-level issues. Then expand to PO
(training and requirements) and RV (vulnerability response) as the team matures.

### For Enterprises
Begin with PO (Prepare) — establish the organizational framework, training, and
tooling. Deploy PS (Protect) across the build infrastructure. Roll out PW (Produce)
practices team by team, using Security Champions as force multipliers. Stand up
RV (Respond) processes in parallel.

### Mapping to Executive Communication
- PO → "We have the right people, training, and tools"
- PS → "Our build pipeline is trustworthy"
- PW → "We're building it right"
- RV → "We can respond quickly when something goes wrong"
