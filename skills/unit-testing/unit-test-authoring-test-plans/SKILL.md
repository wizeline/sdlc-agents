---
name: unit-test-authoring-test-plans
description: Produces formal test plan documents in Markdown or .docx format from test suites, requirements, and coverage data. Suitable for team handoffs, audit trails, QA sign-off, and sprint documentation.
---

Read `references/index.md` before executing any step.

---

## Usage
Apply this skill when:
- A formal test plan document is needed for a sprint, feature, or module
- Stakeholders require a human-readable summary of testing strategy
- QA team needs a structured sign-off document
- An audit or compliance process requires test documentation
- A developer wants a Markdown test plan alongside their test file

---

## Test Plan Document Structure

Every test plan must include these sections:

### 1. Cover Page / Header
    Test Plan: <Feature or Module Name>
    Version: 1.0
    Date: <date>
    Author: TestForge AI (test-unit-gen-agent)
    Status: Draft | Review | Approved

### 2. Scope
- What is being tested (module, feature, user story reference)
- What is explicitly OUT of scope (integration tests, performance tests, etc.)
- Target environments (local dev, CI, staging)

### 3. Test Objectives
Written as measurable goals:
- Achieve >= 80% line coverage for <module>
- Verify all acceptance criteria in user story US-042
- Validate error handling for all invalid input types

### 4. Test Strategy

| Approach | Description |
|----------|-------------|
| Unit Testing | Isolated function/method testing with mocked dependencies |
| Framework | <pytest / Jest / JUnit 5 / xUnit> |
| Coverage Tool | <coverage.py / Istanbul / JaCoCo> |
| Execution | <manual / CI via GitHub Actions / Jenkins> |
| BDD Style | <Yes/No> — Gherkin scenarios used for requirements-driven tests |

### 5. Test Cases Summary Table
    | ID     | Test Name                                         | Category  | Priority | Status  |
    |--------|---------------------------------------------------|-----------|----------|---------|
    | TC-001 | test_login_when_valid_credentials_expects_token   | Happy     | HIGH     | Planned |
    | TC-002 | test_login_when_invalid_password_expects_401      | Error     | HIGH     | Planned |
    | TC-003 | test_login_when_account_locked_expects_403        | Error     | HIGH     | Planned |
    | TC-004 | test_login_when_empty_username_expects_bad_request| Boundary  | MEDIUM   | Planned |

### 6. Coverage Targets

| Metric | Target | Current | Gap |
|--------|--------|---------|-----|
| Line Coverage | 80% | 64% | 16% |
| Branch Coverage | 75% | 51% | 24% |
| Function Coverage | 90% | 78% | 12% |

### 7. Dependencies & Risks

| Dependency | Risk | Mitigation |
|------------|------|------------|
| External payment API | Tests may fail if mock schema changes | Pin mock responses to spec version |
| Database schema | Schema changes break fixture data | Use factory-based fixtures, not hard-coded SQL |

### 8. Entry & Exit Criteria

Entry Criteria (tests can begin):
- Source code or requirements are available
- Test environment is configured
- All dependencies are mockable

Exit Criteria (testing is complete):
- All HIGH priority test cases pass
- Coverage targets are met
- Test review agent verdict: APPROVED or APPROVED_WITH_NOTES
- No CRITICAL or HIGH severity defects outstanding

### 9. Defect Management
- Defects found during test execution logged in: <Jira / GitHub Issues / Linear>
- Severity levels: CRITICAL | HIGH | MEDIUM | LOW
- Blocking defects must be resolved before CI gate passes

### 10. Approvals
    | Role         | Name | Signature | Date |
    |--------------|------|-----------|------|
    | Author       |      |           |      |
    | Tech Lead    |      |           |      |
    | QA Lead      |      |           |      |

---

## BDD / Gherkin Test Cases (when requirements-driven)
When the input includes user stories or acceptance criteria, supplement the test plan
with Gherkin scenarios:

    Feature: User Login

      Scenario: Successful login with valid credentials
        Given a registered user with username "alice" and password "secret"
        When the user submits the login form
        Then the system returns a JWT token
        And the response status is 200

      Scenario: Login fails with wrong password
        Given a registered user with username "alice"
        When the user submits login with password "wrong"
        Then the response status is 401
        And the error message is "Invalid credentials"

---

## Output Formats

### Markdown (.md)
- Use this for developer-facing documentation
- Stored alongside test files in the repository
- Rendered in GitHub/GitLab automatically

### Word Document (.docx)
- Use this for formal QA sign-off, audits, or stakeholder review
- Use the docx skill to generate the .docx file from the Markdown content
- Formatting: professional heading styles, page numbers, company letterhead

---

## Quality Checklist
Before delivering the test plan:
- [ ] All test cases from the generated test suite appear in Section 5
- [ ] Coverage targets are specific and measurable (not just "high")
- [ ] Entry/exit criteria are unambiguous
- [ ] Test case IDs are unique and traceable to requirements where possible
- [ ] Document version and date are current
- [ ] Status column reflects actual state (Planned / In Progress / Pass / Fail)
