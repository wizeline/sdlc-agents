# Skill: generating-unit-tests
> TestForge AI | Core Test Generation Skill

## What This Skill Does
Converts structured inputs (source code, requirements, API specs) into framework-native,
immediately runnable unit test suites using Arrange-Act-Assert structure.

---

## Usage
Apply this skill when:
- Writing unit tests for any function, method, or class
- Generating tests from a requirement or user story before code exists
- Producing tests for an API endpoint from an OpenAPI spec

---

## Step 1 — Understand the Unit
Read the provided source code or requirement carefully:
- What does this unit DO? (single responsibility check)
- What are its inputs and their types?
- What does it return or mutate?
- What can go wrong? (exceptions, error states)
- Does it call external systems? (DB, API, file, time, random)

---

## Step 2 — Build the Test Case Matrix
Before writing a single line of test code, map out every case in a table:

  Unit: calculate_discount(price: float, tier: str) -> float

  | # | Condition              | Input             | Expected     | Category  |
  |---|------------------------|-------------------|--------------|-----------|
  | 1 | Standard tier, valid   | 100.0, "standard" | 90.0         | Happy     |
  | 2 | Premium tier           | 100.0, "premium"  | 80.0         | Happy     |
  | 3 | Unknown tier           | 100.0, "vip"      | ValueError   | Error     |
  | 4 | Price is zero          | 0.0, "standard"   | 0.0          | Boundary  |
  | 5 | Price is negative      | -10.0, "standard" | ValueError   | Boundary  |
  | 6 | Price is None          | None, "standard"  | TypeError    | Boundary  |
  | 7 | Tier is None           | 100.0, None       | TypeError    | Boundary  |

---

## Step 3 — Write Each Test (AAA Pattern)

Python (pytest):

    def test_calculate_discount_when_standard_tier_expects_ten_percent_off():
        # ARRANGE
        price = 100.0
        tier = "standard"
        # ACT
        result = calculate_discount(price, tier)
        # ASSERT
        assert result == 90.0

JavaScript (Jest):

    test("returns 10% discount for standard tier", () => {
      // ARRANGE
      const price = 100.0;
      const tier = "standard";
      // ACT
      const result = calculateDiscount(price, tier);
      // ASSERT
      expect(result).toBe(90.0);
    });

Java (JUnit 5):

    @Test
    void calculateDiscount_whenStandardTier_returnsTenPercentOff() {
        double result = calculator.calculateDiscount(100.0, "standard");
        assertEquals(90.0, result, 0.001);
    }

---

## Step 4 — Mock External Dependencies

Python:

    @patch("module.external_api.call")
    def test_fn_when_api_success_expects_result(mock_api):
        mock_api.return_value = {"status": "ok", "data": 42}
        result = my_function()
        assert result == 42
        mock_api.assert_called_once()

JavaScript (Jest):

    jest.mock("./externalService");
    test("should return result when service succeeds", async () => {
      externalService.fetch.mockResolvedValue({ data: 42 });
      const result = await myFunction();
      expect(result).toBe(42);
      expect(externalService.fetch).toHaveBeenCalledTimes(1);
    });

---

## Step 5 — Parametrize for Multiple Cases

pytest:

    @pytest.mark.parametrize("price,tier,expected", [
        (100.0, "standard", 90.0),
        (100.0, "premium",  80.0),
        (200.0, "standard", 180.0),
    ])
    def test_calculate_discount_parametrized(price, tier, expected):
        assert calculate_discount(price, tier) == expected

Jest:

    test.each([
      [100.0, "standard", 90.0],
      [100.0, "premium",  80.0],
    ])("discount(%f, %s) should be %f", (price, tier, expected) => {
      expect(calculateDiscount(price, tier)).toBe(expected);
    });

---

## Step 6 — Exception Testing

pytest:

    def test_calculate_discount_when_negative_price_expects_value_error():
        with pytest.raises(ValueError, match="Price cannot be negative"):
            calculate_discount(-10.0, "standard")

JUnit 5:

    @Test
    void calculateDiscount_whenNegativePrice_throwsIllegalArgumentException() {
        assertThrows(IllegalArgumentException.class,
            () -> calculator.calculateDiscount(-10.0, "standard"));
    }

---

## Quality Checklist
Before emitting the final test file:
- [ ] Test matrix was built first (not ad-hoc)
- [ ] Every row in the matrix has a corresponding test
- [ ] All mocks have assertions (not just setup)
- [ ] Tests are fully isolated (no shared mutable state)
- [ ] Naming follows: test_unit_when_condition_expects_outcome
- [ ] File runs with a single command, zero additional config
