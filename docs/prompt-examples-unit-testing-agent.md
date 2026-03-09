# Unit Testing Agent — Prompt Examples

Ready-to-use prompts for the **Unit Testing** AI Core.
This core includes **2 agents** backed by **7 skills**.

> **How to use**: Copy any prompt below and paste it into your AI coding assistant.
> The agent will automatically load the required skill references, analyze your context, and deliver runnable test output.

---

## Agent → Skill Quick Reference

| Agent | Skills Used |
| --- | --- |
| `test-unit-gen-agent` | `unit-test-generating-unit-tests`, `unit-test-analyzing-code-coverage` |
| `test-unit-review-agent` | `unit-test-analyzing-code-coverage`, `unit-test-generating-unit-tests`, `unit-test-authoring-test-plans` |

### Orchestration Skills (multi-skill pipelines)

| Skill | What It Coordinates |
| --- | --- |
| `unit-test-generating-test-suite` | Generate → Analyze → Review (full end-to-end) |
| `unit-test-reviewing-test-suite` | Analyze + Review (quality gate, no generation) |
| `unit-test-running-coverage-analysis` | Coverage gap analysis only |
| `unit-test-creating-test-plan` | Test plan document from suite + coverage data |
| `unit-test-shifting-left-from-requirements` | Tests and BDD scenarios from requirements, before code exists |

---

## Skill Outputs Quick Reference

| Output | What It Contains |
| --- | --- |
| **Test File** | Runnable, framework-native test suite following the AAA pattern |
| **coverage_plan.md** | Coverage intent: what is tested vs planned, gap list |
| **Review Report** | Scored findings across 10 dimensions, verdict (APPROVED / REQUIRES_REVISION) |
| **Gap Report** | Prioritized untested paths, projected coverage delta, effort estimate |
| **Test Plan Document** | 10-section Markdown plan suitable for QA sign-off or audit |
| **Shift-Left Skeletons** | Failing test stubs + Gherkin scenarios derived directly from requirements |

---

## `test-unit-gen-agent` — Test Generation

> **Persona:** Developer who needs runnable tests from source code, specs, or requirements

### From source code

```text
Generate a pytest suite for the validate_email() function below.
Cover: happy path, empty string, malformed addresses, None input, and internationalized domains.
Output the test file and a coverage_plan.md.

[paste source code]
```

```text
Write JUnit 5 tests for the OrderService class. Mock the InventoryRepository and
PaymentGateway dependencies. Name every test: test_<method>_when_<condition>_expects_<outcome>.

[paste source code]
```

```text
Write Go table-driven tests using the standard testing package for the ParseConfig function.
Cover: valid config, missing required fields, invalid types, and empty input file.

[paste source code]
```

```text
Generate xUnit tests (C#) for the calculate_tax() function. Cover all boundary values
and the three tax bracket edge cases. Name each test clearly after its scenario.

[paste source code]
```

### From an OpenAPI spec

```text
Generate Jest tests for every endpoint in this OpenAPI spec. Mock all HTTP calls with msw.
Cover: 200 happy path, 400 validation errors, 401 unauthorized, 404 not found,
and 500 server error for each endpoint.

[paste OpenAPI YAML/JSON]
```

### From requirements (shift-left)

```text
I have a user story for a checkout flow and no implementation yet.
Write failing pytest tests before the code — these tests should act as the definition of done.
Also produce Gherkin BDD scenarios to align QA, dev, and product on expected behavior.

User Story: [paste user story and acceptance criteria]
```

```text
Generate tests from these acceptance criteria before any code is written.
Flag any criteria that are ambiguous or untestable as written.

Acceptance Criteria: [paste acceptance criteria]
```

---

## `test-unit-review-agent` — Test Quality Review

> **Persona:** Team lead or QA engineer auditing an existing test file

### Quality audit

```text
Audit this test file. Score coverage completeness and test quality out of 10.
Flag all issues in a table with severity (HIGH / MEDIUM / LOW) and a suggested fix for each.
Return a final verdict: APPROVED, APPROVED_WITH_NOTES, or REQUIRES_REVISION.

[paste test file]
```

```text
Review this test suite before I approve the PR. Focus on: missing branch coverage,
brittle assertions, and over-mocking. I need zero HIGH issues before I merge.

[paste test file]
```

### With test plan update

```text
Review this test suite and produce an updated test plan document reflecting the findings.
I need both the Review Report and the updated test_plan.md.

[paste test file]
```

---

## Full Pipeline — Both Agents in Sequence

These prompts trigger generation by `test-unit-gen-agent` followed by automatic
handoff to `test-unit-review-agent`.

```text
Generate a pytest test suite for the PaymentProcessor class below, then review it
for quality. Return the final test file and a Review Report with a verdict.

[paste source code]
```

```text
Generate Jest tests for this UserAuthService TypeScript class, then run a quality review.
Flag any mocking issues and brittle assertions. I need an APPROVED verdict before delivery.

[paste source code]
```

```text
I'm releasing checkout.go next week. Generate testify tests for this module,
review them for quality, and return the final test file with a Review Report.
Flag any HIGH or MEDIUM issues with exact fix instructions.

[paste source code]
```

---

## Standalone Orchestration Skills

These prompts exercise individual multi-skill pipelines directly.

---

### `unit-test-generating-test-suite` — End-to-end generation pipeline

```text
Generate a complete test suite for authentication.py.
Target framework: pytest. Output: test file, conftest.py if needed, and coverage_plan.md.

[paste source code]
```

```text
Generate a Vitest test suite for CartService.ts.
I need full branch coverage on the applyDiscount() method specifically.

[paste source code]
```

---

### `unit-test-reviewing-test-suite` — Standalone review quality gate

```text
Run a full quality review on the test file below.
Score all 10 review dimensions and list every issue found with severity and suggested fix.

[paste test file]
```

```text
Review test_payments.py for false-confidence issues — cases where tests pass
but coverage is misleading or mocks hide real behavior gaps.

[paste test file]
```

---

### `unit-test-running-coverage-analysis` — Coverage gap analysis only

```text
Analyze this coverage report. Classify every gap as CRITICAL / HIGH / MEDIUM / LOW.
Focus on the checkout flow — treat any gap there as CRITICAL regardless of percentage.
List the 5 highest-priority tests to write next.

[paste coverage.py / Istanbul / JaCoCo report]
```

```text
No tests exist for inventory_service.py yet. Produce a gap report showing projected
coverage delta if I add the top 10 recommended tests.

[paste source code]
```

---

### `unit-test-creating-test-plan` — Formal test plan document

```text
Produce a test plan for the Payments module. Inputs: source code and coverage report below.
I need a Markdown test plan document ready for QA sign-off.

[paste source code and coverage report]
```

```text
Generate a sprint test plan for Sprint 42. Modules in scope: user-auth, notifications, billing.
Include entry/exit criteria, coverage targets per module, and an approval table.
Stakeholders: QA lead, engineering manager, product owner.
```

---

### `unit-test-shifting-left-from-requirements` — Tests before code exists

```text
Write failing tests and Gherkin BDD scenarios from the user story below — no implementation exists.
These tests should be committed before any code is written and serve as the formal definition of done.
Flag any requirement that is ambiguous or currently untestable.

User Story:
As a registered user, I want to reset my password via email link,
so that I can regain access to my account if I forget my credentials.

Acceptance Criteria:
- Link expires after 24 hours
- Link is single-use
- User receives confirmation email after successful reset
- New password must meet complexity requirements (8+ chars, 1 number, 1 special char)
```

```text
I'm starting implementation of a rate-limiting feature. Produce failing test skeletons
from the PRD section below before I write a single line of implementation.
Use pytest. Also identify any acceptance criteria that cannot be unit-tested as written.

[paste PRD section]
```

---

## Full Suite — All Agents & Skills in One Prompt

The following prompt is designed to exercise **every agent and skill** in the Unit Testing AI Core.
Adapt the module names, framework, and coverage targets to your project.

```text
We are onboarding a new module to our test suite: a multi-layered checkout service
written in Python (FastAPI), with the following components:

- PaymentProcessor — handles Stripe API calls, idempotency keys, retry logic
- InventoryReserver — reserves stock with optimistic locking against PostgreSQL
- OrderOrchestrator — coordinates PaymentProcessor and InventoryReserver, emits
  domain events via Kafka, and handles rollback on partial failures

Context:
- No tests exist today for any of these components
- We have acceptance criteria and a PRD for the feature (attached below)
- We have a coverage report from a partial scan run last week (attached below)
- Target: 80% branch coverage on all three components before the next release
- Compliance: we are SOC 2 Type II certified; test plans must be audit-ready

Please complete the following in order:

1. Shift-Left Test Generation
   From the acceptance criteria and PRD alone — before reading the source code —
   produce failing pytest test skeletons and Gherkin BDD scenarios for each
   acceptance criterion. Flag any criterion that is ambiguous or untestable.
   These tests should be committed before implementation begins.

2. Full Test Suite Generation
   Generate a production-ready pytest test suite for all three components.
   Mock Stripe, PostgreSQL, and Kafka. Apply the AAA pattern throughout.
   Name every test: test_<unit>_when_<condition>_expects_<outcome>.
   Output: test_checkout.py, conftest.py, and coverage_plan.md.

3. Coverage Gap Analysis
   Using the coverage report and the generated test suite, identify all
   CRITICAL and HIGH coverage gaps. Estimate the effort to close each gap
   and project the coverage delta if the top 5 recommended tests are added.

4. Test Suite Quality Review
   Review the generated test suite across all quality dimensions:
   correctness, coverage completeness, test independence, brittle test
   detection, over-mocking, naming, framework compliance, and security safety.
   Return a Review Report with severity-tagged findings and a verdict.
   Any HIGH issue must include a concrete suggested fix.

5. Formal Test Plan Document
   Combine the test suite, coverage analysis, and review findings into a
   10-section Markdown test plan document. Include: entry/exit criteria,
   coverage targets per component, BDD scenarios, approval table with
   signatures for QA lead and engineering manager, and an evidence index
   suitable for SOC 2 Type II auditors.

Deliverables: test_checkout.py, conftest.py, coverage_plan.md, review_report.md, test_plan.md.

[paste source code]
[paste PRD / acceptance criteria]
[paste coverage report]
```

### What This Prompt Activates

| Step | Agent / Skill | Output |
| ---- | ------------- | ------ |
| 1. Shift-left | `unit-test-shifting-left-from-requirements` | Failing test skeletons + Gherkin scenarios |
| 2. Generation | `test-unit-gen-agent` → `unit-test-generating-unit-tests` | Test file + coverage_plan.md |
| 3. Gap analysis | `unit-test-running-coverage-analysis` → `unit-test-analyzing-code-coverage` | Gap report + coverage delta |
| 4. Review | `test-unit-review-agent` → `unit-test-reviewing-test-suite` | Review Report + verdict |
| 5. Test plan | `unit-test-creating-test-plan` → `unit-test-authoring-test-plans` | Audit-ready test plan document |

---

## Tips for Better Results

1. **Name your framework** — Mention pytest, Jest, JUnit 5, xUnit, etc. so the agent generates idiomatic code.
2. **Describe external dependencies** — "calls Stripe", "reads from PostgreSQL", "publishes to Kafka" triggers correct mocking patterns.
3. **State your coverage target** — "I need 80% branch coverage" helps the gap analysis prioritize what to recommend.
4. **Paste the source code or requirements** — The more context provided, the more complete the test matrix.
5. **Ask for specific outputs** — "Give me a coverage_plan.md", "Produce a test plan ready for QA sign-off", "Return a Review Report with a verdict" ensures actionable deliverables.
