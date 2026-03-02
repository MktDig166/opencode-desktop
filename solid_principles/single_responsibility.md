**1. [S] Single Responsibility Principle (SRP)**

---

## Definition

A class should have one and only one reason to change. It should encapsulate a single responsibility or "actor" (a group of people who request a change).

---

## Plain Language Translation

> "Each thing in its place. A class does one thing, and does it well. If it changes for more than one reason, it's wrong."

**Analogy:** It's like a gas station. The attendant fills the tank, doesn't wash the windshield. Each person has a specific role.

---

## Why it matters

- Improves **readability** - easier to understand what a class does
- Improves **testability** - easier to write unit tests for focused logic
- Improves **maintainability** - changes are isolated to specific areas
- Reduces **coupling** - less dependencies between unrelated concerns
- Enables **reusability** - smaller, focused classes can be reused

---

## Key concepts

- **Actor:** A group of stakeholders who request changes (e.g., Users, Admins, Billing Team)
- **Cohesion:** How strongly related are the responsibilities within a class
- **Coupling:** How much one class depends on others
- **Separation of Concerns:** Dividing code into distinct sections, each addressing a specific concern

---

## Common violations

- A class that validates user input AND saves to database AND sends emails
- A "God Object" that knows and does everything
- A utility class that handles parsing, formatting, AND business logic
- Mixing domain logic with infrastructure code (DB, HTTP, file I/O)

---

## Anti-patterns

- **God Class:** A massive class that knows and does too much
- **Feature Envy:** A class that uses too much of another class's data
- **Duplicated Logic:** Same responsibility scattered across multiple classes

---

## Java examples

**Bad (violates SRP)**
```java
public class UserService {
    public void registerUser(User user) {
        // validation
        if (user.getEmail() == null) throw new IllegalArgumentException();
        // persist to DB
        database.save(user);
        // send welcome email
        emailService.send(user.getEmail(), "Welcome!");
    }
}
```

**Good (SRP applied)**
```java
// Separate classes, each with one responsibility
public class User { /* data holder */ }

interface UserRepository { void save(User user); }
class JdbcUserRepository implements UserRepository { /* DB code */ }

interface EmailService { void sendWelcome(String email); }
class SmtpEmailService implements EmailService { /* email code */ }

class UserRegistrationService {
    private final UserRepository repo;
    private final EmailService email;
    
    public UserRegistrationService(UserRepository repo, EmailService email) {
        this.repo = repo;
        this.email = email;
    }
    
    public void register(User user) {
        validate(user);
        repo.save(user);
        email.sendWelcome(user.getEmail());
    }
    
    private void validate(User user) { /* validation */ }
}
```

---

## PHP examples

**Bad (violates SRP)**
```php
class UserService {
    public function registerUser($user) {
        // validation
        if (empty($user['email'])) throw new Exception('Invalid email');
        // persist to DB
        DB::table('users')->insert($user);
        // send welcome email
        Mail::send($user['email'], 'Welcome!');
    }
}
```

**Good (SRP applied)**
```php
// Separate classes, each with one responsibility
class User { /* data holder */ }

interface UserRepositoryInterface {
    public function save(User $user);
}

class EloquentUserRepository implements UserRepositoryInterface {
    public function save(User $user) { /* DB code */ }
}

interface MailerInterface {
    public function sendWelcome(string $email);
}

class SmtpMailer implements MailerInterface {
    public function sendWelcome(string $email) { /* email code */ }
}

class UserRegistrationService {
    private $repo;
    private $mailer;
    
    public function __construct(UserRepositoryInterface $repo, MailerInterface $mailer) {
        $this->repo = $repo;
        $this->mailer = $mailer;
    }
    
    public function register(User $user) {
        $this->validate($user);
        $this->repo->save($user);
        $this->mailer->sendWelcome($user->email);
    }
    
    private function validate(User $user) { /* validation */ }
}
```

---

## How to apply

1. Identify all reasons a class might change
2. If more than one reason exists, split the class
3. Name classes by what they do, not who uses them
4. Apply **Composition over Inheritance**
5. Use **Dependency Injection** to connect separate concerns

---

## Benefits

- Classes are easier to understand and maintain
- Changes are isolated and less risky
- Testing becomes simpler and more focused
- Code is more reusable
- Teams can work on different responsibilities independently

---

## Related principles

- **Open/Closed:** SRP is a prerequisite for OCP
- **Interface Segregation:** Smaller interfaces support SRP
- **Dependency Inversion:** Depends on abstractions, not concretions

---

## Notes/Warnings

- Don't over-split! Balance between SRP and complexity
- "One reason to change" doesn't mean "one method"
- Context matters - SRP in one domain may differ in another
