# **4. [I] Interface Segregation Principle (ISP)**

---

## Definition

No client should be forced to depend on methods it does not use. Interfaces should be **specific to the client's needs**, not giant catch-all contracts.

---

## Plain Language Translation

> "Don't make a menu with everything for everyone. Each client orders what they want. If someone only wants coffee, don't make them order a whole dish just because the menu has everything."

**Analogy:** It's like a TV remote. You don't want buttons for something the TV doesn't do. Each functionality has its own specific button.

---

## Why it matters

- **Reduces unnecessary dependencies** - classes only know what they need
- **Improves maintainability** - changes to unused methods don't affect clients
- **Enhances testability** - easier to mock what you actually use
- **Prevents fat interfaces** - avoids forcing empty implementations
- **Increments flexibility** - clients can choose what to implement

---

## Key concepts

- **Client-specific interfaces** - interfaces built for specific needs
- **Role-based interfaces** - interfaces organized by role/usage
- **Fat interfaces** - large interfaces with many unrelated methods
- **Decoupling** - reducing dependencies between components

---

## Common violations

- One interface with dozens of methods for different purposes
- Implementing classes forced to provide empty implementations
- Clients that only use a fraction of an interface's methods
- Mixing unrelated responsibilities in one interface

---

## Anti-patterns

- **God Interface:** One interface to rule them all
- **Interface Pollution:** Contaminating interfaces with unrelated methods
- **Forced Implementation:** Making classes implement methods they don't need

---

## Java examples

**Bad (violates ISP)**
```java
// One giant interface - fat and polluted
interface Machine {
    void print(Document d);
    void scan(Document d);
    void fax(Document d);
    void copy(Document d);
}

class SimplePrinter implements Machine {
    public void print(Document d) { /* print */ }
    public void scan(Document d) { /* forced to implement - do nothing? */ }
    public void fax(Document d) { /* forced to implement - do nothing? */ }
    public void copy(Document d) { /* forced to implement - do nothing? */ }
}
```

**Good (ISP applied)**
```java
// Segregated, client-specific interfaces
interface Printer { void print(Document d); }
interface Scanner { Document scan(); }
interface Fax { void fax(Document d); }
interface Copier { void copy(Document d); }

// Each machine implements only what it needs
class SimplePrinter implements Printer {
    public void print(Document d) { /* print */ }
}

class MultifunctionPrinter implements Printer, Scanner, Fax, Copier {
    public void print(Document d) { /* print */ }
    public void scan(Document d) { /* scan */ }
    public void fax(Document d) { /* fax */ }
    public void copy(Document d) { /* copy */ }
}
```

---

## PHP examples

**Bad (violates ISP)**
```php
// One giant interface
interface WorkerInterface {
    public function code();
    public function design();
    public function test();
    public function deploy();
}

class Developer implements WorkerInterface {
    public function code() { /* code */ }
    public function design() { /* forced to implement */ }
    public function test() { /* forced to implement */ }
    public function deploy() { /* forced to implement */ }
}
```

**Good (ISP applied)**
```php
// Segregated interfaces
interface CoderInterface { public function code(); }
interface DesignerInterface { public function design(); }
interface TesterInterface { public function test(); }
interface DeployerInterface { public function deploy(); }

// Each role implements only what it needs
class Developer implements CoderInterface, DeployerInterface {
    public function code() { /* code */ }
    public function deploy() { /* deploy */ }
}

class QATester implements TesterInterface {
    public function test() { /* test */ }
}
```

---

## How to apply

1. **Start with small interfaces** - don't create fat interfaces upfront
2. **Think about clients** - what does each client actually need?
3. **Prefer many small interfaces** over one large interface
4. Use **composition** - combine small interfaces as needed
5. In PHP, leverage **traits** for shared implementation

---

## Benefits

- Smaller, focused dependencies
- Easier to understand and maintain
- More flexible design
- Better testability
- Reduced coupling

---

## Related principles

- **Single Responsibility:** ISP supports SRP
- **Dependency Inversion:** Depends on abstractions (but small ones)
- **Liskov Substitution:** Small interfaces make substitution clearer

---

## Notes/Warnings

- Don't over-segregate! Balance between focused and manageable
- Sometimes combining interfaces is appropriate
- In PHP, traits can provide default implementations