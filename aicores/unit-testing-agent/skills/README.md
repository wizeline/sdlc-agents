# TestForge AI — Unit Testing Agent

> AI Core | Engineering Productivity | Status: Inception

## What This Is

TestForge AI eliminates manual unit test scripting by converting source code, requirements,
and specs into executable unit test suites — enabling shift-left coverage at the function
and class level before implementation is complete.

## Project Structure

```
testforge-ai/
  README.md                                    <- you are here
  agents/
    test-unit-gen-agent/                       <- generates test suites
    test-unit-review-agent/                    <- reviews & validates tests
  skills/
    unit-test-generating-unit-tests/           <- atomic: how to author tests
    unit-test-analyzing-code-coverage/         <- atomic: how to detect gaps
    unit-test-authoring-test-plans/            <- atomic: how to write test plans
    unit-test-generating-test-suite/           <- orchestration: full gen workflow
    unit-test-reviewing-test-suite/             <- orchestration: standalone review
    unit-test-running-coverage-analysis/       <- orchestration: gap analysis
    unit-test-creating-test-plan/              <- orchestration: plan document
    unit-test-shifting-left-from-requirements/ <- orchestration: TDD red phase
```

## TDD Principles (Applied Across All Agents and Skills)

Every agent and skill in this project enforces the following non-negotiable practices:

1. **Red-Green-Refactor** — tests are written to define behavior first; implementation follows
2. **Shift-left first** — tests are produced before or alongside code, never after
3. **One behavior per test** — each test function covers exactly one scenario
4. **Deterministic** — no uncontrolled randomness or time dependency; always seed or mock
5. **Mock only externalities** — DBs, APIs, file I/O, clocks only; never mock internal logic
6. **Behavior over implementation** — tests verify contracts and outputs, survive refactoring
7. **Coverage-driven** — branch coverage prioritized; 80%+ required on critical paths
8. **Framework-native** — output is directly executable with zero modification
9. **AAA structure** — Arrange / Act / Assert enforced throughout
10. **Spec-faithful** — tests reflect actual requirements, not assumed behavior

## Quick Start

1. Provide source code, requirements, or an API spec
2. Specify target language and test framework (or let the agent detect it)
3. Run test-unit-gen-agent → outputs runnable test suite
4. Run test-unit-review-agent → validates quality & coverage
5. (Optional) Run unit-test-authoring-test-plans skill → generates .md or .docx test plan

## Agents

| Agent                  | Role                                                     |
| ---------------------- | -------------------------------------------------------- |
| test-unit-gen-agent    | Primary generator — turns inputs into test suites       |
| test-unit-review-agent | Quality gate — reviews coverage, structure, correctness |

## Skills

| Skill                                  | Type          | Role                                                     |
|----------------------------------------|---------------|----------------------------------------------------------|
| unit-test-generating-unit-tests        | Atomic        | AAA authoring, test matrix, mocking, frameworks          |
| unit-test-analyzing-code-coverage      | Atomic        | Gap detection, severity classification, tool integration |
| unit-test-authoring-test-plans         | Atomic        | 10-section plan structure, Gherkin, .md/.docx output     |
| unit-test-generating-test-suite        | Orchestration | End-to-end gen + review workflow                         |
| unit-test-reviewing-test-suite          | Orchestration | Standalone quality audit                                 |
| unit-test-running-coverage-analysis     | Orchestration | Gap analysis with prioritized recommendations            |
| unit-test-creating-test-plan           | Orchestration | Formal test plan document creation                       |
| unit-test-shifting-left-from-requirements | Orchestration | TDD Red phase — tests from requirements before code     |

## Supported Inputs

- Source code (functions, classes, modules — any language)
- Requirements / user stories / PRDs
- OpenAPI / Swagger specs
- Wireframes / UI mockups
- Historical test suites (pattern matching)

## Supported Outputs

- Unit test suites: pytest, JUnit 5, xUnit, Jest, Vitest, RSpec, Go testing
- Coverage reports (JSON, HTML, Markdown)
- Test plan documents (.md, .docx)

## KPIs This Agent Drives

- Unit Test Coverage Rate (%)
- Tests Generated per Sprint
- Manual Scripting Hours Saved
- Test-to-Code Ratio
- Defect Escape Rate (unit level)
