# Orthogonality and Low Coupling

---

## Definition

Orthogonality means that changes in one component do not affect the behavior of other components. Components should be independent and interact only through well-defined interfaces. Low coupling is the degree to which one module depends on other modules.

From "The Pragmatic Programmer": Orthogonal systems are composed of components that can be changed, added, or removed without affecting other components.

---

## Plain Language Translation

> "Components should work independently. A change in one place shouldn't cascade to other places. Keep modules decoupled."

**Analogy:** It's like electrical outlets. Each outlet works independently. If one burns out, the others keep working. You don't need to rewire the entire house when one fails.

---

## Why it matters

- **Isolated changes** - modifying one component doesn't break others
- **Easier testing** - components can be tested in isolation
- **Reusability** - independent components can be reused in different contexts
- **Parallel development** - teams can work on different components simultaneously
- **Reduced risk** - changes have predictable, localized impact
- **Maintainability** - easier to understand, debug, and extend
- **Scalability** - new features can be added without touching existing code
- **Refactoring safety** - you can improve code without fear of side effects

---

## Key concepts

- **Orthogonality:** Changes in one component don't affect other components
- **Coupling:** The degree of interdependence between modules
- **Cohesion:** How strongly related and focused the responsibilities of a single module are
- **Interface Segregation:** Small, focused interfaces reduce coupling
- **Dependency Injection:** Reduces hardcoded dependencies
- **Mediator Pattern:** Centralizes communication between components
- **Event-driven architecture:** Decoupled communication through events

---

## Common violations

- Direct dependencies between unrelated modules
- Global state shared across components
- Circular dependencies between modules
- Passing too many parameters between functions/methods
- Deep inheritance hierarchies
- "Train wrecks" - method chains like `a.getB().getC().getD().doSomething()`
- Sharing mutable state between components
- Hardcoding dependencies instead of injecting them

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Orthogonality and low coupling mean:
- **Classes** depend on **interfaces**, not concrete implementations
- **Dependency Injection** removes hardcoded dependencies
- **Events/Delegates** for decoupled communication
- **Composition** over inheritance
- **Facade pattern** to simplify complex subsystems
- **Observer pattern** for loose coupling between publisher/subscriber

### In Procedural Programming

Orthogonality and low coupling mean:
- **Functions** are self-contained and independent
- **Data** is passed explicitly as parameters
- **No global variables** - state is passed as arguments
- **Module-level scope** - limit visibility of functions/data
- **Include/require independence** - files don't depend on include order

### In Functional Programming

Orthogonality and low coupling mean:
- **Pure functions** have no side effects
- **Immutable data** prevents shared state issues
- **Function composition** - chain transformations without coupling
- **Higher-order functions** - pass behavior, not data dependencies
- **No mutable state** between function calls

### Key Takeaway

> **The principle is the same: components should be independent.**
>
> **The implementation differs based on the paradigm:**
> - OOP: interfaces, dependency injection, composition, patterns
> - Procedural: explicit parameters, module boundaries, no globals
> - Functional: pure functions, immutability, composition

---

## OOP Examples

### Java (OOP)

**Bad (high coupling)**
```java
// Tight coupling - direct instantiation
class OrderService {
    private MySqlDatabase db = new MySqlDatabase();  // tightly coupled
    
    public void saveOrder(Order order) {
        db.save("orders", order);  // depends on concrete MySQL
    }
    
    public void sendEmail(Order order) {
        new EmailSender().send(order.getEmail(), "Order confirmation");
    }
}

// To switch to PostgreSQL: must modify OrderService!
```

**Good (low coupling - orthogonal)**
```java
// Loose coupling via interfaces and dependency injection
interface OrderRepository {
    void save(Order order);
}

interface EmailSender {
    void send(String to, String message);
}

class OrderService {
    private final OrderRepository repository;
    private final EmailSender sender;
    
    // Dependencies are injected - OrderService doesn't create them
    public OrderService(OrderRepository repository, EmailSender sender) {
        this.repository = repository;
        this.sender = sender;
    }
    
    public void processOrder(Order order) {
        repository.save(order);
        sender.send(order.getEmail(), "Order confirmation");
    }
}

// Usage - dependencies are created elsewhere
OrderService service = new OrderService(
    new PostgresOrderRepository(),
    new SmtpEmailSender()
);
```

### PHP (OOP)

**Bad (high coupling)**
```php
// Tight coupling - direct instantiation
class OrderService {
    private $db;
    
    public function __construct() {
        $this->db = new MySqlDatabase();  // tightly coupled
    }
    
    public function createOrder($data) {
        $this->db->table('orders')->insert($data);
    }
}
```

**Good (low coupling - orthogonal)**
```php
// Loose coupling via interfaces and dependency injection
interface OrderRepositoryInterface {
    public function create(array $data): Order;
}

interface MailerInterface {
    public function send(string $to, string $message): void;
}

class OrderService {
    private $repository;
    private $mailer;
    
    // Dependencies are injected
    public function __construct(
        OrderRepositoryInterface $repository,
        MailerInterface $mailer
    ) {
        $this->repository = $repository;
        $this->mailer = $mailer;
    }
    
    public function createOrder(array $data): Order {
        $order = $this->repository->create($data);
        $this->mailer->send($order->email, 'Order created!');
        return $order;
    }
}

// Usage - dependencies are created elsewhere
$service = new OrderService(
    new EloquentOrderRepository(),
    new SmtpMailer()
);
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (high coupling - global state)**
```php
<?php
// Global state creates hidden dependencies
$db = null;

function initDatabase() {
    global $db;
    $db = new MySqlDatabase();
}

function createUser($data) {
    global $db;
    // Hidden dependency on global $db
    $db->table('users')->insert($data);
}

function sendWelcomeEmail($email) {
    global $mailer;
    // What if $mailer wasn't initialized?
    $mailer->send($email, 'Welcome!');
}
?>
```

**Good (low coupling - explicit dependencies)**
```php
<?php
// No global state - dependencies are passed explicitly

function validateUserData(array $data): array {
    if (empty($data['email'])) {
        throw new InvalidArgumentException('Email required');
    }
    return $data;
}

function saveUserToDatabase(array $data, $db): int {
    return $db->table('users')->insert($data);
}

function sendWelcomeEmail(string $email, $mailer): void {
    $mailer->send($email, 'Welcome!');
}

// Each function is independent and testable
// You can pass different implementations

function createUser(array $data, $db, $mailer): int {
    $validated = validateUserData($data);
    $userId = saveUserToDatabase($validated, $db);
    sendWelcomeEmail($validated['email'], $mailer);
    return $userId;
}
?>
```

---

## How to apply

### General Steps

1. **Identify dependencies** - What does this module depend on?
2. **Reduce dependencies** - Remove unnecessary couplings
3. **Use interfaces** - Define contracts between components
4. **Inject dependencies** - Pass dependencies from outside
5. **Favor composition** - Compose behavior from smaller parts
6. **Limit visibility** - Expose only what's necessary
7. **Avoid shared state** - Pass data explicitly

### For OOP

1. Use **Dependency Injection** - Pass dependencies via constructor/setters
2. Apply **Interface Segregation** - Small, focused interfaces
3. Use **Events/Delegates** for decoupled communication
4. Apply **Facade Pattern** to hide complex subsystems
5. Prefer **composition over inheritance**

### For Procedural

1. Pass dependencies explicitly as parameters
2. Avoid global variables
3. Keep functions self-contained
4. Use module-level scope appropriately
5. Separate pure functions from I/O

### For Functional

1. Write pure functions with no side effects
2. Keep data immutable
3. Use function composition
4. Pass functions as parameters (higher-order functions)
5. Isolate side effects to specific functions

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Isolated changes | Modify one component without breaking others |
| Easier testing | Test each component independently |
| Higher reusability | Components work in different contexts |
| Parallel development | Teams work on different components |
| Reduced risk | Changes have predictable impact |
| Better maintainability | Easier to understand and debug |
| Improved scalability | Add features without touching existing code |
| Refactoring safety | Improve code without fear of side effects |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested by the user
- **DO NOT convert OOP code to procedural** unless explicitly requested by the user
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **DO NOT add classes to procedural codebases** unless explicitly requested
- **DO NOT rewrite procedural functions as classes** in OOP codebases unless explicitly requested
- **APPLY coupling principles within the existing paradigm** - orthogonality matters regardless of paradigm

### Context Matters

- If working on **procedural code**: keep it procedural but apply coupling principles
- If working on **OOP code**: keep it OOP but use interfaces, DI, patterns
- If working on **functional code**: keep it functional but use pure functions, immutability

### Coupling Principles by Paradigm

| Paradigm | How to Achieve Orthogonality |
|----------|------------------------------|
| OOP | Interfaces, DI, composition, patterns |
| Procedural | Explicit parameters, no globals, modular |
| Functional | Pure functions, immutability, composition |

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Separation of Concerns (SoC):** Orthogonality supports SoC by keeping responsibilities separate
- **Single Responsibility (SRP):** Classes with one responsibility are easier to keep orthogonal
- **Interface Segregation (ISP):** Small interfaces reduce unnecessary coupling
- **Dependency Inversion (DIP):** Depends on abstractions to achieve orthogonality
- **The Pragmatic Programmer:** Orthogonality is a core principle from this book

---

## Notes/Warnings

- Orthogonality is a **universal principle** - applies to ALL programming paradigms
- The implementation differs, but the goal is the same: **independent components**
- Don't over-engineer! Balance orthogonality with simplicity
- Too many small interfaces can become hard to manage
- Consider the cost of added abstraction vs. the benefit of decoupling
- **Always apply coupling principles within the existing paradigm** - don't change paradigms to achieve orthogonality