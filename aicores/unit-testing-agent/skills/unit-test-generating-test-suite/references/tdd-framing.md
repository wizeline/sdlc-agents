# Reference: TDD Framing
> TestForge AI | Canonical Reference Document

Skills and agents in this project cite this document as the authoritative source
for Test-Driven Development principles. Read this before applying any TestForge skill.

---

## What TDD Is
Test-Driven Development is a software development discipline in which tests are written
before implementation code. The test defines the desired behavior — the implementation
is then written solely to make that test pass. This reverses the traditional order and
produces code that is provably correct by construction.

TDD is not a testing technique. It is a design and development discipline that uses
tests as the medium for specifying behavior.

---

## The Red-Green-Refactor Cycle

Every unit of work in TDD follows this three-step cycle, repeated continuously:

### 🔴 Red — Write a Failing Test First
Write a test that describes the next piece of desired behavior.
Run it. It must fail — because no implementation exists yet.
A test that passes before any code is written is not a valid Red phase test.

Rules for the Red phase:
- The test must be specific enough to fail on a missing or wrong implementation
- The test must describe behavior from the caller's perspective, not internal mechanics
- The test name must communicate what is expected, not how it is implemented
- If the test could accidentally pass without code, strengthen the assertion

### 🟢 Green — Write the Minimum Code to Pass
Write only enough implementation to make the failing test pass.
Do not optimize. Do not generalize. Do not add untested behavior.
The goal is a passing test — nothing more.

Rules for the Green phase:
- Resist adding logic not required by the current test
- A hardcoded return value that makes the test pass is valid at this stage
- Move to Refactor only after the test is green

### 🔵 Refactor — Improve Without Changing Behavior
Clean up the implementation and the test code.
Remove duplication. Improve naming. Simplify logic.
Run all tests after every change. If any test breaks, the refactor introduced a regression.

Rules for the Refactor phase:
- All tests must remain green throughout refactoring
- Refactoring the internals must not require changing any test
- If a test breaks during refactoring, the test was testing implementation — fix the test
- Test code is refactored with the same care as production code

Then repeat — write the next failing test.

---

## Why This Order Matters

| Writing tests after code | Writing tests before code (TDD) |
|--------------------------|----------------------------------|
| Tests describe what code does | Tests describe what code should do |
| Easy to write tests that always pass | Tests must be strong enough to fail |
| Coverage is an afterthought | Coverage is a byproduct of design |
| Refactoring is risky | Refactoring is safe — tests catch regressions |
| Tests couple to implementation | Tests couple to behavior and contracts |

---

## Shift-Left and TDD
Shift-left testing means moving testing earlier in the development lifecycle.
TDD takes this to its logical extreme: tests are the first artifact, written before
any implementation exists. In a shift-left TDD workflow:

1. Requirements → test skeletons (Red phase at feature level)
2. Test skeletons → implementation (Green phase)
3. Implementation → refactored code + passing tests (Refactor phase)
4. Tests are committed before implementation code in version control

This makes the test suite the living specification of the system.

---

## TDD and Coverage
In a TDD project, code coverage is a byproduct of the Red-Green-Refactor cycle —
not a target to chase independently. Each cycle produces one more tested behavior.
A coverage gap in a TDD project means a behavior was implemented without a prior
failing test — a violation of the TDD discipline, not just a testing oversight.

Target coverage floors for TestForge AI output:
- Line coverage: >= 80%
- Branch coverage: >= 75% (prioritized — every if/else must be exercised)
- Function coverage: >= 90%
- Critical paths (auth, payments, data integrity, error handling): >= 80% line + branch

---

## TDD Anti-Patterns to Avoid

| Anti-Pattern | Description | Correct Approach |
|---|---|---|
| Test After | Writing tests after implementation — tests describe what code does, not what it should | Write the test first, then implement |
| Tautological Tests | Tests that always pass because assertions are too weak | Assert specific expected values, not just that something is not null |
| Implementation Tests | Tests that break when internals are refactored but behavior is unchanged | Test public contracts and observable outputs, not private methods |
| Over-Specification | Mocks that assert exact internal call sequences irrelevant to behavior | Assert only what the caller cares about |
| Green Without Red | Skipping the failing test step and writing code first | Always run the test before the implementation exists — confirm it fails |
| Big Bang TDD | Writing many tests at once before implementing any | One test, one implementation cycle — keep cycles short |

---

## Key Terms
| Term | Definition |
|------|-----------|
| Unit | The smallest testable piece of behavior — typically one function or method |
| Red phase | The state where a test is written and failing |
| Green phase | The state where minimal code makes the test pass |
| Refactor phase | Improving code quality without changing behavior or breaking tests |
| Shift-left | Moving testing earlier in the development lifecycle |
| Test contract | The public interface and observable behavior a test is written against |
| Behavioral test | A test that survives refactoring because it tests outputs, not internals |
