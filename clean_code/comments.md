# Comments

---

## Definition

Comments should be used sparingly and only when they add value that the code itself cannot convey. The best comment is one you don't have to write because the code is self-explanatory. Comments should explain "why," not "what."

From "Clean Code" by Robert C. Martin: "Comments are, at best, a necessary evil. If our programming languages were expressive enough, or if we had the talent to subtly wield those languages to express our intent, we would not need comments very much."

---

## Plain Language Translation

> "If you need a comment to explain what the code does, the code is not clear enough. Write better code instead."

**Analogy:** It's like instructions on a product. Good products don't need manuals for basic use. Code should be the same way - self-explanatory.

---

## Why it matters

- **Code clarity** - Self-documenting code is easier to read
- **Maintenance** - Comments can become outdated and misleading
- **Trust** - Outdated comments erode confidence in code
- **Noise** - Too many comments clutter code
- **Truth** - Code tells the truth; comments can lie
- **Professionalism** - Clean code doesn't need excuses

---

## Key concepts

- **Self-documenting code** - Code that explains itself
- **Intent comments** - Explain why, not what
- **Javadocs/PhpDocs** - API documentation for public functions
- **Warning comments** - Alert about consequences
- **TODO comments** - Mark future work
- **Legal comments** - Copyright, licensing
- **Comments vs code** - Code changes, comments often don't

---

## Common violations

- Commented-out code left in production
- Explaining what the code does instead of why
- Outdated comments that contradict the code
- Obvious comments that add no value
- Block comments explaining every line
- Comments that lie (contradicted by code)
- Magic numbers without explanation (unless named constants)

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Comments mean:
- **Javadoc/Phpdoc** for public APIs
- **Intent comments** for complex business logic
- **Warning comments** for side effects
- **TODO comments** for future work
- **No comments** for obvious code

### In Procedural Programming

Comments mean:
- **Intent comments** for complex functions
- **Block comments** for algorithm explanations
- **Parameter comments** if not self-evident
- **Legal comments** for licensing
- **No comments** for simple code

### In Functional Programming

Comments mean:
- **Intent comments** for transformation logic
- **Type annotations** (often preferred over comments)
- **Pure function documentation**
- **No comments** for obvious transformations

### Key Takeaway

> **The principle is the same: comments should be minimal and purposeful.**
>
> **The implementation differs slightly based on the paradigm, but the goal is the same: code should be self-explanatory.**

---

## OOP Examples

### Java (OOP)

**Bad (unnecessary comments)**
```java
// Initialize the counter to 0
int counter = 0;

// If the user is active, return true
if (user.isActive()) {
    return true;
}

// Add item to the list
items.add(item);
```

**Good (no comments needed - code is clear)**
```java
int counter = 0;
if (user.isActive()) {
    return true;
}
items.add(item);
```

**Good (necessary comments - intent and warnings)**
```java
// Business Rule: Discount applies only to orders over $100
// This is mandated by the Q1 2024 marketing campaign
if (orderTotal > 100) {
    applyDiscount(orderTotal, DISCOUNT_RATE);
}

// WARNING: This method is not thread-safe.
// Ensure proper synchronization in concurrent contexts.
public void processOrder(Order order) { ... }
```

### PHP (OOP)

**Bad (obvious comments)**
```php
class UserService {
    // Get the user by ID
    public function getUserById(int $id): ?User {
        // Return the user
        return $this->repository->find($id);
    }
}
```

**Good (no obvious comments)**
```php
class UserService {
    public function getUserById(int $id): ?User {
        return $this->repository->find($id);
    }
}
```

**Good (necessary comments)**
```php
/**
 * Calculates the shipping cost based on distance and weight.
 * 
 * Business Rule: Free shipping for orders over $50 (per marketing Q1 2024)
 *
 * @param float $distance Distance in kilometers
 * @param float $weight Weight in kilograms
 * @return float Shipping cost in dollars
 */
public function calculateShipping(float $distance, float $weight): float { ... }
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (commented-out code)**
```php
<?php
function calculateTotal($items) {
    $total = 0;
    foreach ($items as $item) {
        $total += $item['price'] * $item['quantity'];
    }
    // $total = applyDiscount($total); // TODO: add discount later
    return $total;
}
?>
```

**Good (no commented code, clean)**
```php
<?php
function calculateTotal(array $items): float {
    return array_reduce($items, function($total, $item) {
        return $total + ($item['price'] * $item['quantity']);
    }, 0);
}
?>
```

---

## How to apply

### General Rules

1. **If you can write self-explanatory code, do it** - Rename variables, extract functions
2. **Comments should explain "why"** - Not "what" the code does
3. **Keep comments updated** - Or remove them
4. **Delete commented-out code** - Use version control instead
5. **Use TODO sparingly** - Mark for future work, then complete it
6. **Write Javadocs/Phpdocs** for public APIs
7. **Warn about side effects** - If they exist

### When to Use Comments

- **Business rules** - Explain why a business decision was made
- **Complex algorithms** - Explain the approach
- **Warnings** - Alert about consequences
- **Public APIs** - Document interfaces
- **Legal requirements** - Copyright, licensing
- **TODO items** - Mark work to be done

### When NOT to Use Comments

- **Explaining obvious code** - Rewrite the code instead
- **Commented-out code** - Delete it
- **What the code does** - Code should explain this
- **Redundant information** - Don't repeat what code says

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Cleaner code | Less clutter |
| Maintainable | No outdated comments to manage |
| Trustworthy | Comments match code |
| Professional | Code speaks for itself |
| Self-documenting | Intent is clear |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY comment best practices within the existing paradigm**

### Context Matters

- If working on **procedural code**: keep comments minimal and purposeful
- If working on **OOP code**: use Javadocs/Phpdocs for public APIs
- If working on **functional code**: prefer type annotations over comments

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Meaningful Names:** Good names reduce need for comments
- **Small Functions:** Small functions need fewer comments
- **DRY:** Don't repeat explanations in comments and code
- **KISS:** Simple code doesn't need explanations

---

## Notes/Warnings

- **Comments are a last resort** - Try to write better code first
- **Outdated comments are worse than no comments** - Keep them updated or delete them
- **Code tells the truth** - Comments can lie
- **Version control** - Use it for deleted code instead of commenting it out