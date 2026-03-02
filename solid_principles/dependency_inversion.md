**5. [D] Dependency Inversion Principle (DIP)**

---

## Definition

1. High-level modules should **not depend** on low-level modules. Both should depend on **abstractions**.
2. Abstractions should **not depend** on details. Details should **depend** on abstractions.

---

## Plain Language Translation

> "Don't depend on details. Program to interfaces, not implementations. The 'what' matters more than the 'how'."

**Analogy:** It's like an electrical outlet. You don't buy "fridge outlet" or "TV outlet". The outlet is an abstraction. The appliance (fridge, TV) depends on the outlet, not the other way around.

---

## Why it matters

- **Reduces coupling** - high-level code doesn't know about low-level details
- **Improves testability** - easy to swap real implementations with mocks
- **Increases flexibility** - swap implementations without changing high-level code
- **Enables reusability** - high-level code works with any implementation
- **Supports change** - infrastructure changes don't break business logic

---

## Key concepts

- **High-level modules** - business logic, rules, policies
- **Low-level modules** - infrastructure, DB, I/O, external services
- **Abstractions** - interfaces, abstract classes
- **Details** - concrete implementations
- **Dependency Injection** - passing dependencies from outside

---

## Common violations

- Using `new` to create concrete objects inside business logic
- High-level class directly instantiating repositories/services
- Hardcoded database connections in core logic
- Direct calls to external APIs from domain code

---

## Anti-patterns

- **Service Locator:** Global access to dependencies
- **God Class:** Class that creates its own dependencies
- **Tight Coupling:** Hard dependencies on concrete classes
- **New in Constructor:** Creating objects with `new` inside constructors

---

## Java examples

**Bad (violates DIP)**
```java
// High-level class depends on low-level details
class OrderService {
    private MySqlDatabase db = new MySqlDatabase(); // direct instantiation
    
    public void saveOrder(Order order) {
        db.save("orders", order); // depends on concrete MySQL
    }
}

// To switch to PostgreSQL: must modify OrderService!
```

**Good (DIP applied)**
```java
// Define abstraction (interface)
interface OrderRepository {
    void save(Order order);
}

// Low-level implementations
class MySqlOrderRepository implements OrderRepository {
    public void save(Order order) { /* MySQL save */ }
}

class PostgresOrderRepository implements OrderRepository {
    public void save(Order order) { /* PostgreSQL save */ }
}

// High-level class depends on abstraction, not details
class OrderService {
    private final OrderRepository repository; // injected, not created
    
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }
    
    public void saveOrder(Order order) {
        repository.save(order); // works with any implementation
    }
}

// Usage - dependency is injected
OrderService service = new OrderService(new MySqlOrderRepository());
// To switch: new OrderService(new PostgresOrderRepository())
```

---

## PHP examples

**Bad (violates DIP)**
```php
// High-level class depends on low-level details
class OrderService {
    private $db;
    
    public function __construct() {
        $this->db = new MySqlDatabase(); // direct instantiation
    }
    
    public function saveOrder(Order $order) {
        $this->db->table('orders')->insert($order);
    }
}
```

**Good (DIP applied)**
```php
// Define abstraction (interface)
interface OrderRepositoryInterface {
    public function save(Order $order);
}

// Low-level implementations
class MySqlOrderRepository implements OrderRepositoryInterface {
    public function save(Order $order) { /* MySQL save */ }
}

class PostgresOrderRepository implements OrderRepositoryInterface {
    public function save(Order $order) { /* PostgreSQL save */ }
}

// High-level class depends on abstraction
class OrderService {
    private $repository;
    
    public function __construct(OrderRepositoryInterface $repository) {
        $this->repository = $repository;
    }
    
    public function saveOrder(Order $order) {
        $this->repository->save($order);
    }
}

// Usage - dependency is injected
$service = new OrderService(new MySqlOrderRepository());
// To switch: new OrderService(new PostgresOrderRepository())
```

---

## How to apply

1. **Use interfaces** - define contracts between layers
2. **Inject dependencies** - pass them via constructor or setters
3. **Avoid `new` in business logic** - let DI container or factory create objects
4. **Depend on abstractions** - interfaces, not concrete classes
5. **Apply Inversion of Control (IoC)** - framework manages object creation
6. Use **DI Containers** (e.g., Spring, Laravel, Symfony) for management

---

## Benefits

- Easy to test with mocks/stubs
- Swap implementations without changing business logic
- Flexible architecture
- Better separation of concerns
- Supports multiple implementations

---

## Related principles

- **Single Responsibility:** DIP helps isolate responsibilities
- **Open/Closed:** DIP enables OCP by depending on abstractions
- **Interface Segregation:** Small interfaces support DIP

---

## Notes/Warnings

- DIP requires more initial setup but pays off in long-term maintenance
- Use DI containers for complex applications
- "Depend on abstractions" doesn't mean "use abstract classes" - interfaces are preferred
- Constructor injection is the most common approach
