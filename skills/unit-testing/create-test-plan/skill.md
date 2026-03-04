# Skill: create-test-plan
> TestForge AI | Orchestration Skill

## What This Skill Does
Orchestrates the creation of a formal test plan document for a feature, module,
or sprint. Combines test suite data, coverage analysis, and requirements into a
structured document suitable for QA sign-off, team handoffs, or audit trails.

---

## When to Apply This Skill
- A stakeholder or QA team needs a human-readable summary of the testing strategy
- A sprint or feature requires formal test documentation before work begins
- A compliance or audit process requires evidence of test planning
- A developer wants a Markdown test plan committed alongside their test file

---

## Atomic Skills to Load First
Read these files before executing any step:

1. `../authoring-test-plans/skill.md`
   — Full 10-section test plan structure, BDD/Gherkin scenario format, coverage
     target tables, entry/exit criteria, approval tables, and output format guidance
     for both Markdown and .docx

2. `../analyzing-code-coverage/skill.md`
   — Used to populate the Coverage Targets section (Section 6) with accurate
     current vs target metrics and gap commentary

---

## Execution Steps

### Step 1 — Gather Inputs
Collect as many of the following as are available:
  - Generated test suite (for populating the test cases table in Section 5)
  - Coverage report or gap analysis (for Section 6 — Coverage Targets)
  - Requirements, user stories, or PRD (for scope, objectives, and BDD scenarios)
  - Preferred output format: md (default) or docx

### Step 2 — Author the Test Plan
Follow the full 10-section structure from `../authoring-test-plans/skill.md`:

  1. Cover Page / Header
  2. Scope (in scope, out of scope, target environments)
  3. Test Objectives (measurable goals)
  4. Test Strategy (framework, coverage tool, execution method, BDD flag)
  5. Test Cases Summary Table (ID, name, category, priority, status)
  6. Coverage Targets (current vs target per coverage type)
  7. Dependencies and Risks
  8. Entry and Exit Criteria
  9. Defect Management
  10. Approvals

Include BDD / Gherkin scenarios in Section 4 when requirements or user stories
are provided as input.

### Step 3 — Format Output
  - md format: emit directly as Markdown, ready to commit to the repository
  - docx format: apply the docx skill to convert the Markdown into a
    professionally formatted Word document with heading styles and page numbers

---

## Output Deliverables
```
test_plan_<feature_or_module>.md      <- always produced
test_plan_<feature_or_module>.docx    <- produced only when --format docx is requested
```
