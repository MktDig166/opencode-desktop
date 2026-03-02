**2. [O] Open/Closed Principle (OCP)**

---

## Definition

Software entities (classes, modules, functions) should be **open for extension** but **closed for modification**. You can add new behavior without changing existing code.

---

## Plain Language Translation

> "If you need to add something new, extend it, don't change what already works. Working code should not be modified just to add features."

**Analogy:** It's like a smartphone with cases. You can change the appearance (extend) without opening the phone (modify).

---

## Why it matters

- **Prevents regressions** - existing code keeps working
- **Reduces bugs** - less modification = less risk
- **Enables scalability** - new features without touching stable code
- **Facilitates testing** - existing tests remain valid
- **Supports team work** - multiple developers can add features independently

---

## Key concepts

- **Extension:** Adding new functionality by creating new classes/modules
- **Modification:** Changing existing code to add functionality
- **Polymorphism:** Same interface, different implementations
- **Abstraction:** Using interfaces/base classes to define contracts

---

## Common violations

- Adding new `if/else` or `switch/case` branches for every new type
- Modifying a class to handle a new feature
- Checking object type before acting (`instanceof`)
- Hardcoding behaviors that should vary

---

## Anti-patterns

- **Switch Statement:** Repeated switch/case for new types
- **Shotgun Surgery:** One change requires changes in many places
- **Feature Envy:** Manipulating another class's data instead of extending it

---

## Java examples

**Bad (violates OCP)**
```java
public class ShapeRenderer {
    public void render(Object shape) {
        if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            // draw circle
        } else if (shape instanceof Square) {
            Square s = (Square) shape;
            // draw square
        } else if (shape instanceof Triangle) {
            // add new branch every time
        }
    }
}
```

**Good (OCP applied)**
```java
interface Drawable {
    void draw();
}

class Circle implements Drawable {
    public void draw() { /* draw circle */ }
}

class Square implements Drawable {
    public void draw() { /* draw square */ }
}

class Triangle implements Drawable {
    public void draw() { /* draw triangle */ }
}

class ShapeRenderer {
    public void render(Drawable shape) {
        shape.draw(); // works with ANY Drawable, no modification needed
    }
}
```

---

## PHP examples

**Bad (violates OCP)**
```php
class PaymentProcessor {
    public function process($paymentType) {
        if ($paymentType === 'credit') {
            // process credit
        } elseif ($paymentType === 'debit') {
            // process debit
        } elseif ($paymentType === 'pix') {
            // add new type = modify this class
        }
    }
}
```

**Good (OCP applied)**
```php
interface PaymentInterface {
    public function pay(float $amount);
}

class CreditPayment implements PaymentInterface {
    public function pay(float $amount) { /* credit logic */ }
}

class DebitPayment implements PaymentInterface {
    public function pay(float $amount) { /* debit logic */ }
}

class PixPayment implements PaymentInterface {
    public function pay(float $amount) { /* pix logic */ }
}

class PaymentProcessor {
    public function process(PaymentInterface $payment, float $amount) {
        $payment->pay($amount); // new payment types = new classes, no modification
    }
}
```

---

## How to apply

1. Identify what varies in your code and separate it from what stays the same
2. Use **polymorphism** (interfaces, abstract classes) instead of conditionals
3. Apply **Strategy Pattern** - encapsulate algorithms
4. Apply **Factory Pattern** - create objects without specifying exact class
5. Use **inheritance** wisely - prefer composition

---

## Benefits

- New features without touching tested code
- Fewer regressions and bugs
- Easier maintenance
- Better code organization
- Supports plugin architectures

---

## Related principles

- **Single Responsibility:** OCP works better with SRP
- **Liskov Substitution:** Subtypes must fulfill OCP contracts
- **Dependency Inversion:** Depend on abstractions for OCP

---

## Notes/Warnings

- Don't over-engineer! Not every change needs OCP
- YAGNI (You Aren't Gonna Need It) - apply when you have actual extensions
- OCP and SRP often work together
