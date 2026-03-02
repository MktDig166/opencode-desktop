# Meaningful Names

---

## Definition

Names should reveal intent. The name of a variable, function, class, or module should tell you why it exists, what it does, and how it is used. Choosing good names is one of the most important skills in programming.

From "Clean Code" by Robert C. Martin: "The name of a variable, function, or class, should answer all the big questions: why it exists, what it does, and how it is used."

---

## Plain Language Translation

> "Names should tell you what they mean. If you need a comment to explain a name, the name is not good enough."

**Analogy:** It's like labeling boxes in a warehouse. If you have a box labeled "Stuff," no one knows what's inside. If you label it "Winter Clothes," everyone knows immediately.

---

## Why it matters

- **Readability** - Code is read far more often than it is written
- **Maintainability** - Easy to understand and modify
- **Reduced comments** - Good names reduce the need for explanatory comments
- **Self-documenting code** - Names reveal intent
- **Fewer bugs** - Clear names prevent misunderstandings
- **Better collaboration** - Team members can understand code quickly

---

## Key concepts

- **Intent-revealing names** - Names should answer "why it exists"
- **Avoid disinformation** - Don't use names that mean something else
- **Meaningful distinction** - Names must differ in meaning
- **Pronounceable names** - If you can't pronounce it, you can't discuss it
- **Searchable names** - Single-letter names are hard to find
- **Avoid encodings** - No hungarian notation, no type prefixes
- **Classes** - Noun or noun phrases (User, Account, Payment)
- **Methods** - Verb or verb phrases (getUser, calculateTotal, saveOrder)

---

## Common violations

- Using single letters (i, j, k, x, y, z) except in loops
- Using vague names (data, info, temp, stuff, thing)
- Using similar names that mean different things (accountData vs accountInfo)
- Using names that conflict with language keywords
- Adding type information to names (strName, intValue, boolFlag)
- Using magic numbers without named constants
- Abbreviations that obscure meaning (calcTot vs calculateTotal)

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Meaningful names mean:
- **Classes:** Nouns that represent objects (User, Order, PaymentProcessor)
- **Methods:** Verbs that represent actions (getUser, calculateTotal, processOrder)
- **Variables:** Descriptive nouns that represent state (customerName, orderItems, totalAmount)
- **Constants:** ALL_CAPS with underscores (MAX_RETRY_COUNT, DEFAULT_TIMEOUT)

### In Procedural Programming

Meaningful names mean:
- **Functions:** Verb phrases that describe what they do (calculate_total, validate_email, save_user)
- **Variables:** Clear nouns (customer_name, order_items, total_amount)
- **Constants:** ALL_CAPS (MAX_RETRY_COUNT, DEFAULT_TIMEOUT)
- **Modules/Files:** Descriptive names that indicate purpose (user_validation.php, order_processing.php)

### In Functional Programming

Meaningful names mean:
- **Functions:** Descriptive verb phrases (calculate-total, validate-email, save-user)
- **Variables:** Immutable values with clear purposes (customer-name, order-items)
- **Pure functions:** Names that indicate transformation (map, filter, reduce)

### Key Takeaway

> **The principle is the same: names should reveal intent.**
>
> **The implementation differs based on the paradigm:**
> - OOP: camelCase for methods/variables, PascalCase for classes
> - Procedural: snake_case for functions/variables, SCREAMING_SNAKE_CASE for constants
> - Functional: kebab-case or snake_case, descriptive and concise

---

## OOP Examples

### Java (OOP)

**Bad (unclear names)**
```java
// What is d? What is x? Unclear!
int d;
String x;
List<Account> a = getList();
if (status == 1) { ... }
```

**Good (meaningful names)**
```java
int daysSinceCreation;
String customerName;
List<Account> activeAccounts = getActiveAccounts();
if (accountStatus == AccountStatus.ACTIVE) { ... }
```

**Bad (type encoding)**
```java
String strName;
int intValue;
boolean boolFlag;
List<AccountVO> accountList;
```

**Good (no encoding)**
```java
String name;
int value;
boolean isActive;
List<Account> accounts;
```

### PHP (OOP)

**Bad (unclear names)**
```php
class UserMgr {
    public function proc($d) {
        $x = $this->repo->get($d);
        return $x;
    }
}
```

**Good (meaningful names)**
```php
class UserController {
    public function processOrder(Order $order) {
        $customer = $this->repository->findById($order->customerId);
        return $customer;
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (unclear names)**
```php
<?php
function calc($a, $b) {
    $x = $a * $b;
    return $x;
}

$d = calc(10, 20);
?>
```

**Good (meaningful names)**
```php
<?php
function calculateOrderTotal(float $itemPrice, int $quantity): float {
    $totalAmount = $itemPrice * $quantity;
    return $totalAmount;
}

$orderTotal = calculateOrderTotal(10.00, 20);
?>
```

---

## How to apply

### General Steps

1. **Ask "what does this represent?"** - Use that as the name
2. **Avoid single letters** - Except for loop counters (i, j, k)
3. **Use search-friendly names** - Longer names are easier to find
4. **Replace magic numbers** - Use named constants
5. **Match the paradigm** - Use appropriate naming conventions
6. **Review names** - If you need a comment to explain, rename it

### Naming Conventions by Paradigm

| Element | OOP (Java) | OOP (PHP) | Procedural (PHP) | Functional |
|---------|------------|-----------|-----------------|------------|
| Class | PascalCase | PascalCase | - | - |
| Method | camelCase | camelCase | snake_case | kebab-case/snake_case |
| Variable | camelCase | camelCase | snake_case | kebab-case/snake_case |
| Constant | UPPER_SNAKE | UPPER_SNAKE | UPPER_SNAKE | kebab-case |

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Self-documenting | Code explains itself |
| Reduced comments | Less need for explanatory notes |
| Better readability | Anyone can understand |
| Easier maintenance | Changes are obvious |
| Fewer bugs | Misunderstandings reduced |
| Team collaboration | Shared understanding |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY naming conventions within the existing paradigm**

### Context Matters

- If working on **procedural code**: use snake_case, descriptive names
- If working on **OOP code**: use camelCase/PascalCase, class-based names
- If working on **functional code**: use functional naming conventions

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Small Functions:** Functions with good names do one thing well
- **Clean Code:** Meaningful names are a foundation of clean code
- **Code Formatting:** Consistent naming supports formatting
- **DRY:** Meaningful names help identify duplication

---

## Notes/Warnings

- **Long names are better than short ambiguous names** - Don't be afraid of descriptive names
- **Consistency matters** - Use the same name for the same concept across the codebase
- **Context matters** - A name should make sense in its context (e.g., `i` is fine in a for loop)
- **Refactor if needed** - If a name becomes unclear, rename it