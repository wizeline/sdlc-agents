# Skill: run-coverage-analysis
> TestForge AI | Orchestration Skill

## What This Skill Does
Orchestrates a focused coverage gap analysis for a source module or test suite.
Produces a prioritized gap report, a concrete list of recommended tests, and a
projected coverage delta — without generating any test code.

---

## When to Apply This Skill
- A developer asks "what am I missing?" after initial test generation
- A coverage report (coverage.py, Istanbul, JaCoCo) is available and needs interpretation
- Pre-sprint planning requires a gap estimate and effort forecast for a module
- A team wants to know exactly what it takes to reach a coverage target (e.g. 80%)

---

## Atomic Skills to Load First
Read this file before executing any step:

1. `../analyzing-code-coverage/skill.md`
   — All four coverage types (line / branch / function / statement), gap severity
     classification, tool integration guides (coverage.py, Istanbul, JaCoCo, coverlet),
     anti-pattern detection, and all three output report formats

---

## Execution Steps

### Step 1 — Receive Input
Accepted inputs (one or more):
  - Coverage report: JSON (coverage.py / Istanbul), XML (JaCoCo / Cobertura), HTML
  - Source code file (for structural gap analysis when no report is provided)
  - Existing test file (to cross-reference against source for uncalled paths)
  - Coverage target, e.g. "80% line, 75% branch" (defaults to these if not specified)

### Step 2 — Analyze
Follow the full analysis workflow from `../analyzing-code-coverage/skill.md`:

  - Parse coverage data or analyze source structure statically
  - Identify: uncovered lines, untaken branches, uncalled functions, unhandled exceptions
  - Classify every gap: CRITICAL | HIGH | MEDIUM | LOW
  - Flag coverage anti-patterns (over-permissive mocks, integration tests masking gaps)

### Step 3 — Produce Outputs
Generate all three deliverables defined in the analyzing-code-coverage skill.

---

## Output Deliverables
```
coverage_gap_report.md        <- prioritized gap table (severity, location, description)
recommended_tests.md          <- specific named tests to write, with descriptions
coverage_delta_estimate.md    <- projected coverage % once recommended tests are added
```
