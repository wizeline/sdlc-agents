# Unit Testing Agent — AI Core

> **AI Core** | Engineering Productivity | From source code or requirements to reviewed, runnable test suites.

The **Unit Testing Agent** (internally called *TestForge AI*) eliminates manual unit test scripting by converting source code, requirements, API specs, and user stories into production-ready, immediately executable unit test suites. It enforces shift-left testing principles — tests can be the *first* artifact produced for a feature, not the last.

---

## Architecture Overview

```
unit-testing-agent/
├── agents/
│   ├── test-unit-gen-agent.md    ← Generates test suites from any structured input
│   └── test-unit-review-agent.md ← Quality gate: reviews coverage, structure, and correctness
└── skills/
    ├── unit-test-generating-unit-tests/        ← Atomic: AAA pattern, test matrix, mocking
    ├── unit-test-analyzing-code-coverage/      ← Atomic: gap detection, severity classification
    ├── unit-test-authoring-test-plans/         ← Atomic: 10-section test plan structure, Gherkin
    ├── unit-test-generating-test-suite/        ← Orchestration: end-to-end gen + review workflow
    ├── unit-test-reviewing-test-suite/         ← Orchestration: standalone quality audit
    ├── unit-test-running-coverage-analysis/    ← Orchestration: gap analysis with recommendations
    ├── unit-test-creating-test-plan/           ← Orchestration: formal test plan document
    └── unit-test-shifting-left-from-requirements/ ← Orchestration: TDD red phase from requirements
```

---

## MCP Connections

| Server      | URL                              | Used By                                      |
|-------------|----------------------------------|----------------------------------------------|
| `atlassian` | `https://mcp.atlassian.com/v1/mcp` | `test-unit-gen-agent`, `test-unit-review-agent` |

The Atlassian MCP connection enables fetching user stories and acceptance criteria directly from Jira, enabling true shift-left testing — generating test skeletons from requirements before any implementation code exists.

---

## TDD Principles (Enforced Across All Agents and Skills)

1. **Red-Green-Refactor** — tests define behavior first; implementation follows
2. **Shift-left first** — tests are produced before or alongside code, never after
3. **One behavior per test** — each test function covers exactly one scenario
4. **Deterministic** — no uncontrolled randomness or time dependency; always seed or mock
5. **Mock only externalities** — DBs, APIs, file I/O, clocks only; never mock internal logic
6. **Behavior over implementation** — tests verify contracts and outputs, survive refactoring
7. **Coverage-driven** — branch coverage prioritized; 80%+ required on critical paths
8. **Framework-native** — output is directly executable with zero modification
9. **AAA structure** — Arrange / Act / Assert enforced throughout
10. **Spec-faithful** — tests reflect actual requirements, not assumed behavior

---

## Agents

### `test-unit-gen-agent`

**Description:** Primary test generator. Converts any structured input (source code, requirements, API specs, user stories) into production-ready, immediately runnable unit test suites. Enforces the AAA pattern, builds a test case matrix before writing a single line of code, and produces a `coverage_plan.md` alongside the test file.

**Triggers:** providing source code, requirements, or an OpenAPI spec and asking for unit tests

**Skills loaded:** `unit-test-generating-unit-tests`, `unit-test-analyzing-code-coverage`

**Supported frameworks:**
| Language | Default | Alternatives |
|----------|---------|--------------|
| Python | pytest | unittest |
| JavaScript | Jest | Vitest, Mocha |
| TypeScript | Jest+ts-jest | Vitest |
| Java | JUnit 5 | TestNG |
| C# | xUnit | NUnit, MSTest |
| Go | testing | testify |
| Ruby | RSpec | Minitest |
| Rust | cargo test | — |

**Examples:**
```
"Generate pytest tests for this authentication module"
"Write JUnit 5 tests for this Java payment service class"
"I have a user story for a checkout flow — write tests before the code exists"
"Here's my OpenAPI spec — generate unit tests for each endpoint"
"Generate tests for all edge cases in this discount calculation function"
```

---

### `test-unit-review-agent`

**Description:** Quality gate for all generated unit tests. Reviews test suites for correctness, coverage completeness, structural quality, and framework compliance. Returns one of three verdicts: `APPROVED`, `APPROVED_WITH_NOTES`, or `REQUIRES_REVISION`. Automatically called by `test-unit-gen-agent` — can also be invoked standalone for existing test suites.

**Triggers:** reviewing or auditing an existing test file; CI/CD quality gate

**Skills loaded:** `unit-test-analyzing-code-coverage`, `unit-test-generating-unit-tests`, `unit-test-authoring-test-plans`

**10 Review Dimensions:**
1. Coverage Completeness
2. Test Correctness
3. Test Independence & Isolation
4. Brittle Test Detection
5. Over-Mocking Detection
6. Naming & Readability
7. Framework Compliance
8. Security & Safety (no real credentials or PII in test data)
9. False Confidence Check
10. Test Code Quality

**Verdicts:**
| Verdict | Criteria |
|---------|---------|
| `APPROVED` | Zero HIGH or MEDIUM issues |
| `APPROVED_WITH_NOTES` | Zero HIGH, one or more LOW issues |
| `REQUIRES_REVISION` | One or more HIGH or MEDIUM issues → returns to gen-agent with fix list |

**Examples:**
```
"Review this existing pytest test file for quality issues"
"Audit our test suite before the v2.0.0 release"
"Act as a CI quality gate — evaluate these generated tests"
"Our team lead wants a test suite quality report before the PR is merged"
```

---

## Skills

### `unit-test-generating-unit-tests` *(Atomic)*

**Description:** The core test authoring skill. Converts structured inputs into framework-native, immediately runnable unit tests using the Arrange-Act-Assert (AAA) pattern.

**Workflow:**
1. Understand the unit (inputs, outputs, side effects, error states)
2. Build the test case matrix (happy path, boundary, error, side effects)
3. Write each test in AAA pattern with self-documenting names
4. Mock external dependencies (DB, API, file I/O, clocks)
5. Parametrize for multiple cases
6. Exception testing

**Naming convention:** `test_<unit>_when_<condition>_expects_<outcome>`

**Example:**
```
"Write a test for calculate_discount() — cover happy path, unknown tier, zero price, and negative price"
```

---

### `unit-test-analyzing-code-coverage` *(Atomic)*

**Description:** Detects coverage gaps in existing or newly generated test suites. Produces a prioritized gap report and recommended tests to close each gap.

**Coverage Types:**
| Type | Target |
|------|--------|
| Line Coverage | ≥ 80% |
| Branch Coverage | ≥ 75% (highest priority) |
| Function Coverage | ≥ 90% |
| Statement Coverage | ≥ 80% |

**Gap Severity:** CRITICAL → HIGH → MEDIUM → LOW

**Supported Tools:** `coverage.py` (Python), `Istanbul/c8` (JavaScript), `JaCoCo` (Java), `coverlet` (C#)

**Example:**
```
"Analyze this coverage.json report and tell me the highest-priority gaps to close"
```

---

### `unit-test-authoring-test-plans` *(Atomic)*

**Description:** Produces formal test plan documents (Markdown or `.docx`) for team handoffs, QA sign-off, sprint documentation, and compliance audit trails.

**10-Section Structure:** Cover page, Scope, Test Objectives, Test Strategy, Test Cases Summary Table, Coverage Targets, Dependencies & Risks, Entry/Exit Criteria, Defect Management, Approvals

**BDD/Gherkin support:** Generates Feature/Scenario/Given/When/Then scenario documents when requirements or user stories are provided.

**Output formats:** `.md` (default) or `.docx` (professional QA sign-off format)

**Example:**
```
"Create a formal test plan document for the user authentication module — include BDD scenarios"
```

---

### `unit-test-generating-test-suite` *(Orchestration)*

**Description:** End-to-end test suite generation workflow. Coordinates `test-unit-gen-agent` and `test-unit-review-agent` into a single supervised pipeline with up to 2 revision cycles.

**Workflow:** Classify input → Generate suite (gen-agent) → Review suite (review-agent) → Revise if needed → Deliver

**Outputs:**
```
test_<module>.<ext>     ← runnable test suite
coverage_plan.md        ← coverage intent summary
review_report.md        ← quality verdict and reviewer notes
```

**Example:**
```
"Generate a complete, reviewed test suite for this checkout module"
```

---

### `unit-test-reviewing-test-suite` *(Orchestration)*

**Description:** Standalone quality review of any test suite. Uses `test-unit-review-agent` to evaluate across 6 core dimensions and produce a structured report.

**Outputs:**
```
review_report.md          ← full quality report with verdict
revision_instructions.md  ← (REQUIRES_REVISION only) annotated fix list
```

**Example:**
```
"Run a standalone quality audit on our existing Jest test suite"
```

---

### `unit-test-running-coverage-analysis` *(Orchestration)*

**Description:** Focused coverage gap analysis. Produces a prioritized gap report, a concrete list of recommended tests, and a projected coverage delta — without generating any test code.

**Outputs:**
```
coverage_gap_report.md        ← prioritized gap table
recommended_tests.md          ← specific named tests to write
coverage_delta_estimate.md    ← projected coverage % after changes
```

**Example:**
```
"Analyze our coverage report and tell me exactly what tests to write to reach 80% branch coverage"
```

---

### `unit-test-creating-test-plan` *(Orchestration)*

**Description:** Orchestrates the creation of a formal test plan document by combining test suite data, coverage analysis, and requirements into a structured 10-section plan.

**Outputs:**
```
test_plan_<feature_or_module>.md      ← always produced
test_plan_<feature_or_module>.docx    ← produced when --format docx is requested
```

**Example:**
```
"Create a formal test plan for the payments feature — include coverage targets and BDD scenarios"
```

---

### `unit-test-shifting-left-from-requirements` *(Orchestration)*

**Description:** Generates unit test skeletons and BDD scenarios directly from requirements, user stories, or PRDs — before any implementation code exists. Operationalizes the **Red phase** of the Red-Green-Refactor cycle at scale.

**Workflow:** Parse requirements → Build test case matrix → Generate failing test skeletons → Produce BDD/Gherkin scenarios → Flag ambiguities

**Outputs:**
```
test_<feature>_skeleton.<ext>     ← failing test skeletons ready for implementation
gherkin_scenarios_<feature>.md    ← BDD scenarios for QA and product alignment
requirements_coverage_map.md      ← maps each requirement to test IDs
ambiguities.md                    ← unclear or incomplete requirements
```

**Example:**
```
"I have these user stories for the login feature — generate test skeletons before we start coding"
"Make tests the first artifact for this sprint feature using TDD"
```

---

## Supported Inputs

- Source code (functions, classes, modules — any language)
- Requirements / user stories / PRDs
- OpenAPI / Swagger specs
- Historical test suites (pattern matching for extension)
- Coverage reports (JSON, XML, HTML — from coverage.py, Istanbul, JaCoCo, coverlet)

## Supported Outputs

- Unit test suites: pytest, JUnit 5, xUnit, Jest, Vitest, RSpec, Go testing, cargo test
- Coverage reports (JSON, HTML, Markdown)
- Test plan documents (`.md`, `.docx`)
- BDD/Gherkin scenario files
- Requirements coverage maps

---

## KPIs This AI Core Drives

| Metric | Description |
|--------|-------------|
| Unit Test Coverage Rate | % of lines/branches covered by generated tests |
| Tests Generated per Sprint | Volume of automated test authoring |
| Manual Scripting Hours Saved | Engineering time reclaimed |
| Test-to-Code Ratio | Balance of tests vs implementation |
| Defect Escape Rate | Bugs that slip past unit-level testing |

---

## Installation

### Install the full AI Core (all agents)
```bash
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/unit-testing-agent -a gemini
```

### Install a single agent
```bash
npx agents add https://github.com/wizeline/sdlc-agents/tree/main/aicores/unit-testing-agent/agents/test-unit-gen-agent -a gemini
```

### Install a single skill
```bash
npx skills add https://github.com/wizeline/sdlc-agents/tree/main/aicores/unit-testing-agent/skills/unit-test-generating-unit-tests -a gemini
```
