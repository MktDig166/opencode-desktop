# Separation of Concerns (SoC)

---

## Definition

Separate code into distinct blocks, each with a clear and singular purpose. Each module, function, class, or layer should handle one specific aspect of the problem, and nothing more.

---

## Plain Language Translation

> "Each thing in its place. A function does one thing, a file does one thing, a module does one thing. Don't mix different responsibilities in the same place."

**Analogy:** It's like a restaurant kitchen. The Chef doesn't wash dishes. The Sous-chef doesn't serve tables. Each person has a specific role. If someone tries to do everything, the kitchen becomes a mess.

---

## Why it matters

- **Maintainability** - easier to find and fix bugs when code is organized
- **Testability** - each concern can be tested independently
- **Readability** - developers can understand code faster
- **Reusability** - isolated concerns can be reused
- **Parallel development** - teams can work on different concerns simultaneously
- **Reduces complexity** - smaller, focused pieces are easier to manage
- **Paradigm agnostic** - applies to OOP, procedural, and functional

---

## Key concepts

- **Concern:** A specific responsibility or area of interest (e.g., validation, logging, database access)
- **Modularization:** Dividing code into independent, interchangeable modules
- **Cohesion:** How strongly related the elements within a single module are
- **Coupling:** How much one module depends on others
- **Layering:** Organizing code into horizontal layers (presentation, business, data)

---

## Common violations

- Mixing business logic with database queries in the same function
- Handling HTTP requests and business rules in the same class/function
- Putting validation, transformation, and storage in one place
- Creating "God functions" that do everything
- Combining UI logic with data processing
- Mixing different paradigms without justification

---

## OOP vs Procedural/Functional - Clear Distinction

### In Object-Oriented Programming (OOP)

Separation of concerns means:
- Each **class** has a single, well-defined responsibility
- **Domain logic** is separated from **infrastructure** (DB, HTTP, etc.)
- **Interfaces** define contracts between layers
- **Single Responsibility Principle (SRP)** applies

### In Procedural Programming

Separation of concerns means:
- Each **function** does one thing and does it well
- **Data** and **functions** that operate on that data are organized together
- **Files/modules** group related functionality
- No classes or objects - just procedures and data structures

### In Functional Programming

Separation of concerns means:
- Each **pure function** has a single purpose
- **Side effects** are isolated from pure logic
- **Data transformations** are chained in a pipeline
- **Immutability** is preserved

### Key Takeaway

> **The principle is the same: separate different responsibilities into different units.**
> 
> **The implementation differs based on the paradigm:**
> - OOP: separate into classes/interfaces
> - Procedural: separate into functions/modules
> - Functional: separate into pure functions

---

## OOP Examples

### Java (OOP)

**Bad (mixing concerns)**
```java
public class UserService {
    public void registerUser(User user) {
        // validation concern
        if (user.getEmail() == null) throw new IllegalArgumentException();
        
        // database concern (infrastructure)
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost/db");
        PreparedStatement stmt = conn.prepareStatement("INSERT INTO users...");
        stmt.executeUpdate();
        
        // email concern (infrastructure)
        Properties props = new Properties();
        // ... send email
    }
}
```

**Good (separated concerns)**
```java
// Each class has ONE concern

// Domain class - represents data
public class User { /* getters/setters */ }

// Validation concern
class UserValidator {
    public void validate(User user) {
        if (user.getEmail() == null) throw new IllegalArgumentException();
    }
}

// Data access concern
interface UserRepository {
    void save(User user);
}

class JdbcUserRepository implements UserRepository {
    public void save(User user) { /* JDBC code */ }
}

// Notification concern
interface Notifier {
    void sendWelcome(String email);
}

class EmailNotifier implements Notifier {
    public void sendWelcome(String email) { /* email code */ }
}

// Orchestration only - coordinates the concerns
class UserRegistrationService {
    private final UserValidator validator;
    private final UserRepository repository;
    private final Notifier notifier;
    
    public UserRegistrationService(UserValidator v, UserRepository r, Notifier n) {
        this.validator = v;
        this.repository = r;
        this.notifier = n;
    }
    
    public void register(User user) {
        validator.validate(user);       // validation
        repository.save(user);          // persistence
        notifier.sendWelcome(user.getEmail()); // notification
    }
}
```

### PHP (OOP)

**Bad (mixing concerns)**
```php
class UserController {
    public function create($data) {
        // validation
        if (empty($data['email'])) throw new Exception('Invalid email');
        
        // database
        DB::table('users')->insert($data);
        
        // email
        Mail::send($data['email'], 'Welcome!');
        
        // response
        return response()->json(['success' => true]);
    }
}
```

**Good (separated concerns)**
```php
// Validation concern
class UserValidator {
    public function validate(array $data): void {
        if (empty($data['email'])) {
            throw new InvalidArgumentException('Invalid email');
        }
    }
}

// Data access concern
interface UserRepositoryInterface {
    public function create(array $data): User;
}

class EloquentUserRepository implements UserRepositoryInterface {
    public function create(array $data): User {
        return User::create($data);
    }
}

// Notification concern
interface MailerInterface {
    public function sendWelcome(string $email): void;
}

class SmtpMailer implements MailerInterface {
    public function sendWelcome(string $email): void {
        Mail::raw('Welcome!', function($m) use ($email) {
            $m->to($email)->subject('Welcome');
        });
    }
}

// Orchestration - just coordinates
class UserService {
    private $validator;
    private $repository;
    private $mailer;
    
    public function __construct(
        UserValidator $validator,
        UserRepositoryInterface $repository,
        MailerInterface $mailer
    ) {
        $this->validator = $validator;
        $this->repository = $repository;
        $this->mailer = $mailer;
    }
    
    public function register(array $data): User {
        $this->validator->validate($data);
        $user = $this->repository->create($data);
        $this->mailer->sendWelcome($user->email);
        
        return $user;
    }
}
```

---

## Procedural/Functional Examples

### PHP (Procedural)

**Bad (mixing concerns)**
```php
<?php
// One function does EVERYTHING
function createUser($data) {
    // Validation
    if (empty($data['email']) || empty($data['name'])) {
        return ['error' => 'Invalid data'];
    }
    
    // Database
    $conn = mysqli_connect('localhost', 'user', 'pass', 'db');
    mysqli_query($conn, "INSERT INTO users (name, email) VALUES ('{$data['name']}', '{$data['email']}')");
    
    // Logging
    file_put_contents('log.txt', "User created: {$data['email']}\n", FILE_APPEND);
    
    // Response
    return ['success' => true, 'message' => 'User created'];
}
?>
```

**Good (separated concerns - procedural)**
```php
<?php
// Each function has ONE concern

// Validation concern
function validateUserData(array $data): array {
    if (empty($data['email']) || empty($data['name'])) {
        throw new InvalidArgumentException('Invalid data');
    }
    if (!filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
        throw new InvalidArgumentException('Invalid email format');
    }
    return $data;
}

// Database concern
function saveUserToDatabase(array $data): int {
    $conn = getDbConnection();
    $stmt = $conn->prepare("INSERT INTO users (name, email) VALUES (?, ?)");
    $stmt->bind_param("ss", $data['name'], $data['email']);
    $stmt->execute();
    return $stmt->insert_id;
}

// Logging concern
function logUserCreation(string $email): void {
    $logMessage = "User created: " . $email . " at " . date('Y-m-d H:i:s') . "\n";
    file_put_contents('log.txt', $logMessage, FILE_APPEND);
}

// Response concern
function response(array $data): array {
    header('Content-Type: application/json');
    return $data;
}

// Main orchestration function
function createUser(array $data): array {
    try {
        // Call each concern in order
        $validated = validateUserData($data);
        $userId = saveUserToDatabase($validated);
        logUserCreation($validated['email']);
        
        return response(['success' => true, 'user_id' => $userId]);
    } catch (Exception $e) {
        return response(['success' => false, 'error' => $e->getMessage()]);
    }
}
?>
```

**Key differences from OOP:**
- No classes - just functions
- Data is passed explicitly between functions
- Each function has a single, clear purpose
- Organized by grouping related functions in the same file or module

---

## How to apply

### General Steps

1. **Identify concerns** - What are the different responsibilities in this code?
2. **Separate them** - Each concern gets its own function/class/module
3. **Define boundaries** - Clear interfaces between concerns
4. **Minimize dependencies** - Concerns should know as little as possible about each other
5. **Test independently** - Each concern can be tested in isolation

### For OOP

1. Create classes with single responsibilities
2. Use interfaces to define contracts
3. Separate domain logic from infrastructure
4. Use dependency injection to connect concerns

### For Procedural

1. Create functions that do one thing
2. Pass data explicitly between functions
3. Group related functions in modules/files
4. Avoid global state

### For Functional

1. Create pure functions with no side effects
2. Isolate side effects (I/O, state) to specific functions
3. Chain transformations in pipelines
4. Keep data immutable

---

## Benefits

| Benefit | OOP | Procedural | Functional |
|---------|-----|------------|------------|
| Easier maintenance | ✅ | ✅ | ✅ |
| Better testability | ✅ | ✅ | ✅ |
| Clear code organization | ✅ | ✅ | ✅ |
| Reduced bugs | ✅ | ✅ | ✅ |
| Parallel development | ✅ | ✅ | ✅ |
| Reusability | ✅ | ✅ | ✅ |

---

## CRITICAL: No Paradigm Mixing

> **⚠️ VERY IMPORTANT: Read carefully**

### Rules

- **DO NOT convert procedural code to OOP** unless explicitly requested by the user
- **DO NOT convert OOP code to procedural** unless explicitly requested by the user
- **DO NOT mix paradigms** in the same codebase without explicit approval from the user
- **DO NOT add classes to procedural codebases** unless explicitly requested
- **DO NOT rewrite procedural functions as classes** in OOP codebases unless explicitly requested

### Context Matters

- If working on **procedural code**: keep it procedural
- If working on **OOP code**: keep it OOP  
- If working on **functional code**: keep it functional

### When to Ask for Permission

If you identify that a significant refactor is needed to change the paradigm, you MUST:

1. **STOP** - Do not make changes without approval
2. **EXPLAIN** - Describe what you found and why a paradigm change would be needed
3. **ASK** - Request explicit permission to proceed with the refactor
4. **WAIT** - Do not proceed until you have confirmation

### Why This Matters

Mixing paradigms without proper consideration leads to:
- Inconsistent codebases
- Confusion for developers
- Maintenance nightmares
- Architectural conflicts

### Example of What NOT To Do

```
User: "Fix this PHP function"
AI: *rewrites entire codebase in classes* ❌

User: "Add feature to this procedural module"
AI: *converts to OOP patterns* ❌
```

Instead:
```
User: "Fix this PHP function"
AI: "I'll fix the function while keeping it procedural. Should I also consider refactoring to OOP for better maintainability? Please confirm." ✅
```

---

## Related principles

- **SRP (Solid):** Single Responsibility is SoC applied to classes (OOP only)
- **Modularization:** Organizing code into independent modules
- **Layered Architecture:** Separating into presentation, business, data layers
- **Clean Architecture:** SoC at the architectural level

---

## Notes/Warnings

- SoC is a **universal principle** - it applies to ALL programming paradigms
- The implementation differs, but the concept remains the same
- Balance is key - don't over-modularize to the point of complexity
- Consider the existing codebase paradigm before making changes
- Always match the paradigm of the existing code