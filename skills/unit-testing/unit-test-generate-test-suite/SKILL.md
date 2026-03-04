# Skill: unit-test-generate-test-suite
> TestForge AI | Orchestration Skill

## What This Skill Does
Orchestrates end-to-end unit test suite generation from any structured input.
Coordinates agents and atomic skills into a single workflow — from raw input
to a reviewed, approved, runnable test suite.

---

## When to Apply This Skill
- A developer provides source code and asks for unit tests
- A CI/CD pipeline triggers test generation for a new commit or PR
- A team wants to bootstrap tests for an untested module
- Any input (code, spec, requirement) needs to produce a runnable test file

---

## Atomic Skills to Load First
Read these files before executing any step — they contain the detailed
instructions this orchestration skill depends on:

1. `../unit-test-generating-unit-tests/SKILL.md`
   — Test matrix methodology, AAA pattern, mocking, parametrization,
     exception testing, and framework-specific code examples

2. `../unit-test-analyzing-code-coverage/SKILL.md`
   — Coverage type definitions and gap report format, used when
     producing coverage_plan.md at the end of generation

---

## Execution Steps

Read `references/index.md` before executing any step.

### Step 1 — Classify Input
Identify what has been provided and how to interpret it:

  source_code   → extract signatures, logic paths, side effects
  requirements  → extract acceptance criteria and business rules
  openapi_spec  → extract endpoints, parameters, response schemas
  mixed         → combine all of the above

Also identify: target language, test framework (or auto-detect), scope (function | class | module).

### Step 2 — Invoke test-unit-gen-agent
Hand off to `../../agents/unit-testing/test-unit-gen-agent.md` with the classified input.

The agent will:
- Build the test case matrix (following unit-test-generating-unit-tests skill)
- Author all test cases using AAA pattern
- Assemble the test file and coverage_plan.md

### Step 3 — Invoke test-unit-review-agent
Hand off the generated suite to `../../agents/unit-testing/test-unit-review-agent.md`.

The agent returns one of three verdicts:
  APPROVED               → proceed to delivery
  APPROVED_WITH_NOTES    → proceed to delivery with report attached
  REQUIRES_REVISION      → return to Step 2 with annotated fix list

Maximum revision cycles: 2.
If still REQUIRES_REVISION after 2 cycles, deliver with full issue report for human resolution.

---

## Output Deliverables
```
test_<module>.<ext>    <- runnable test suite (pytest, Jest, JUnit 5, xUnit, etc.)
coverage_plan.md       <- coverage intent summary and gap notes
review_report.md       <- quality verdict and any reviewer notes
```
