# Skill: unit-test-review-test-suite
> TestForge AI | Orchestration Skill

## What This Skill Does
Orchestrates a standalone quality review of any existing or newly generated unit
test suite. Produces a structured review report with severity-tagged findings
and an unambiguous verdict.

---

## When to Apply This Skill
- A developer submits an existing test file for quality audit
- A team lead wants to assess test quality before a release or merge
- A CI/CD gate requires a quality verdict before a PR is approved
- A test suite generated externally needs validation before use

---

## Atomic Skills to Load First
Read these files before executing any step:

1. `../unit-test-analyzing-code-coverage/SKILL.md`
   — Coverage type definitions, severity classification (CRITICAL / HIGH / MEDIUM / LOW),
     and gap report format — the reference standard for coverage dimension review

2. `../unit-test-generating-unit-tests/SKILL.md`
   — AAA pattern, naming conventions, correct mocking structure, and exception testing
     patterns — the reference standard for judging test quality under review

---

## Execution Steps

Read `references/index.md` before executing any step.

### Step 1 — Receive Input
Accepted inputs (one or more):
  - Test file(s) to review (required)
  - Source code the tests are written against (recommended)
  - Coverage report: JSON (coverage.py / Istanbul), XML (JaCoCo / Cobertura)
  - Requirements or user stories the tests should satisfy

### Step 2 — Invoke test-unit-review-agent
Pass all inputs to `../../agents/unit-testing/test-unit-review-agent.md`.

The agent evaluates across 6 dimensions:
  1. Coverage Completeness     — all paths, branches, and edge cases addressed
  2. Test Correctness          — assertions verify the right things, no false positives
  3. Test Independence         — no shared mutable state, no order dependency
  4. Naming and Readability    — self-documenting names, AAA structure visible
  5. Framework Compliance      — correct imports, decorators, no deprecated APIs
  6. Security and Safety       — no real credentials, PII, or live network calls

### Step 3 — Deliver Review Report
Always emit a structured report containing:
  - Coverage Score (X/10) and Quality Score (X/10)
  - Passed checks list
  - Issues table: Severity | Location | Issue | Suggested Fix
  - Recommended additions (specific missing test cases by name)
  - Final verdict

---

## Output Deliverables
```
review_report.md          <- full quality report with verdict
revision_instructions.md  <- (REQUIRES_REVISION only) annotated fix list
                             ready to pass directly to test-unit-gen-agent
```
