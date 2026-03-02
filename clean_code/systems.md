# Systems

---

## Definition

Systems should separate the construction of complex objects from their use. This separation is achieved through dependency injection, factories, and other creational patterns. Systems should be constructed carefully, not allowed to grow organically.

From "Clean Code" by Robert C. Martin: "Separating construction from use is a powerful technique. It allows you to understand the construction of an object or system separately from its runtime behavior."

---

## Plain Language Translation

> "How you build things should be separate from how you use them. Use factories and dependency injection to control creation."

**Analogia:** It's like a car factory. The factory (construction) is separate from the car being driven (use). You don't build a car while driving it.

---

## Why it matters

- **Separation of concerns** - Construction separate from use
- **Flexibility** - Easy to swap implementations
- **Testability** - Easy to mock dependencies
- **Maintainability** - Changes to construction don't affect use
- **Clarity** - Clear what creates what

---

## Key concepts

- **Dependency Injection** - Pass dependencies in
- **Factories** - Create objects for you
- **Construction vs Use** - Separate these concerns
- **Inversion of Control** - Framework controls creation

---

## Common violations

- Creating dependencies inside the classes that use them
- Using `new` in business logic
- No separation between construction and use
- Tight coupling to concrete classes

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Systems mean:
- **Dependency injection containers**
- **Factory patterns**
- **Builder patterns**
- **Separation of construction from use**

### In Procedural Programming

Systems mean:
- **Factory functions**
- **Configuration functions**
- **Clear initialization**

### In Functional Programming

Systems mean:
- **Dependency injection via function parameters**
- **Pure function composition**
- **No side effects in construction**

### Key Takeaway

> **The principle is the same: separate construction from use.**
>
> **The implementation differs based on the paradigm.**

---

## OOP Examples

### Java (OOP)

**Bad (creating inside)**
```java
public class OrderService {
    private Database db = new MySqlDatabase();  // Tight coupling!
    private EmailService email = new EmailService();  // Can't swap!
    
    public void saveOrder(Order order) {
        db.save(order);
    }
}
```

**Good (DI - injection from outside)**
```java
public class OrderService {
    private final OrderRepository repository;
    private final EmailService emailService;
    
    public OrderService(OrderRepository repository, EmailService emailService) {
        this.repository = repository;
        this.emailService = emailService;
    }
    
    public void saveOrder(Order order) {
        repository.save(order);
    }
}
```

### PHP (OOP)

**Bad (creating inside)**
```php
class OrderService {
    private $db;
    
    public function __construct() {
        $this->db = new MySqlDatabase();  // Tight coupling!
    }
}
```

**Good (dependency injection)**
```php
class OrderService {
    private $repository;
    
    public function __construct(OrderRepositoryInterface $repository) {
        $this->repository = $repository;
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Good (factory functions)**
```php
<?php
// Factory function - separates construction from use
function createDatabaseConnection(): PDO {
    return new PDO('mysql:host=localhost;dbname=test', 'user', 'pass');
}

function createUserService(PDO $db): UserService {
    return new UserService($db);
}

// Usage
$db = createDatabaseConnection();
$service = createUserService($db);
?>
```

---

## How to apply

### General Rules

1. **Don't use `new` in business logic** - Pass dependencies in
2. **Use factories** - For complex object creation
3. **Use dependency injection** - Pass dependencies in constructor
4. **Separate construction from use** - Keep these concerns separate

### Best Practices

- **Constructor injection** - Most common DI
- **Factory patterns** - For complex creation
- **DI containers** - For larger applications

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Flexibility | Easy to swap implementations |
| Testability | Easy to mock dependencies |
| Maintainability | Changes isolated |
| Clarity | Clear what creates what |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY system construction best practices within the existing paradigm**

### Context Matters

- If working on **procedural code**: use factory functions
- If working on **OOP code**: use DI and factories
- If working on **functional code**: pass dependencies as parameters

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Dependency Inversion:** Depend on abstractions
- **Factories:** Separate creation from use
- **DI:** Pass dependencies in

---

## Notes/Warnings

- **Don't use `new` in business logic** - Pass dependencies instead
- **Separate construction from use** - Keep concerns separate
- **Use DI containers for large apps** - Manage complexity