# Error Handling

---

## Definition

Error handling is about managing unexpected situations in code. Good error handling makes programs robust and user-friendly without cluttering the code with excessive checking. Use exceptions instead of error codes, provide context with exceptions, and handle errors at the appropriate level.

From "Clean Code" by Robert C. Martin: "Error handling is important, but if it obscures logic, it's wrong."

---

## Plain Language Translation

> "Handle errors clearly. Don't clutter code with error checks. Use exceptions, not error codes."

**Analogy:** It's like a hotel. When something goes wrong (no rooms available), the hotel doesn't give you a code. It tells you clearly: "We're sorry, we're fully booked."

---

## Why it matters

- **Clarity** - Code is easier to read
- **Robustness** - Programs handle unexpected situations
- **User experience** - Users get clear feedback
- **Maintainability** - Errors are easy to find and fix
- **Separation** - Business logic separate from error handling
- **Debugging** - Easier to find root causes

---

## Key concepts

- **Exceptions** - Preferred over error codes
- **Context** - Provide meaningful error messages
- **Don't return null** - Avoid null pointer exceptions
- **Don't pass null** - Prevent null problems
- **Error handling** - Separate from business logic
- **Fail fast** - Detect errors early

---

## Common violations

- Returning error codes instead of throwing exceptions
- Catching exceptions and doing nothing
- Returning null instead of throwing exceptions
- Passing null without validation
- Catching generic Exception/Throwable
- Error messages without context
- Mixing error handling with business logic

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Error handling means:
- **Try-catch blocks** for exception handling
- **Custom exceptions** for specific error types
- **Exception hierarchies** for organized error handling

### In Procedural Programming

Error handling means:
- **Return codes** or **exceptions**
- **Error checking** after function calls
- **Global error handlers**

### In Functional Programming

Error handling means:
- **Either/Result types** (functional approach)
- **Maybe/Monads** for nullable values
- **Functional error propagation**

### Key Takeaway

> **The principle is the same: handle errors clearly and appropriately.**
>
> **The implementation differs based on the paradigm.**

---

## OOP Examples

### Java (OOP)

**Bad (error codes)**
```java
public int saveUser(User user) {
    if (user == null) {
        return -1;  // What does -1 mean?
    }
    if (!isValid(user)) {
        return -2;  // What does -2 mean?
    }
    return database.save(user);
}
```

**Good (exceptions)**
```java
public void saveUser(User user) {
    if (user == null) {
        throw new IllegalArgumentException("User cannot be null");
    }
    if (!isValid(user)) {
        throw new ValidationException("User validation failed: " + getValidationErrors(user));
    }
    repository.save(user);
}
```

**Bad (catching everything)**
```java
try {
    doSomething();
} catch (Exception e) {
    // Do nothing - bad!
}
```

**Good (specific exceptions)**
```java
try {
    file.write(data);
} catch (IOException e) {
    throw new StorageException("Failed to write to file: " + file.getName(), e);
}
```

### PHP (OOP)

**Bad (returning null)**
```php
public function findUser($id) {
    $result = $this->db->query("SELECT * FROM users WHERE id = ?", [$id]);
    if (empty($result)) {
        return null;  // Now caller must check for null!
    }
    return $result[0];
}
```

**Good (exceptions or optional)**
```php
public function findUser(int $id): ?User {
    $result = $this->repository->find($id);
    if ($result === null) {
        throw new UserNotFoundException("User with ID $id not found");
    }
    return $result;
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Good (exceptions in procedural)**
```php
<?php
function divide(float $a, float $b): float {
    if ($b == 0) {
        throw new DivisionByZeroException("Cannot divide by zero");
    }
    return $a / $b;
}

try {
    $result = divide(10, 0);
} catch (DivisionByZeroException $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

---

## How to apply

### General Rules

1. **Use exceptions** - Not error codes
2. **Provide context** - Meaningful error messages
3. **Don't return null** - Throw exceptions instead
4. **Don't pass null** - Validate inputs
5. **Catch specific exceptions** - Not generic Exception
6. **Separate concerns** - Keep error handling separate from logic

### Best Practices

- **Fail fast** - Validate early
- **Wrap exceptions** - Add context when rethrowing
- **Log appropriately** - Don't log and rethrow
- **Use custom exceptions** - For specific error types

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Cleaner code | Less error checking clutter |
| Better UX | Clear error messages |
| Easier debugging | Clear error origins |
| Maintainability | Organized error handling |
| Robustness | Handles unexpected situations |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY error handling best practices within the existing paradigm**

### Context Matters

- If working on **procedural code**: handle errors appropriately
- If working on **OOP code**: use exceptions properly
- If working on **functional code**: consider functional error handling

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **KISS:** Simple error handling
- **Fail Fast:** Detect errors early
- **Null Handling:** Don't return/pass null

---

## Notes/Warnings

- **Exceptions are for exceptional cases** - Don't overuse them
- **Provide context** - Error messages should help debug
- **Don't swallow exceptions** - At least log them
- **Validate inputs** - Fail fast with clear messages