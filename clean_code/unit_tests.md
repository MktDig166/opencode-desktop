# Unit Tests

---

## Definition

Unit tests are automated tests that verify the smallest pieces of code work correctly. They are the foundation of test-driven development and essential for maintaining clean, bug-free code. Tests should be clean, readable, and focused.

From "Clean Code" by Robert C. Martin: "Tests are as important to the health of a project as the production code itself."

---

## Plain Language Translation

> "Test your code. Tests prove your code works and keep it working."

**Analogy:** It's like a car's crash test. Before releasing a car, they test every part. Software should be tested the same way.

---

## Why it matters

- **Confidence** - Know your code works
- **Regression prevention** - Catch bugs early
- **Documentation** - Tests show how code works
- **Refactoring safety** - Change code without fear
- **Design quality** - Testable code is better code

---

## Key concepts

- **Three laws of TDD** - Red, Green, Refactor
- **F.I.R.S.T. principles** - Fast, Independent, Repeatable, Self-validating, Timely
- **Arrange-Act-Assert** - Test structure
- **One assert per test** - Focus on one thing
- **Test doubles** - Mocks, stubs, fakes

---

## Common violations

- No tests for critical code
- Tests that are hard to read
- Tests that test multiple things
- Tests that depend on each other
- Slow tests that slow down development
- Tests that don't actually test anything
- No test coverage for edge cases

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Unit tests mean:
- **Testing classes and methods**
- **Mocking dependencies**
- **Testing behavior**

### In Procedural Programming

Unit tests mean:
- **Testing functions**
- **Testing output based on input**
- **Pure function testing**

### In Functional Programming

Unit tests mean:
- **Testing pure functions**
- **Property-based testing**
- **Testing transformations**

### Key Takeaway

> **The principle is the same: test your code.**
>
> **The implementation differs based on the paradigm.**

---

## OOP Examples

### Java (OOP)

**Good unit test**
```java
@Test
void testCalculateDiscount_appliesDiscount_whenTotalExceeds100() {
    // Arrange
    OrderCalculator calculator = new OrderCalculator(0.1); // 10% discount
    
    // Act
    double result = calculator.calculateDiscount(150.0);
    
    // Assert
    assertEquals(15.0, result, 0.01);
}
```

### PHP (PHPUnit)

**Good unit test**
```php
class OrderCalculatorTest extends TestCase
{
    public function testCalculateDiscountAppliesDiscountWhenTotalExceeds100(): void
    {
        // Arrange
        $calculator = new OrderCalculator(0.10);
        
        // Act
        $result = $calculator->calculateDiscount(150.0);
        
        // Assert
        $this->assertEquals(15.0, $result, 0.01);
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Good unit test**
```php
<?php
use PHPUnit\Framework\TestCase;

class CalculateTotalTest extends TestCase
{
    public function testCalculateTotalReturnsCorrectSum(): void
    {
        $items = [
            ['price' => 10.00, 'quantity' => 2],
            ['price' => 5.00, 'quantity' => 3]
        ];
        
        $result = calculateTotal($items);
        
        $this->assertEquals(35.00, $result);
    }
}
?>
```

---

## How to apply

### General Rules

1. **Follow the three laws of TDD** - Red, Green, Refactor
2. **Write fast tests** - Tests should run quickly
3. **Tests should be independent** - No dependencies between tests
4. **Use descriptive names** - Test names should explain what they test
5. **One assert per test** - Focus on one behavior
6. **Test behavior, not implementation** - Don't test how, test what

### Best Practices

- **Arrange-Act-Assert** - Clear test structure
- **F.I.R.S.T. principles** - Fast, Independent, Repeatable, Self-validating, Timely
- **Test doubles** - Use mocks for dependencies
- **Boundary tests** - Test edge cases

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Confidence | Know code works |
| Safety | Refactor without fear |
| Documentation | Tests show behavior |
| Quality | Better design |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY testing best practices within the existing paradigm**

### Context Matters

- If working on **procedural code**: test functions
- If working on **OOP code**: test classes
- If working on **functional code**: test pure functions

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Clean Code:** Tests are part of clean code
- **TDD:** Test-driven development
- **F.I.R.S.T.:** Test quality principles

---

## Notes/Warnings

- **Write tests first** - Follow TDD
- **Keep tests clean** - As important as production code
- **Test behavior** - Not implementation details