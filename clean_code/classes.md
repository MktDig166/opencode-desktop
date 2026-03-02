# Classes

---

## Definition

Classes should be small, with a single responsibility. The name should describe what the class does. Classes should have high cohesion - all methods and properties should be related. Classes should follow the Single Responsibility Principle (SRP).

From "Clean Code" by Robert C. Martin: "The first rule of classes is that they should be small. The second rule of classes is that they should be smaller than that."

---

## Plain Language Translation

> "Classes should be small, with one job. If a class does more than one thing, split it."

**Analogy:** It's like a restaurant team. The chef cooks, the waiter serves, the cashier handles money. Each has one job. If one person tried to do everything, nothing would get done well.

---

## Why it matters

- **Readability** - Easy to understand what the class does
- **Maintainability** - Changes are isolated
- **Testability** - Easier to test small classes
- **Reusability** - Small classes can be reused
- **Cohesion** - All elements are related

---

## Key concepts

- **Single Responsibility** - One reason to change
- **High cohesion** - All elements work together
- **Low coupling** - Minimal dependencies
- **Class organization** - Variables, constructors, public, private

---

## Common violations

- Classes that are 1000+ lines
- Classes with too many responsibilities
- Classes with too many methods
- Classes with too many dependencies
- Classes where not all members are related (low cohesion)

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Classes mean:
- **Templates for creating objects**
- **Single responsibility**
- **High cohesion, low coupling**

### In Procedural Programming

Classes do not apply:
- **Use functions and data structures**
- **No class-based organization**

### In Functional Programming

Classes do not apply:
- **Use functions and data structures**
- **No class-based organization**

### Key Takeaway

> **Classes are ONLY for OOP codebases.**
>
> **Do NOT apply class principles to procedural or functional code.**

---

## OOP Examples

### Java (OOP)

**Bad (large class)**
```java
// This class does EVERYTHING - bad!
class UserManager {
    // Database operations
    public void save() { ... }
    public void delete() { ... }
    public List<User> findAll() { ... }
    
    // Email operations
    public void sendWelcome() { ... }
    public void sendPasswordReset() { ... }
    
    // Validation
    public boolean validate() { ... }
    
    // Logging
    public void log() { ... }
    
    // 50 more methods...
}
```

**Good (small, focused classes)**
```java
class UserRepository {
    public void save(User user) { ... }
    public void delete(User user) { ... }
    public List<User> findAll() { ... }
}

class UserValidator {
    public boolean validate(User user) { ... }
}

class UserNotifier {
    public void sendWelcome(User user) { ... }
    public void sendPasswordReset(User user) { ... }
}
```

### PHP (OOP)

**Bad (large class)**
```php
class OrderService {
    public function create() { /* create */ }
    public function update() { /* update */ }
    public function delete() { /* delete */ }
    public function calculateTotal() { /* calculate */ }
    public function applyDiscount() { /* discount */ }
    public function sendEmail() { /* email */ }
    public function log() { /* logging */ }
    // 20 more methods...
}
```

**Good (small classes)**
```php
class OrderCreator {
    public function create(array $data): Order { /* ... */ }
}

class OrderUpdater {
    public function update(Order $order, array $data): Order { /* ... */ }
}

class OrderCalculator {
    public function calculateTotal(Order $order): float { /* ... */ }
}
```

---

## How to apply

### General Rules

1. **Classes should be small** - Under 200 lines
2. **Single responsibility** - One reason to change
3. **High cohesion** - All members are related
4. **Low coupling** - Few dependencies
5. **Descriptive names** - Name describes what it does

### Class Organization

- **Variables** - At top of class
- **Constructors** - After variables
- **Public methods** - After constructors
- **Private methods** - After public methods

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Readability | Easy to understand |
| Testability | Easy to test |
| Maintainability | Changes are isolated |
| Reusability | Small, focused |

---

## CRITICAL: OOP ONLY

> **⚠️ VERY IMPORTANT: This principle applies ONLY to Object-Oriented Programming (OOP) codebases.**

### Rules

- **DO NOT apply class principles to procedural code** - Use functions instead
- **DO NOT apply class principles to functional code** - Use functions instead
- **Classes are for OOP ONLY**

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Single Responsibility (SRP):** Classes have one responsibility
- **Cohesion:** All elements are related
- **Low Coupling:** Minimal dependencies

---

## Notes/Warnings

- **Classes are OOP only** - Don't use classes in procedural or functional code
- **Small is beautiful** - Smaller is better
- **One responsibility** - Each class does one thing