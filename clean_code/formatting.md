# Code Formatting

---

## Definition

Code formatting is about presenting code in a consistent, readable manner. Good formatting improves readability, reduces cognitive load, and makes the codebase easier to maintain. Formatting should be consistent and follow established conventions.

From "Clean Code" by Robert C. Martin: "Coding is a social activity. The formatting of code communicates to other developers (and to your future self) about the structure and organization of the codebase."

---

## Plain Language Translation

> "Code should look like it was written by one person. Consistent formatting makes code easy to read and understand."

**Analogy:** It's like a well-organized book. Consistent margins, spacing, and structure make it easy to read. Code formatting works the same way.

---

## Why it matters

- **Readability** - Easy to read and understand
- **Consistency** - One style across the codebase
- **Collaboration** - Team members can read each other's code
- **Maintainability** - Easy to find and fix issues
- **Professionalism** - Clean code looks professional
- **Reduced bugs** - Clear structure helps spot errors

---

## Key concepts

- **Vertical formatting** - How code is organized top to bottom
- **Horizontal formatting** - How code is organized left to right
- **Blank lines** - Separate concepts
- **Indentation** - Show structure
- **Line length** - Keep lines manageable (80-120 characters)
- **Whitespace** - Use spaces for readability
- **Consistency** - Same style everywhere

---

## Common violations

- Inconsistent indentation (spaces vs tabs, different counts)
- Lines that are too long (requires horizontal scrolling)
- No blank lines between concepts
- Inconsistent spacing around operators
- Mixing styles (camelCase with snake_case)
- Code that looks "messy" or "dense"
- No structure to class/function organization

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Code formatting means:
- **Vertical spacing:** Blank lines between methods, between concepts
- **Horizontal spacing:** Consistent indentation, spacing around operators
- **Class organization:** Variables at top, constructors, public methods, private methods
- **Import statements:** Organized alphabetically or by type

### In Procedural Programming

Code formatting means:
- **Function organization:** Functions defined in logical order
- **Vertical spacing:** Blank lines between functions, between concepts
- **Horizontal spacing:** Consistent indentation
- **Include/require organization:** At the top of files

### In Functional Programming

Code formatting means:
- **Function composition:** Chained functions clearly formatted
- **Vertical spacing:** Blank lines between function definitions
- **Indentation:** For nested transformations
- **Type annotations:** Clearly formatted

### Key Takeaway

> **The principle is the same: consistent, readable formatting.**
>
> **The implementation differs slightly based on conventions of each language and paradigm.**

---

## OOP Examples

### Java (OOP)

**Bad (inconsistent formatting)**
```java
public class UserService{
private UserRepository repo;
public UserService(UserRepository r){this.repo=r;}
public User getUser(int id){
return repo.findById(id);}
public void saveUser(User u){
repo.save(u);}
}
```

**Good (consistent formatting)**
```java
public class UserService {
    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }

    public User getUser(int id) {
        return repository.findById(id);
    }

    public void saveUser(User user) {
        repository.save(user);
    }
}
```

### PHP (OOP)

**Bad (inconsistent formatting)**
```php
class OrderService {
private $repo;
function __construct($r){$this->repo=$r;}
public function process($o){return $this->repo->save($o);}
}
```

**Good (consistent formatting)**
```php
class OrderService {
    private $repository;

    public function __construct(OrderRepository $repository) {
        $this->repository = $repository;
    }

    public function processOrder(Order $order): Order {
        return $this->repository->save($order);
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (inconsistent formatting)**
```php
<?php
function calculateTotal($items,$tax){
$total=0;
foreach($items as $item){
$total+=$item['price']*$item['quantity'];}
return $total*(1+$tax);
}
function saveToDb($data){
echo" saving "; }
?>
```

**Good (consistent formatting)**
```php
<?php

function calculateTotal(array $items, float $tax): float {
    $total = 0;
    foreach ($items as $item) {
        $total += $item['price'] * $item['quantity'];
    }
    return $total * (1 + $tax);
}

function saveToDatabase(array $data): void {
    echo "saving";
}

?>
```

---

## How to apply

### General Rules

1. **Use a code formatter** - Use tools like Prettier, PHP CS Fixer, etc.
2. **Configure your IDE** - Set up automatic formatting
3. **Use consistent indentation** - Spaces or tabs (pick one, be consistent)
4. **Keep lines short** - 80-120 characters maximum
5. **Add blank lines** - Separate concepts vertically
6. **Organize imports/includes** - Alphabetically or by type
7. **Follow language conventions** - Respect community standards

### Vertical Formatting

- **Blank lines** between concepts
- **Related code** together
- **Dependencies** close to where used
- **Functions** in logical order

### Horizontal Formatting

- **Indentation** shows structure
- **Spaces** around operators
- **Align** related assignments
- **Line length** - Keep manageable

---

## Benefits

| Benefit | Description |
|---------|-------------|
| Readability | Easy to scan and understand |
| Consistency | Same style everywhere |
| Collaboration | Team can read each other's code |
| Maintainability | Easy to find and fix issues |
| Professionalism | Clean, polished appearance |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested
- **DO NOT convert OOP code to procedural** unless explicitly requested
- **DO NOT mix paradigms** in the same codebase without explicit approval
- **APPLY formatting within the existing paradigm and language conventions**

### Context Matters

- If working on **procedural code**: use PHP conventions
- If working on **OOP code**: use language-specific conventions (Java/PHP)
- If working on **functional code**: use functional conventions

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

---

## Related principles

- **Meaningful Names:** Good formatting supports readability
- **Small Functions:** Good formatting shows function structure
- **Comments:** Less needed when code is well-formatted
- **KISS:** Simple formatting is easier to read

---

## Notes/Warnings

- **Use automated formatters** - Don't waste time formatting manually
- **Be consistent** - Whatever style you pick, use it everywhere
- **Follow conventions** - Respect community standards for your language
- **Configure your IDE** - Automatic formatting saves time