# Objects and Data Structures

---

## Definition

Objects expose behavior and hide data. Data structures expose data and have no significant behavior. The key distinction is that objects are about "what" something does, while data structures are about "what" something is.

From "Clean Code" by Robert C. Martin: "Objects expose behavior and hide data. This makes it easy to add new kinds of objects without changing existing behaviors. Data structures expose data and have no significant behavior. This makes it easy to add new data structures without changing existing functions."

---

## Plain Language Translation

> "Objects are for doing. Data structures are for storing. Don't mix them up."

**Analogy:** It's like a bank. A bank account (object) has rules about deposits and withdrawals. A piece of paper with account numbers (data structure) just holds information. Each serves a different purpose.

---

## Why it matters

- **Flexibility** - Objects allow adding new behavior without changing data
- **Encapsulation** - Objects protect their internal state
- **Simplicity** - Data structures are simple and straightforward
- **Maintenance** - Changes to behavior don't affect data structures
- **Testability** - Both are easily testable in different ways

---

## Key concepts

- **Objects** - Encapsulate behavior, hide data
- **Data structures** - Expose data, no behavior
- **Procedural code** - Uses data structures
- **OOP** - Uses objects
- **Law of Demeter** - Don't expose internal structure
- **DTOs (Data Transfer Objects)** - Pure data containers

---

## Common violations

- Classes with no behavior (just getters/setters)
- Using objects as data structures (anemic domain model)
- Exposing internal data through getters
- Creating "hybrid" classes that confuse objects and data structures
- Breaking encapsulation by exposing mutable state

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Objects and data structures mean:
- **Objects:** Encapsulate behavior, hide data (preferred)
- **Data structures:** DTOs, plain objects (when needed)
- **Balance:** Use objects for behavior, data structures for data

### In Procedural Programming

Objects and data structures mean:
- **Data structures:** Simple arrays or records
- **Functions:** Operate on data structures
- **No objects** - Just data and functions that manipulate it

### In Functional Programming

Objects and data structures mean:
- **Immutable data structures** - Preferred
**- **Pure functions - Transform data, don't modify it
- **No mutable state** - Data flows through functions

### Key Takeaway

> **The principle is the same: separate behavior from data.**
>
> **The implementation differs based on the paradigm:**
> - OOP: Objects with behavior, data structures for transport
> - Procedural: Functions operate on data structures
> - Functional: Pure functions transform immutable data

---

## OOP Examples

### Java (OOP)

**Bad (hybrid - has data but behaves like object)**
```java
// This is a data structure pretending to be an object
class User {
    private String name;
    private String email;
    
    // Just getters and setters - no real behavior!
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

**Good (objects with behavior)**
```java
// Object with behavior
class User {
    private String name;
    private String email;
    
    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    // Behavior!
    public void changeEmail(String newEmail) {
        validateEmail(newEmail);
        this.email = newEmail;
    }
    
    private void validateEmail(String email) {
        // validation logic
    }
}
```

**Good (DTO - data structure for transport)**
```java
// Simple data structure for transferring data
class UserDTO {
    public String name;
    public String email;
    // No behavior, just data
}
```

### PHP (OOP)

**Bad (anemic domain model)**
```php
class User {
    private $name;
    private $email;
    
    // Just getters/setters
    public function getName() { return $this->name; }
    public function setName($name) { $this->name = $name; }
}
```

**Good (object with behavior)**
```php
class User {
    private $name;
    private $email;
    
    public function __construct(string $name, string $email) {
        $this->name = $name;
        $this->email = $email;
    }
    
    public function changeEmail(string $newEmail): void {
        $this->validateEmail($newEmail);
        $this->email = $newEmail;
    }
    
    private function validateEmail(string $email): void {
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException('Invalid email');
        }
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Good (data structure + functions)**
```php
<?php
// Data structure
$user = [
    'name' => 'John',
    'email' => 'john@example.com'
];

// Functions that operate on data structure
function createUser(array $data): array {
    validateUserData($data);
    return saveUserToDatabase($data);
}

function validateUserData(array $data): void {
    if (empty($data['email'])) {
        throw new Exception('Email required');
    }
}
?>
```

---

## How to apply

### General Rules

1. **Use objects for behavior** - Classes should do things
2. **Use data structures for data** - Simple containers for transporting data
3. **Don't expose internal data** - Keep objects' state private
4. **Follow Law of Demeter** - Don't allow train wrecks
5. **Choose the right tool** - Objects vs data structures

### When to Use Objects

- When you have behavior to encapsulate
- When you need to enforce rules
- When you want to hide implementation details
- When you need to protect state

### When to Use Data Structures

- When you just need to transport data
- When you don't need behavior
- When working with external systems (DTOs)
- When performance is critical

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Flexibility | Easy to add new behavior |
| Encapsulation | Protected internal state |
| Clarity | Clear purpose for each |
| Testability | Easy to test both |
| Maintainability | Changes are isolated |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY objects and data structures appropriately within the paradigm**

### Context Matters

- If working on **procedural code**: use data structures and functions
- If working on **OOP code**: use objects for behavior, data structures for transport
- If working on **functional code**: use immutable data structures

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Law of Demeter:** Don't expose internal structure
- **Encapsulation:** Hide data, expose behavior
- **Single Responsibility:** Objects should do one thing
- **Data Transfer Objects:** Pure data containers

---

## Notes/Warnings

- **Balance is key** - Use objects for behavior, data structures for data
- **Don't over-engineer** - Don't create objects when simple data structures work
- **DTOs are valid** - Sometimes you just need to transport data
- **Anemic domain models are a smell** - Objects should have behavior