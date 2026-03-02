# Small Functions

---

## Definition

Functions should be small. They should do one thing and do it well. The ideal function is 20 lines or less, with one level of indentation, and a clear, descriptive name that tells exactly what it does.

From "Clean Code" by Robert C. Martin: "The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that."

---

## Plain Language Translation

> "Functions should do one thing, do it well, and do it in few lines."

**Analogy:** It's like a restaurant kitchen. Each station has a specific job: grill, sauté, salad, dessert. No station tries to do everything. This makes the kitchen fast, organized, and easy to manage.

---

## Why it matters

- **Readability** - Each function is easy to read and understand
- **Testability** - Small functions are easy to test
- **Maintainability** - Bugs are easier to find and fix
- **Reusability** - Small functions can be reused
- **Reduced complexity** - Less nesting, less logic
- **Self-documenting** - The name explains what it does

---

## Key concepts

- **Single responsibility** - A function does one thing
- **One level of indentation** - Keep functions flat
- **Descriptive names** - The name tells what it does
- **Few parameters** - 0-3 parameters is ideal
- **No side effects** - The function does what its name says
- **Extract till you drop** - If a function does more than one thing, split it
- **Switch statements** - Use polymorphism to keep functions small

---

## Common violations

- Functions with 100+ lines
- Functions with multiple levels of indentation
- Functions with 10+ parameters
- Functions with "and" in the name (validateAndSave, calculateAndFormat)
- Functions that do too many things
- Duplicate code across large functions
- Nested conditionals within functions

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Small functions mean:
- **Methods** are focused on one responsibility
- **Private methods** handle sub-tasks
- **Command-Query Separation** - A method either does something or returns something, not both
- **Single Responsibility** - Each method has one reason to change

### In Procedural Programming

Small functions mean:
- **Functions** do one thing
- **Pure functions** have no side effects
- **Functions call other functions** to accomplish tasks
- **Parameters** are explicit and minimal

### In Functional Programming

Small functions mean:
- **Pure functions** - Same input, same output
- **Single transformation** - Each function transforms one thing
- **Function composition** - Chain simple functions together
- **No mutable state** - Data flows through functions

### Key Takeaway

> **The principle is the same: functions should do one thing.**
>
> **The implementation differs based on the paradigm:**
> - OOP: methods with single responsibility in classes
> - Procedural: functions that do one thing
> - Functional: pure functions that transform data

---

## OOP Examples

### Java (OOP)

**Bad (large, complex function)**
```java
public void processUserOrder(User user, Order order) {
    // 50 lines of code doing many things:
    // validation, calculation, database, email, logging
    if (user == null) {
        // validation
    }
    // calculate
    double total = 0;
    for (Item item : order.getItems()) {
        total += item.getPrice() * item.getQuantity();
    }
    // discount
    if (total > 100) {
        total = total * 0.9;
    }
    // save to database
    // send email
    // log
}
```

**Good (small, focused functions)**
```java
public void processUserOrder(User user, Order order) {
    validateUser(user);
    validateOrder(order);
    double total = calculateTotal(order);
    double discount = applyDiscount(total);
    saveOrder(order, discount);
    sendConfirmationEmail(user, order);
    logOrderProcessing(user, order);
}

private void validateUser(User user) { /* validation */ }
private void validateOrder(Order order) { /* validation */ }
private double calculateTotal(Order order) { /* calculation */ }
private double applyDiscount(double total) { /* discount logic */ }
private void saveOrder(Order order, double discount) { /* persistence */ }
private void sendConfirmationEmail(User user, Order order) { /* email */ }
private void logOrderProcessing(User user, Order order) { /* logging */ }
```

### PHP (OOP)

**Bad (large function)**
```php
class OrderService {
    public function createOrder($data) {
        // 50+ lines doing everything
    }
}
```

**Good (small, focused methods)**
```php
class OrderService {
    public function createOrder(array $data): Order {
        $this->validateData($data);
        $order = $this->buildOrder($data);
        $order = $this->applyDiscount($order);
        $this->saveOrder($order);
        $this->sendNotification($order);
        
        return $order;
    }
    
    private function validateData(array $data): void { /* ... */ }
    private function buildOrder(array $data): Order { /* ... */ }
    private function applyDiscount(Order $order): Order { /* ... */ }
    private function saveOrder(Order $order): void { /* ... */ }
    private function sendNotification(Order $order): void { /* ... */ }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (large function)**
```php
<?php
function createUserAndSendWelcomeEmail($data) {
    // 50+ lines of validation, database, email, logging
}
?>
```

**Good (small functions)**
```php
<?php
function createUserAndSendWelcomeEmail(array $data): int {
    $validated = validateUserData($data);
    $userId = saveUserToDatabase($validated);
    sendWelcomeEmail($validated['email']);
    logUserCreation($userId);
    
    return $userId;
}

function validateUserData(array $data): array { /* ... */ }
function saveUserToDatabase(array $data): int { /* ... */ }
function sendWelcomeEmail(string $email): void { /* ... */ }
function logUserCreation(int $userId): void { /* ... */ }
?>
```

---

## How to apply

### General Steps

1. **Start small** - Write small functions from the beginning
2. **Extract often** - If a function grows beyond 20 lines, split it
3. **One level of indentation** - If you need more than one level, extract a function
4. **Descriptive names** - The name should tell what it does
5. **Few parameters** - 0-3 is ideal; use objects if you need more
6. **No side effects** - The function name should describe everything it does

### Tips for Small Functions

- **Use the "extract method" refactoring** - Split large functions into smaller ones
- **Apply the "single responsibility" principle** - Each function does one thing
- **Avoid "and" in function names** - If you have "and", it's doing two things
- **Prefer many small functions** - Over a few large functions
- **Name by what it does, not how it does it** - calculateTotal, not calculateUsingAlgorithm

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Easy to read | Each function fits on one screen |
| Easy to test | One thing to test per function |
| Easy to debug | Clear where the problem is |
| Reusable | Small functions can be reused |
| Maintainable | Changes are isolated |
| Self-documenting | Names explain functionality |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY small functions within the existing paradigm**

### Context Matters

- If working on **procedural code**: keep functions small and focused
- If working on **OOP code**: keep methods small and focused
- If working on **functional code**: keep functions pure and focused

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Single Responsibility (SRP):** Functions should have one reason to change
- **Meaningful Names:** Small functions need good names
- **DRY:** Extract repeated logic into small functions
- **KISS:** Small functions are simpler to understand

---

## Notes/Warnings

- **20 lines is a guideline, not a rule** - The key is doing one thing
- **Don't over-extract** - Too many tiny functions can be hard to follow
- **Balance is key** - A function should be small but also meaningful
- **Consistency matters** - Apply consistently across the codebase