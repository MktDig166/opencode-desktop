# YAGNI (You Aren't Gonna Need It)

---

## Definition

YAGNI stands for "You Aren't Gonna Need It." It's a principle that states: don't implement functionality or add code "just because it might be useful in the future." Only add code when you actually need it.

From "Extreme Programming Explained": YAGNI means implementing things when you actually need them, never when you foresee that you might need them.

---

## Plain Language Translation

> "Don't add code you don't need right now. When you need it later, you'll add it then."

**Analogy:** It's like building a house. You don't build 5 extra bedrooms "because you might have visitors in the future." You build what you need now. If guests come, you add a guest room later.

---

## Why it matters

- **Avoids unnecessary code** - less code to maintain, test, and debug
- **Saves time** - don't spend time on features that may never be used
- **Reduces complexity** - simpler code is easier to understand
- **Prevents over-engineering** - don't build flexible solutions for problems you don't have
- **Keeps code clean** - only what's needed is present
- **Improves focus** - work on what matters now, not speculation
- **Supports DRY** - waiting until needed helps identify the right abstraction

---

## Key concepts

- **Just-in-time implementation:** Add code when you need it, not before
- **Speculation:** Guessing future needs (usually wrong)
- **Simplicity:** Simple solutions for current problems
- **Refactoring:** You can always refactor later when needs become clear
- **YAGNI vs planning:** Planning is good; over-planning based on speculation is not

---

## Common violations

- Creating "flexible" interfaces "just in case"
- Adding parameters that "might be useful"
- Building complex abstractions before having concrete use cases
- Implementing features "for the future"
- Creating extensive frameworks or libraries prematurely
- Adding "hooks" or "extension points" without immediate need
- Building complex configuration systems for simple apps

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

YAGNI means:
- Don't create interfaces until you have multiple implementations
- Don't add abstract base classes "for future extensibility"
- Don't add methods "that might be useful"
- Don't create complex hierarchies before you need them

### In Procedural Programming

YAGNI means:
- Don't create utility functions for "potential" use cases
- Don't add parameters to functions "just in case"
- Don't build complex include structures before needed

### In Functional Programming

YAGNI means:
- Don't create higher-order functions for abstract patterns you don't use
- Don't add currying/partial application where simple functions work
- Don't build complex pipelines for simple transformations

### Key Takeaway

> **The principle is the same: implement what you need, not what you think you'll need.**
>
> **The implementation differs slightly based on the paradigm, but the mindset is the same across all.**

---

## OOP Examples

### Java (OOP)

**Bad (YAGNI violation - premature abstraction)**
```java
// Creating an interface "just in case"
interface UserRepository {
    void save(User user);
    void delete(User user);
    void findById(Long id);
    List<User> findAll();           // maybe needed?
    List<User> findByEmail(String email); // maybe needed?
}

class JdbcUserRepository implements UserRepository {
    @Override
    public void save(User user) { /*...*/ }
    @Override
    public void delete(User user) { /*...*/ }
    @Override
    public void findById(Long id) { /*...*/ }
    @Override
    public List<User> findAll() { return null; }  // not needed yet!
    @Override
    public List<User> findByEmail(String email) { return null; }  // not needed yet!
}
```

**Good (YAGNI - implement when needed)**
```java
// Simple implementation first
class UserRepository {
    public void save(User user) {
        // only what we need now
    }
}

// Later, when needed:
interface UserRepository {
    void save(User user);
    void delete(User user);
    User findById(Long id);
}
```

### PHP (OOP)

**Bad (YAGNI violation - over-engineering)**
```php
// Creating abstract class "for future flexibility"
abstract class BaseController {
    public function index() { /*...*/ }
    public function show($id) { /*...*/ }
    public function create() { /*...*/ }
    public function edit($id) { /*...*/ }
    public function update($id) { /*...*/ }
    public function delete($id) { /*...*/ }
    // All these methods "might be useful"
}

class UserController extends BaseController {
    // Only need index and show
}
```

**Good (YAGNI - simple and focused)**
```php
// Simple controller with only what we need
class UserController {
    public function index() { /* list users */ }
    public function show(int $id) { /* show user */ }
}

// Later, when needed:
class UserController {
    public function index() { /* list users */ }
    public function show(int $id) { /* show user */ }
    public function store(Request $request) { /* create user */ }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (YAGNI violation - unnecessary parameters)**
```php
<?php
// Adding parameters "just in case"
function createUser($name, $email, $phone = null, $address = null, $notes = null, $preferences = null) {
    // Only name and email are needed now
}

// Usage: createUser('John', 'john@email.com')
?>
```

**Good (YAGNI - simple parameters)**
```php
<?php
// Only what we need now
function createUser(string $name, string $email): int {
    // create user
}

// Later, when needed:
function createUser(string $name, string $email, ?string $phone = null): int {
    // create user with optional phone
}
?>
```

---

## How to apply

### General Steps

1. **Ask "do I need this now?"** - If not, don't add it
2. **Wait for concrete use cases** - Add code when you have a real need
3. **Refactor later** - You can always improve code when needs arise
4. **Start simple** - Begin with the simplest solution
5. **Avoid "flexibility"** - Don't build for hypothetical scenarios

### Practical Tips

- **Don't create interfaces** until you have 2+ implementations
- **Don't add methods** until you call them
- **Don't add parameters** until you use them
- **Don't build frameworks** when a simple function will do
- **Don't add "hooks"** for features you don't have
- **Start with the simplest thing** that could possibly work
- **Refactor when needed** - not before

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Less code | Only what's needed is present |
| Faster development | Don't waste time on speculation |
| Simplicity | Easy to understand, easy to maintain |
| Reduced complexity | No unnecessary abstractions |
| Better focus | Work on what matters now |
| DRY-friendly | Extract when pattern emerges |
| Cleaner codebase | No dead or unused code |

---

## DRY vs YAGNI - Understanding the Relationship

### How They Work Together

| Principle | Focus | Question |
|-----------|-------|----------|
| **DRY** | Don't duplicate existing code | "Am I repeating myself?" |
| **YAGNI** | Don't add unnecessary code | "Do I need this now?" |

### Why They Complement Each Other

**YAGNI helps DRY:**
- By waiting to implement, you see the real pattern before extracting
- Premature extraction often leads to wrong abstractions
- Waiting until you have real use cases makes extraction easier

**DRY helps YAGNI:**
- By extracting when needed, you avoid duplication
- Single source of truth is easier to modify later
- DRY code is simpler to extend when needs arise

### The Balance

```
YAGNI: Don't add code you don't need
DRY:   Don't duplicate code you do need

Together: Add code when you need it, but only once
```

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested by the user
- **DO NOT convert OOP code to procedural** unless explicitly requested by the user
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY YAGNI within the existing paradigm** - use appropriate solutions for each

### Context Matters

- If working on **procedural code**: keep it procedural, add functions as needed
- If working on **OOP code**: keep it OOP, add classes/interfaces when needed
- If working on **functional code**: keep it functional, add abstractions when needed

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **DRY (Don't Repeat Yourself):** YAGNI and DRY work together - add code when needed, only once
- **KISS (Keep It Simple, Stupid):** YAGNI supports simplicity
- **Orthogonality:** YAGNI helps maintain focused, independent components
- **The Pragmatic Programmer:** YAGNI is a core principle from this book
- **Refactoring:** YAGNI supports refactoring when needs become clear

---

## Notes/Warnings

- **YAGNI is not about skipping planning** - Plan for what you know, not what you guess
- **YAGNI is not about laziness** - It's about focused, intentional work
- **YAGNI supports DRY** - Waiting until you see the pattern leads to better extraction
- **Balance is key** - Don't under-engineer, but don't over-engineer either
- **Real-world needs > speculation** - Add code when you have a concrete use case
- **Refactoring is always available** - You can always improve later