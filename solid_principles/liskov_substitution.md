# **3. [L] Liskov Substitution Principle (LSP)**

---

## Definition

Objects of a superclass should be replaceable with objects of its subclasses **without breaking the application**. Subtypes must maintain the behavior expected by the base type.

---

## Plain Language Translation

> "If you say something is a type of another thing, it must work like it. A square says it's a rectangle? Then it must behave like a rectangle, no surprises."

**Analogy:** It's like renting a car. If you pay for a sedan, you can't receive a truck. The promise is "car", and every car must work like a car.

---

## Why it matters

- **Guarantees polymorphism works safely** - code using base type works with any subtype
- **Prevents hidden bugs** - unexpected behaviors are avoided
- **Maintains contracts** - expectations are preserved
- **Enables reuse** - base class code works with all subclasses
- **Builds trust** - subtypes are predictable

---

## Key concepts

- **Substitutability:** A subtype can stand in for its supertype
- **Preconditions:** Conditions that must be true before a method runs (subtypes cannot strengthen)
- **Postconditions:** Conditions that must be true after a method runs (subtypes cannot weaken)
- **Invariants:** Conditions that must remain true throughout the object's life (subtypes must preserve)
- **Contract:** The set of expectations (preconditions, postconditions, invariants)

---

## Common violations

- Subclass that changes behavior in unexpected ways
- Overriding a method to do nothing or throw an exception
- Changing parameter types or return types in ways that break compatibility
- Strengthening preconditions (requiring more than the parent)
- Weakening postconditions (promising less than the parent)

---

## Anti-patterns

- **Circle-Ellipse Problem:** Treating a circle as a stretched ellipse
- **Square-Rectangle Paradox:** Square changing width affects height
- **Throwing Exceptions:** Overriding methods to throw "not supported"
- **Empty Implementations:** Leaving methods empty in subclasses

---

## Java examples

**Bad (violates LSP)**
```java
class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int w) { width = w; }
    public void setHeight(int h) { height = h; }
    public int area() { return width * height; }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int w) {
        width = w;
        height = w; // Square: width = height always!
    }
    
    @Override
    public void setHeight(int h) {
        width = h; // Breaks Rectangle expectations!
        height = h;
    }
}

// Problem: Code expecting Rectangle behavior breaks with Square
void increaseWidth(Rectangle r) {
    r.setWidth(r.getWidth() + 1);
    // For Rectangle: area changes
    // For Square: both dimensions change unexpectedly
}
```

**Good (LSP applied)**
```java
abstract class Shape {
    public abstract int area();
}

class Rectangle extends Shape {
    private int width;
    private int height;
    
    public Rectangle(int w, int h) { width = w; height = h; }
    public int area() { return width * height; }
}

class Square extends Shape {
    private int side;
    
    public Square(int s) { side = s; }
    public int area() { return side * side; }
}

// Both work correctly - no surprising behavior
void printArea(Shape s) {
    System.out.println(s.area()); // Works for any Shape
}
```

---

## PHP examples

**Bad (violates LSP)**
```php
class Rectangle {
    protected $width;
    protected $height;
    
    public function setWidth(int $w) { $this->width = $w; }
    public function setHeight(int $h) { $this->height = $h; }
    public function area() { return $this->width * $this->height; }
}

class Square extends Rectangle {
    public function setWidth(int $w) {
        $this->width = $w;
        $this->height = $w; // Forces equal sides!
    }
    
    public function setHeight(int $h) {
        $this->width = $h; // Breaks Rectangle contract!
        $this->height = $h;
    }
}
```

**Good (LSP applied)**
```php
interface ShapeInterface {
    public function area(): int;
}

class Rectangle implements ShapeInterface {
    private $width;
    private $height;
    
    public function __construct(int $w, int $h) {
        $this->width = $w;
        $this->height = $h;
    }
    
    public function area(): int {
        return $this->width * $this->height;
    }
}

class Square implements ShapeInterface {
    private $side;
    
    public function __construct(int $side) {
        $this->side = $side;
    }
    
    public function area(): int {
        return $this->side * $this->side;
    }
}

// Both work correctly
function printArea(ShapeInterface $shape) {
    echo $shape->area();
}
```

---

## How to apply

1. **Follow the "is-a" test** - Is a Square really a Rectangle?
2. **Don't override to do less** - Subclass capabilities should match parent
3. **Preserve invariants** - Keep the rules the parent establishes
4. **Don't strengthen preconditions** - Accept what parent accepts
5. **Don't weaken postconditions** - Promise at least what parent promises
6. Use **composition** over inheritance when behavior differs

---

## Benefits

- Safe polymorphism
- Predictable code behavior
- Fewer hidden bugs
- Easier testing
- Reliable inheritance hierarchies

---

## Related principles

- **Open/Closed:** LSP enables OCP
- **Interface Segregation:** Smaller interfaces make LSP easier
- **Dependency Inversion:** Depends on contracts, not implementations

---

## Notes/Warnings

- LSP violations often come from forcing "is-a" relationships that don't fit
- Composition is often better than inheritance when behavior differs
- "Works correctly" means substitutability without changing behavior