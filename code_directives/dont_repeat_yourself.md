# DRY (Don't Repeat Yourself)

---

## Definition

Every piece of knowledge must have a single, unambiguous, authoritative representation within a system. The same information should not be duplicated in multiple places.

From "The Pragmatic Programmer": DRY is about having a single source of truth for any piece of knowledge in your system.

---

## Plain Language Translation

> "Don't repeat yourself. If you're writing the same thing more than once, extract it to one place."

**Analogy:** It's like a reference document. If everyone has their own copy and can edit it, you'll have conflicting versions. Better to have a single source of truth that everyone references.

---

## Why it matters

- **Single source of truth** - one place to change, one place to fix
- **Reduced bugs** - fix once, everywhere is updated
- **Easier maintenance** - changes only need to happen in one place
- **Consistency** - all code uses the same implementation
- **Better testing** - test one implementation, not many duplicates
- **Smaller codebase** - less duplication means less code to maintain
- **Clearer intent** - when you need to change behavior, it's obvious where

---

## Key concepts

- **Single source of truth:** One authoritative place for each piece of knowledge
- **Duplication:** Same logic, same data, or same information repeated
- **Abstraction:** Extract common patterns into reusable components
- **DRY vs WET:** DRY = Don't Repeat Yourself; WET = We Enjoy Typing (or Write Every Time)
- **Knowledge:** Facts, rules, logic, formulas, validations, queries

---

## Common violations

- Copy-paste code across multiple files
- Same validation logic repeated in different layers
- Identical queries duplicated in multiple repositories
- Hardcoded values that appear in multiple places
- Same business rule implemented differently in different modules
- Comments explaining the same thing in multiple files
- Duplicate error messages or strings

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

DRY means:
- **Methods** are extracted to avoid duplicate logic in classes
- **Base classes** or **traits** share common behavior
- **Interfaces** define contracts once
- **Constants** are defined in one place
- **Shared services** provide common functionality

### In Procedural Programming

DRY means:
- **Functions** are created for repeated logic
- **Constants** are defined at the top or in a config file
- **Include files** provide common utilities
- **Data structures** are defined once

### In Functional Programming

DRY means:
- **Pure functions** are composed to avoid duplication
- **Higher-order functions** capture common patterns
- **Constants** and **immutable data** are defined once
- **Function composition** reuses transformations

### Key Takeaway

> **The principle is the same: each piece of knowledge exists in one place.**
>
> **The implementation differs based on the paradigm:**
> - OOP: methods, classes, traits, interfaces
> - Procedural: functions, includes, constants
> - Functional: pure functions, composition, higher-order functions

---

## OOP Examples

### Java (OOP)

**Bad (duplication)**
```java
// Validation repeated in multiple places
class UserController {
    public void createUser(User user) {
        if (user.getEmail() == null || !user.getEmail().contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // create user
    }
}

class AdminController {
    public void createAdmin(User user) {
        if (user.getEmail() == null || !user.getEmail().contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
        // create admin
    }
}
```

**Good (DRY)**
```java
// Validation extracted to a single place
class UserValidator {
    public static void validate(User user) {
        if (user.getEmail() == null || !user.getEmail().contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
    }
}

class UserController {
    public void createUser(User user) {
        UserValidator.validate(user);
        // create user
    }
}

class AdminController {
    public void createAdmin(User user) {
        UserValidator.validate(user);
        // create admin
    }
}
```

### PHP (OOP)

**Bad (duplication)**
```php
// Validation repeated
class UserController {
    public function create(array $data) {
        if (empty($data['email']) || !filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException('Invalid email');
        }
        // create user
    }
}

class AdminController {
    public function create(array $data) {
        if (empty($data['email']) || !filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException('Invalid email');
        }
        // create admin
    }
}
```

**Good (DRY)**
```php
// Validation extracted to a validator class
class UserValidator {
    public static function validate(array $data): void {
        if (empty($data['email']) || !filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException('Invalid email');
        }
    }
}

class UserController {
    public function create(array $data) {
        UserValidator::validate($data);
        // create user
    }
}

class AdminController {
    public function create(array $data) {
        UserValidator::validate($data);
        // create admin
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (duplication)**
```php
<?php
// Same validation repeated
function createUser($data) {
    if (empty($data['email']) || !filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
        throw new Exception('Invalid email');
    }
    // create user
}

function updateUser($id, $data) {
    if (empty($data['email']) || !filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
        throw new Exception('Invalid email');
    }
    // update user
}
?>
```

**Good (DRY)**
```php
<?php
// Validation extracted to a function
function validateEmail(string $email): void {
    if (empty($email) || !filter_var($email, FILTER_VALIDATE_EMAIL)) {
        throw new Exception('Invalid email');
    }
}

function createUser($data) {
    validateEmail($data['email']);
    // create user
}

function updateUser($id, $data) {
    validateEmail($data['email']);
    // update user
}
?>
```

---

## How to apply

### General Steps

1. **Identify duplication** - Look for similar code patterns
2. **Extract to single place** - Create a reusable function/method/class
3. **Replace duplicates** - Update all occurrences to use the extracted unit
4. **Test once** - Verify the extracted unit works correctly
5. **Maintain single source** - Future changes go to one place only

### Practical Tips

- **Extract common logic** to utility functions/methods
- **Create shared constants** for repeated values
- **Use templates or partials** for repeated UI components
- **Define validation rules** in one place
- **Centralize queries** in repository classes
- **Use configuration files** for repeated settings

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Single source of truth | One place to change, everywhere is updated |
| Reduced bugs | Fix once, everywhere benefits |
| Easier maintenance | Changes only in one place |
| Consistency | All code uses the same implementation |
| Better testing | Test one implementation |
| Smaller codebase | Less duplication = less code |
| Clearer intent | Obvious where to make changes |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested by the user
- **DO NOT convert OOP code to procedural** unless explicitly requested by the user
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY DRY within the existing paradigm** - use appropriate abstraction for each paradigm

### Context Matters

- If working on **procedural code**: use functions and includes
- If working on **OOP code**: use methods, classes, traits
- If working on **functional code**: use pure functions and composition

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **YAGNI (You Aren't Gonna Need It):** DRY and YAGNI work together - DRY avoids duplication, YAGNI avoids unnecessary code
- **Single Responsibility (SRP):** DRY supports SRP by extracting separate responsibilities
- **Orthogonality:** DRY contributes to orthogonal, independent components
- **The Pragmatic Programmer:** DRY is a core principle from this book

---

## Notes/Warnings

- **DRY is about knowledge, not just code** - Don't repeat logic, rules, business terms, etc.
- **Don't over-DRY** - Sometimes duplication is acceptable (e.g., different contexts, performance reasons)
- **Balance with YAGNI** - Don't extract too early, wait until you see the pattern
- **Consistency matters** - Apply DRY consistently across the codebase
- **DRY + YAGNI = maintainable code** - Extract when needed, not before