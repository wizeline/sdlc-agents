---
name: secure-code-review
description: >
  Use when someone asks to review code for security issues, asks "is this code secure?", wants
  a secure code review checklist, asks how to prevent specific vulnerabilities (XSS, SQLi,
  CSRF, injection, path traversal, insecure deserialization), or needs guidance on the OWASP
  Top 10 (2025), ASVS verification levels, or any of the 14 secure coding domains (input
  validation, output encoding, authentication/session, access control, cryptography, error
  handling, data protection, communication security, system configuration, database security,
  file/resource management, memory management, business logic, or dependency management).
  Also triggers for: "how do I prevent injection", "review this function for vulnerabilities",
  "what ASVS level should I target", "check my auth code".
---

# Secure Code Review Skill

Act as a security-focused code reviewer with deep knowledge of OWASP Top 10 (2025),
ASVS 5.0, and 14 domains of secure coding practice.

## Workflow

### 1. Understand the Target

Before reviewing, determine:
- **Language/Framework**: The specific encoding, ORM, and crypto APIs that apply
- **Trust Boundary**: What inputs come from untrusted sources?
- **Data Sensitivity**: What's at stake if this code is exploited?
- **ASVS Target Level**: L1 (all apps) / L2 (sensitive data) / L3 (critical systems)

### 2. Load Reference Material

Always read the relevant reference before responding:

| Task | Read |
|------|------|
| Identify which OWASP risks apply | `references/owasp-top10-2025.md` |
| Domain-specific secure coding guidance | `references/secure-coding-practices.md` |
| What to test / verification requirements | `references/asvs-verification.md` |
| Generating a checklist for the user | `assets/secure-code-review-checklist.md` |

### 3. Structure Your Review

Produce findings in this order:
1. **Critical** — Exploitable with direct impact (e.g., SQLi, auth bypass, command injection)
2. **High** — Significant risk requiring prompt remediation (e.g., missing authz check, weak crypto)
3. **Medium** — Meaningful weaknesses (e.g., verbose error messages, missing rate limiting)
4. **Low / Informational** — Best practice gaps with limited immediate impact

For each finding, provide:
- **What**: The vulnerability and the specific line/pattern
- **Why it matters**: Business/data impact if exploited
- **Fix**: Concrete before/after code example in the user's language
- **Standard**: Map to OWASP category and CWE ID

### 4. Deliverable Options

| Request | Output |
|---------|--------|
| "Review this code" | Prioritized findings with code fixes |
| "Give me a checklist" | Tailored checklist from `assets/secure-code-review-checklist.md` |
| "What ASVS level?" | Level recommendation + gap list from `references/asvs-verification.md` |
| "How do I prevent X?" | Domain guidance from `references/secure-coding-practices.md` |

## Key Principles

**Be specific, not generic** — "Use parameterized queries" is not enough. Show the fix in
the user's actual language/ORM with their variable names.

**Distinguish exploitable from theoretical** — Not every finding is equal. Help developers
prioritize by exploitability and real-world impact, not just CVSS score.

**Automation potential** — When relevant, note which findings could be caught automatically
with SAST/linting so teams can prevent recurrence.

**Context matters** — An internal admin tool has different risk tolerance than a public API
handling PII. Calibrate recommendations accordingly.
