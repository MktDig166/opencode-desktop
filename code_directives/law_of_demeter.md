# Law of Demeter (LoD)

---

## Definition

The Law of Demeter (LoD), also known as the "principle of least knowledge," states that a method should only call methods on:
1. The object itself
2. Objects passed as parameters
3. Objects it creates
4. Its direct component objects

In simpler terms: "Talk to your immediate neighbors only. Don't talk to strangers."

---

## Plain Language Translation

> "Only ask your direct friends for help. Don't ask your friend's friends for help."

**Analogy:** It's like ordering food at a restaurant. You talk to the waiter, not to the chef, not to the farmer who grew the vegetables, not to the delivery driver. You only communicate with your immediate contact.

---

## Why it matters

- **Reduces coupling** - your code doesn't depend on internal structure of other objects
- **Isolates changes** - changes to internal structure of one object don't break your code
- **Improves maintainability** - easier to modify one component without affecting others
- **Enhances testability** - easier to mock dependencies
- **Prevents "train wrecks"** - avoids long chains of method calls
- **Makes code more readable** - each method call has a clear purpose
- **Follows orthogonality** - naturally leads to decoupled, orthogonal code

---

## Key concepts

- **Train wreck:** A chain of method calls like `a.getB().getC().getD().doSomething()`
- **Direct knowledge:** Only knowing about immediate dependencies
- **Tell, don't ask:** Ask objects to do things, don't interrogate their internals
- **Encapsulation:** Hiding internal structure behind well-defined interfaces
- **Method call depth:** How many objects you traverse to get something done

---

## Common violations

- `object.getA().getB().getC().doSomething()` - traversing too deep
- Accessing nested properties: `user.getAddress().getCity().getName()`
- Depending on return values of other methods' return values
- Asking for internal objects instead of asking the owner to do it
- Getters that expose internal objects (enabling train wrecks)

---

## OOP Examples

### Java (OOP)

**Bad (Law of Demeter violation - train wreck)**
```java
// Violates LoD - depends on internal structure
class Client {
    void process() {
        String city = order.getCustomer().getAddress().getCity().getName();
    }
}
```

**Good (Law of Demeter - ask, don't tell)**
```java
// LoD compliant - asks the owner to do the work
class Client {
    void process() {
        String city = order.getCustomerCity();
    }
}

class Order {
    private Customer customer;
    
    // Delegate the work instead of exposing internals
    public String getCustomerCity() {
        return customer.getAddress().getCity().getName();
    }
}

class Customer {
    private Address address;
    
    public String getCityName() {
        return address.getCity().getName();
    }
}
```

**Another Bad Example**
```java
// Exposing internals enables LoD violations
class Order {
    public Customer customer;  // public field - exposes internals!
}

// Client can do: order.customer.getAddress().getCity()
```

**Another Good Example**
```java
// Proper encapsulation
class Order {
    private Customer customer;
    
    // Only expose what you need
    public String getCustomerCityName() {
        return customer.getAddress().getCity().getName();
    }
    
    public String getCustomerEmail() {
        return customer.getEmail();
    }
}
```

### PHP (OOP)

**Bad (Law of Demeter violation - train wreck)**
```php
// Violates LoD - depends on internal structure
$city = $order->getCustomer()->getAddress()->getCity()->getName();
```

**Good (Law of Demeter - ask, don't tell)**
```php
// LoD compliant - asks the owner to do the work
class Order {
    private $customer;
    
    public function getCustomerCityName(): string {
        return $this->customer->getAddress()->getCity()->getName();
    }
}

// Usage
$city = $order->getCustomerCityName();  // Clean!
```

**Bad (Getters that expose internals)**
```php
class Order {
    public $customer;  // Exposes internal object!
}

// Client can do: $order->customer->getAddress()...
```

**Good (Encapsulated)**
```php
class Order {
    private $customer;
    
    public function customerCity(): string {
        return $this->customer->address()->city()->name();
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

The Law of Demeter applies to function chains in procedural code as well.

**Bad (Function chain - like train wreck)**
```php
<?php
// Too many levels of indirection
$city = getCityName(getAddress(getCustomer($order)));
?>
```

**Good (Direct function calls)**
```php
<?php
// LoD compliant - each function does one thing
function getCustomerCity(Order $order): string {
    $customer = getCustomer($order);
    $address = getAddress($customer);
    return getCityName($address);
}

// Usage
$city = getCustomerCity($order);
?>
```

---

## How to apply

### General Steps

1. **Identify train wrecks** - Look for chains like `a.getB().getC().getD()`
2. **Ask, don't ask** - Instead of getting internal objects, ask the owner to do it
3. **Create delegating methods** - Add methods that delegate to internal objects
4. **Limit object creation** - Only create what you need in the method
5. **Hide internals** - Don't expose internal objects through getters

### Practical Tips

- **Instead of:** `$user->getAddress()->getCity()->getName()`
- **Do:** `$user->getCityName()`

- **Instead of:** `$order->getCustomer()->getEmail()`
- **Do:** `$order->getCustomerEmail()`

- **Instead of:** `$service->getDatabase()->query($sql)`
- **Do:** `$service->executeQuery($sql)`

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Reduced coupling | Code doesn't depend on internals |
| Isolated changes | Internal changes don't break clients |
| Better encapsulation | Objects control their own behavior |
| Improved readability | Clear, direct method calls |
| Easier testing | Fewer dependencies to mock |
| Supports orthogonality | Natural path to decoupled code |

---

## Relation to Orthogonality

### How LoD Relates to Orthogonality

The Law of Demeter is a **specific, practical rule** that naturally leads to **orthogonal code**.

| Aspect | Law of Demeter | Orthogonality |
|--------|----------------|---------------|
| Goal | Don't depend on internals | Components are independent |
| Focus | Method call chains | Component independence |
| Application | Day-to-day coding | Architectural level |
| Result | Loose coupling | Isolated changes |

### Why LoD Leads to Orthogonality

1. **You don't know internals** - Changes to internal structure don't affect you
2. **Ask, don't tell** - You request behavior, not data
3. **Limited knowledge** - You only depend on immediate neighbors
4. **Isolated impact** - Changes stay localized

### Example Connection

```java
// Orthogonal: Components are independent
// LoD: How to achieve it in practice

// This is orthogonal because:
interface OrderService {
    void process(Order order);
}

// This follows LoD:
class OrderServiceImpl implements OrderService {
    private final CustomerRepository customerRepo;
    private final EmailService emailService;
    
    // Only knows immediate neighbors (dependencies)
    public void process(Order order) {
        // Doesn't dig into internal structure
        String email = order.getCustomerEmail();  // Delegated method
        emailService.sendConfirmation(email);
    }
}
```

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested by the user
- **DO NOT convert OOP code to procedural** unless explicitly requested by the user
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY the Law of Demeter within the existing paradigm** - it's a universal principle

### Context Matters

- If working on **procedural code**: apply LoD to function chains
- If working on **OOP code**: apply LoD to method call chains
- If working on **functional code**: apply LoD to function compositions

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Orthogonality:** LoD is a practical rule that leads to orthogonal code
- **Encapsulation:** Hide internal state, expose behavior
- **Tell, Don't Ask:** Ask objects to do things, don't interrogate them
- **Low Coupling:** LoD naturally produces low coupling
- **Single Responsibility:** Focused methods follow LoD naturally
- **Interface Segregation:** Small interfaces support LoD

---

## Notes/Warnings

- **LoD is a means to an end** - It's a tool to achieve orthogonality and low coupling
- **Don't over-apply** - Sometimes deep traversal is necessary
- **Balance with practicality** - Sometimes adding too many delegating methods adds noise
- **Consistency matters** - Apply consistently within a codebase
- **LoD supports orthogonality** - Use it to achieve the broader goal of independent components
- **Ask "is this my direct neighbor?"** - Before accessing any method/property