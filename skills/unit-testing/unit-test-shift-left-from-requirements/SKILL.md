# Skill: unit-test-shift-left-from-requirements
> TestForge AI | Orchestration Skill

## What This Skill Does
Generates unit tests and BDD scenarios directly from requirements, user stories,
or PRDs — before any implementation code exists. Enables true shift-left testing
by making tests the first artifact produced for a feature, not the last.

---

## When to Apply This Skill
- A user story, acceptance criteria, or PRD is provided with no accompanying code
- A team wants tests ready before the developer starts implementation
- BDD / Gherkin scenarios are needed to align QA, dev, and product on expected behavior
- A feature's testability needs to be validated before implementation begins

### TDD Framing
This skill operationalizes the **Red phase** of the Red-Green-Refactor cycle at scale.
Every test skeleton produced here is a failing test by design — it cannot pass until
the developer writes the implementation. Outputs from this skill should be committed
to the repository before any implementation code is written, making the failing tests
the formal definition of done for the feature.

---

## Atomic Skills to Load First
Read these files before executing any step:

1. `../unit-test-generating-unit-tests/SKILL.md`
   — Test matrix methodology and AAA pattern applied in spec-driven mode:
     derive test cases from acceptance criteria rather than from code paths

2. `../unit-test-authoring-test-plans/SKILL.md`
   — BDD / Gherkin scenario format (Feature / Scenario / Given / When / Then)
     used to produce human-readable scenario documents alongside test skeletons

---

## Execution Steps

Read `references/index.md` before executing any step.

### Step 1 — Parse Requirements for Testable Criteria
Read the provided input (user story, PRD, acceptance criteria) and extract:

  - Explicit acceptance criteria ("system shall...", "given... when... then...")
  - Implicit behavioral rules buried in prose descriptions
  - Edge cases and error conditions mentioned in requirements
  - Out-of-scope statements (note these — do not generate tests for them)

Produce a numbered list of testable statements before writing any test code.

### Step 2 — Build the Test Case Matrix
For each testable statement, define:

  | # | Requirement Source | Condition         | Expected Behavior  | Category |
  |---|--------------------|-------------------|--------------------|----------|
  | 1 | AC-3               | Valid user logs in | JWT token returned | Happy    |
  | 2 | AC-3               | Wrong password     | 401 returned       | Error    |

Use the test matrix approach from `../unit-test-generating-unit-tests/SKILL.md`.

### Step 3 — Generate Test Skeletons
Produce test skeletons with:
  - Correct function/method names following the naming convention
  - ARRANGE / ACT / ASSERT sections stubbed out
  - TODO comments where implementation-specific values are not yet known
  - Placeholder mocks only for true externalities (DB, API, file, clock) — not internal logic
  - Fixed seeds for any randomness; mocked time for any date/time assertions
  - One behavior per test — if a requirement yields three behaviors, write three skeletons

Each skeleton must be written so it will **fail** when run against an empty implementation.
If a skeleton could accidentally pass without any code written, it is too weak — strengthen the assertion.

Example (pytest):

    def test_login_when_valid_credentials_expects_jwt_token():
        # ARRANGE
        username = "alice"
        password = "valid_password"  # TODO: align with auth service contract
        # ACT
        result = login(username, password)  # TODO: import once implemented
        # ASSERT
        assert result.token is not None      # will fail until implemented
        assert result.status_code == 200

### Step 4 — Produce BDD / Gherkin Scenarios
For each test case, produce a corresponding Gherkin scenario following
the format in `../unit-test-authoring-test-plans/SKILL.md`:

    Feature: User Login

      Scenario: Successful login with valid credentials
        Given a registered user with username "alice"
        When the user submits correct credentials
        Then the system returns a JWT token
        And the response status is 200

      Scenario: Login fails with incorrect password
        Given a registered user with username "alice"
        When the user submits an incorrect password
        Then the response status is 401
        And the body contains "Invalid credentials"

### Step 5 — Flag Ambiguities
Before delivering output, list any requirements that were:
  - Too vague to produce a test for (request clarification)
  - Contradictory with other requirements (flag for product review)
  - Missing expected values or thresholds (mark with TODO)

---

## Output Deliverables
```
test_<feature>_skeleton.<ext>     <- test skeletons ready to fill in once code is written
gherkin_scenarios_<feature>.md   <- BDD scenarios for QA and product alignment
requirements_coverage_map.md     <- maps each requirement to one or more test IDs
ambiguities.md                   <- list of unclear or incomplete requirements
```
