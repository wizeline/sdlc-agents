---
name: unit-test-analyzing-code-coverage
description: Detects coverage gaps in existing or newly generated test suites. Produces a prioritized list of untested code paths and recommends specific tests to close each gap.
---

## Usage
Apply this skill when:
- A test suite exists and you need to find what is NOT tested
- A coverage report (coverage.py, Istanbul, JaCoCo) is provided for analysis
- A developer asks "what am I missing?" after initial test generation
- Pre-sprint planning requires coverage gap estimation

---

## Coverage Types — Know All Four

| Type | What It Measures | Target |
|------|-----------------|--------|
| Line Coverage | Were these lines executed? | >= 80% |
| Branch Coverage | Were both sides of every if/else hit? | >= 75% |
| Function Coverage | Was every function called at least once? | >= 90% |
| Statement Coverage | Were all statements executed? | >= 80% |

Branch coverage is the most valuable — prioritize it.

### Coverage Targets in Practice
A passing coverage percentage does not guarantee quality. Apply these rules:
- **Critical paths** (authentication, payments, data integrity, error handling)
  must reach **80%+ line and branch coverage** — treat anything below as a HIGH gap
- **Supporting utilities** may have lower targets but must still cover error paths
- High coverage with weak assertions is worse than moderate coverage with strong ones
- Unit test coverage does not replace integration test coverage — note this distinction
  explicitly in every gap report when critical cross-service paths are involved

### TDD and Coverage
In a TDD workflow, coverage gaps indicate missing Red-Green cycles — not just missing tests.
Each uncovered branch represents a behavior that was never defined as a failing test first.
Flag this in reports: coverage gaps in a TDD project suggest incomplete feature development,
not just incomplete testing.

---

## Analysis Workflow

Read `references/index.md` before executing any step.

### Step 1 — Parse Existing Coverage Data
If a coverage report is provided (XML, JSON, HTML), extract:
- Overall coverage percentage per file
- Line numbers with 0 hits (never executed)
- Branch pairs where one side was never taken
- Functions with 0 calls

If no report exists, analyze source code structurally:
- Count all conditional branches (if, elif, else, switch cases, ternary)
- Count all exception handlers (try/except, try/catch)
- Count all public functions and methods
- Cross-reference against existing test file imports and call sites

### Step 2 — Classify Each Gap by Severity

| Severity | Criteria | Priority |
|----------|----------|----------|
| CRITICAL | Core business logic path untested | Fix immediately |
| HIGH | Error/exception handler untested | Fix this sprint |
| MEDIUM | Edge case or boundary value untested | Fix next sprint |
| LOW | Trivial getter/setter, logging line | Acceptable gap |

### Step 3 — Produce the Gap Report

Output format:

    ## Coverage Gap Report
    **Module:** payment_processor.py
    **Current Coverage:** 64% line | 51% branch
    **Target:** 80% line | 75% branch
    **Gap to Close:** 16% line | 24% branch

    ### Critical Gaps (must fix)
    | Location | Gap Type | Missing Test Description |
    |----------|----------|--------------------------|
    | line 87  | Branch   | else-branch of refund eligibility check never tested |
    | line 134 | Function | process_partial_payment() never called in any test |

    ### High Gaps (fix this sprint)
    | Location | Gap Type | Missing Test Description |
    |----------|----------|--------------------------|
    | line 201 | Exception| ValueError handler in parse_amount() not covered |

    ### Recommended New Tests
    1. test_process_partial_payment_when_valid_expects_success
    2. test_check_refund_eligibility_when_ineligible_expects_false
    3. test_parse_amount_when_invalid_string_expects_value_error

### Step 4 — Estimate Effort to Close Gap
For each recommended test, estimate:
- Complexity: LOW | MEDIUM | HIGH (based on mocking needs and logic depth)
- Estimated lines of test code: ~5-20 per simple test, ~30-60 per complex mock test
- Total tests needed to reach target coverage

---

## Coverage Tool Integration

### Python — coverage.py
    coverage run -m pytest
    coverage report --show-missing
    coverage json -o coverage.json    # for programmatic analysis

### JavaScript — Istanbul (nyc / c8)
    npx c8 --reporter=json npm test
    # Outputs: coverage/coverage-final.json

### Java — JaCoCo (Gradle)
    ./gradlew test jacocoTestReport
    # Outputs: build/reports/jacoco/test/jacocoTestReport.xml

### C# — coverlet
    dotnet test --collect:"XPlat Code Coverage"
    # Outputs: coverage.cobertura.xml

---

## Coverage Anti-Patterns to Flag
- Tests that only cover happy paths (missing negative/error branches)
- Parametrized tests that share a single code path (not truly multi-branch)
- Mocks that are too permissive (mock returns True for everything — false coverage)
- Integration tests counted as unit test coverage (different scope)

---

## Output Deliverables
1. coverage_gap_report.md — prioritized gap analysis
2. recommended_tests.md — specific test names and descriptions to write
3. coverage_delta_estimate.md — projected coverage % after recommended tests are added
