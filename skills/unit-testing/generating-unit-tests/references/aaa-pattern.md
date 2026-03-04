# Reference: AAA Pattern — Arrange, Act, Assert
> TestForge AI | Canonical Reference Document

Skills and agents in this project cite this document as the authoritative source
for test structure. Every unit test generated or reviewed by TestForge AI must
follow this pattern.

---

## What the AAA Pattern Is
Arrange-Act-Assert (AAA) is a structural pattern for writing unit tests that separates
each test into three clearly defined phases. It makes tests easier to read, debug,
and maintain by ensuring every test has exactly one purpose and one point of failure.

AAA is the implementation-level complement to TDD's Red-Green-Refactor cycle:
- **Red phase** defines what the test must verify (the Assert)
- **Green phase** produces the code that satisfies it (the Act)
- **Refactor phase** cleans up without touching the test structure

---

## The Three Phases

### ARRANGE — Set Up the World
Prepare everything the unit needs to run: inputs, dependencies, mocks, and fixtures.
The Arrange section should contain no logic — only setup.

What belongs here:
- Input values and test data
- Mock and stub configuration (return values, side effects)
- Object instantiation and state setup
- Fixture loading

What does NOT belong here:
- Assertions
- Calls to the unit under test
- Logic or conditionals

### ACT — Execute the Unit
Call the unit under test exactly once. Capture its output.
The Act section should be a single line in the vast majority of cases.

What belongs here:
- The single function or method call being tested
- Capturing the return value or observable side effect

What does NOT belong here:
- Multiple calls to the unit (split into separate tests)
- Setup logic (move to Arrange)
- Assertions (move to Assert)

### ASSERT — Verify the Outcome
Verify that the unit produced the expected result.
Assert against the return value, a state change, or a side effect — never all three
in a single test unless they are inseparably part of one behavior.

What belongs here:
- Assertions on return values
- Assertions on observable state changes
- Assertions on mock calls (confirming a dependency was called correctly)
- Exception assertions (when error behavior is the behavior under test)

What does NOT belong here:
- Additional Act calls
- Setup or teardown logic

---

## Visual Structure

Every test must visually separate the three phases with blank lines or section comments:

Python (pytest):

    def test_calculate_discount_when_premium_tier_expects_twenty_percent_off():
        # ARRANGE
        price = 100.0
        tier = "premium"

        # ACT
        result = calculate_discount(price, tier)

        # ASSERT
        assert result == 80.0

JavaScript (Jest):

    test('returns 20% discount for premium tier', () => {
      // ARRANGE
      const price = 100.0;
      const tier = 'premium';

      // ACT
      const result = calculateDiscount(price, tier);

      // ASSERT
      expect(result).toBe(80.0);
    });

Java (JUnit 5):

    @Test
    void calculateDiscount_whenPremiumTier_returnsTwentyPercentOff() {
        // Arrange
        double price = 100.0;
        String tier = "premium";

        // Act
        double result = calculator.calculateDiscount(price, tier);

        // Assert
        assertEquals(80.0, result, 0.001);
    }

C# (xUnit):

    [Fact]
    public void CalculateDiscount_WhenPremiumTier_ReturnsTwentyPercentOff()
    {
        // Arrange
        double price = 100.0;
        string tier = "premium";

        // Act
        double result = _calculator.CalculateDiscount(price, tier);

        // Assert
        Assert.Equal(80.0, result);
    }

---

## AAA With Mocks

When the unit under test calls an external dependency, the mock setup belongs in
Arrange, and the mock assertion belongs in Assert:

Python:

    def test_send_welcome_email_when_user_created_expects_email_sent():
        # ARRANGE
        mock_mailer = MagicMock()
        service = UserService(mailer=mock_mailer)
        user = User(email="alice@example.com", name="Alice")

        # ACT
        service.create_user(user)

        # ASSERT
        mock_mailer.send.assert_called_once_with(
            to="alice@example.com",
            template="welcome"
        )

JavaScript (Jest):

    test('sends welcome email when user is created', async () => {
      // ARRANGE
      const mockMailer = { send: jest.fn().mockResolvedValue(true) };
      const service = new UserService(mockMailer);
      const user = { email: 'alice@example.com', name: 'Alice' };

      // ACT
      await service.createUser(user);

      // ASSERT
      expect(mockMailer.send).toHaveBeenCalledWith({
        to: 'alice@example.com',
        template: 'welcome'
      });
    });

---

## AAA for Exception Testing

When the expected behavior is an exception, the Act and Assert merge into a single
assertion block — this is the one accepted exception to the single-line Act rule:

Python:

    def test_calculate_discount_when_negative_price_expects_value_error():
        # ARRANGE
        price = -10.0
        tier = "standard"

        # ACT + ASSERT
        with pytest.raises(ValueError, match="Price cannot be negative"):
            calculate_discount(price, tier)

Jest:

    test('throws when price is negative', () => {
      // ARRANGE
      const price = -10.0;
      const tier = 'standard';

      // ACT + ASSERT
      expect(() => calculateDiscount(price, tier))
        .toThrow('Price cannot be negative');
    });

---

## AAA and Teardown

Some tests require cleanup after the Assert phase. Add a Teardown section when:
- A file was written to disk during the test
- A database record was inserted (even in a test DB)
- A global or module-level state was mutated

    def test_export_report_when_valid_data_expects_file_created():
        # ARRANGE
        output_path = "/tmp/test_report.csv"
        reporter = ReportExporter()

        # ACT
        reporter.export(data=sample_data, path=output_path)

        # ASSERT
        assert os.path.exists(output_path)
        with open(output_path) as f:
            assert "header" in f.read()

        # TEARDOWN
        os.remove(output_path)

Prefer using framework fixtures (pytest fixtures, JUnit @AfterEach, Jest afterEach)
over manual teardown inline — they run even when the test fails.

---

## One Assert Per Test — The Rule and Its Nuance

**The rule:** each test should verify one behavior.
**The nuance:** one behavior may require multiple assertion lines.

Acceptable — multiple assertions on the same behavior:

    # ASSERT — verifying a single "user was created" behavior
    assert response.status_code == 201
    assert response.body["id"] is not None
    assert response.body["email"] == "alice@example.com"

Not acceptable — two different behaviors in one test:

    # BAD — two behaviors, should be two tests
    assert response.status_code == 201       # behavior: creation succeeds
    assert welcome_email_sent == True        # behavior: email is triggered

When in doubt: if the test name cannot describe all assertions in one phrase,
the test covers more than one behavior — split it.

---

## AAA Violations to Flag in Review

| Violation | Example | Fix |
|-----------|---------|-----|
| No Arrange | Test calls unit with hardcoded literals, no setup | Extract setup into named variables |
| Multiple Acts | Two function calls in the Act section | Split into two tests |
| Assert in Arrange | Checking mock state before the unit runs | Move to Assert |
| No Assert | Test calls unit but checks nothing | Add explicit assertion |
| Arrange in Assert | Creating objects during assertion | Move to Arrange |
| Mixed behaviors | Assert covers two unrelated outcomes | Split into two tests |
| Missing mock assertion | Mock set up but never asserted on | Add assert_called_once or equivalent |
