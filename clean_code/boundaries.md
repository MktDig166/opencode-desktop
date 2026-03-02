# Boundaries

---

## Definition

Boundaries refer to how code interacts with external systems, third-party libraries, and code outside your control. Clean boundaries mean clear interfaces, proper abstraction, and learning tests to verify external code behavior.

From "Clean Code" by Robert C. Martin: "Software systems separate the user from the enterprise domain. But we also have to divide the enterprise domain from other domains and from the infrastructure."

---

## Plain Language Translation

> "Keep your code separate from outside code. Use clear boundaries and don't let outside code leak into your system."

**Analogies:** It's like a country's border. Customs and immigration control what comes in and out. Your code should have the same protection.

---

## Why it matters

- **Isolation** - Changes to external code don't break your code
- **Flexibility** - Easy to swap external dependencies
- **Testability** - Can mock external dependencies
- **Maintainability** - Clean interfaces, clear responsibilities
- **Security** - Control what enters your system

---

## Key concepts

- **Third-party libraries** - External code you don't control
- **Learning tests** - Verify external code behavior
- **Adapters** - Translate between interfaces
- **Facades** - Simplify complex interfaces
- **Boundaries** - Where your code meets outside code

---

## Common violations

- Letting third-party code spread throughout your system
- Using raw external types in your domain
- Not testing external library behavior
- No abstraction between your code and external code
- Leaky abstractions - Exposing external details

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Boundaries mean:
- **Adapter pattern** - Wrap external code
- **Facade pattern** - Simplify complex interfaces
- **Interfaces** - Define boundaries

### In Procedural Programming

Boundaries mean:
- **Adapter functions** - Wrap external code
- **Clear function interfaces** - Define what passes through

### In Functional Programming

Boundaries mean:
- **Pure functions** - Keep external code separate
- **Type boundaries** - Define clear interfaces

### Key Takeaway

> **The principle is the same: keep external code separate from your code.**
>
> **The implementation differs based on the paradigm.**

---

## OOP Examples

### Java (OOP)

**Bad (leaking third-party code)**
```java
public class OrderService {
    public void saveOrder(Order order) {
        // Directly using third-party library!
        EntityManager em = Persistence.createEntityManagerFactory("pu").createEntityManager();
        em.persist(order);
    }
}
```

**Good (boundaries with interfaces)**
```java
public class OrderService {
    private final OrderRepository repository;
    
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }
    
    public void saveOrder(Order order) {
        repository.save(order);  // Doesn't know about JPA!
    }
}

interface OrderRepository {
    void save(Order order);
}
```

### PHP (OOP)

**Bad (leaking third-party code)**
```php
class OrderService {
    public function create(array $data) {
        // Directly using Eloquent everywhere!
        $order = new Order();
        $order->fill($data);
        $order->save();
    }
}
```

**Good (boundaries)**
```php
class OrderService {
    private $repository;
    
    public function __construct(OrderRepositoryInterface $repository) {
        $this->repository = $repository;
    }
    
    public function create(array $data): Order {
        $order = new Order();
        $order->fill($data);
        
        return $this->repository->save($order);
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Good (boundary functions)**
```php
<?php
// Boundary function wraps third-party code
function saveUserToDatabase(array $data): int {
    // Third-party code is isolated here
    $id = DB::table('users')->insertGetId($data);
    return $id;
}

function createUser(array $data): int {
    validateUserData($data);
    return saveUserToDatabase($data);
}
?>
```

---

## How to apply

### General Rules

1. **Wrap third-party code** - Don't let it spread
2. **Use learning tests** - Verify external behavior
3. **Define boundaries** - Clear interfaces
4. **Keep external code isolated** - Don't leak it into your domain

### Best Practices

- **Learning tests** - Test external libraries before using
- **Adapters** - Wrap external code
- **Facades** - Simplify complex interfaces
- **Interfaces** - Define clear contracts

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Flexibility | Swap dependencies easily |
| Testability | Mock external dependencies |
| Maintainability | Changes isolated |
| Security | Control what enters |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY boundary best practices within the existing paradigm**

### Context Matters

- If working on **procedural code**: use boundary functions
- If working on **OOP code**: use adapter/facade patterns
- If working on **functional code**: keep pure functions separate

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Dependency Inversion:** Depend on abstractions
- **Adapter Pattern:** Wrap external code
- **Separation of Concerns:** Keep boundaries clean

---

## Notes/Warnings

- **Wrap third-party code** - Don't let it leak
- **Learning tests** - Verify behavior
- **Keep boundaries clear** - Define interfaces