# `self::` vs `static::` vs `parent::` in PHP

## Quick Definitions

|Keyword|Refers to|
|---|---|
|`self::`|The class **where the code is written** (fixed at write-time)|
|`static::`|The class that was **actually called** at runtime (late static binding)|
|`parent::`|The **immediate parent class**|

## 1. `parent::` — Calling the Parent Class

**Definition:** Used inside a child class to call a method (or constructor) belonging to its parent class — usually to **extend** rather than completely replace parent behavior.

```php
<?php

class Animal {
    public string $name;

    public function __construct(string $name) {
        $this->name = $name;
    }

    public function speak(): void {
        echo "{$this->name} makes a sound.\n";
    }
}

class Dog extends Animal {
    public function __construct(string $name) {
        parent::__construct($name); // calls Animal's constructor
    }

    public function speak(): void {
        parent::speak(); // run Animal's speak() first
        echo "{$this->name} barks!\n";
    }
}

$dog = new Dog("Rex");
$dog->speak();
```

### Output

```
Rex makes a sound.
Rex barks!
```

## 2. `self::` — Fixed at Write-Time

**Definition:** Refers to the **exact class where `self::` is written**, no matter which child class calls the method. It does NOT respect inheritance/overriding.

```php
<?php

class ParentClass {
    public static function create(): void {
        echo self::class . "\n"; // always resolves to ParentClass
    }
}

class ChildClass extends ParentClass {
    // doesn't override create()
}

ParentClass::create(); // ParentClass
ChildClass::create();  // ParentClass  <-- NOT ChildClass!
```

### Output

```
ParentClass
ParentClass
```

Even though we called it via `ChildClass::create()`, `self::` still points to `ParentClass` because that's where the code was **written**.

## 3. `static::` — Late Static Binding (Runtime)

**Definition:** Refers to the class that was **actually called at runtime** — even if the method is inherited from a parent. This is called **"late static binding"**.

```php
<?php

class ParentClass {
    public static function create(): void {
        echo static::class . "\n"; // resolves at RUNTIME
    }
}

class ChildClass extends ParentClass {
    // doesn't override create()
}

ParentClass::create(); // ParentClass
ChildClass::create();  // ChildClass  <-- correctly follows the call!
```

### Output

```
ParentClass
ChildClass
```

## Side-by-Side Comparison Example

```php
<?php

class Shape {
    public static function make(): void {
        echo "self:   " . self::class . "\n";
        echo "static: " . static::class . "\n";
    }
}

class Circle extends Shape {
    // inherits make() without overriding
}

echo "Calling via Shape:\n";
Shape::make();

echo "\nCalling via Circle:\n";
Circle::make();
```

### Output

```
Calling via Shape:
self:   Shape
static: Shape

Calling via Circle:
self:   Shape
static: Circle
```

## Real-World Use Case: Factory Pattern

This is where `static::` really shines — useful for things like **ORM models** or **factory methods**:

```php
<?php

class Model {
    public static function create(): static {
        return new static(); // creates instance of the ACTUAL called class
    }

    public function whoAmI(): void {
        echo "I am " . static::class . "\n";
    }
}

class User extends Model {
}

class Product extends Model {
}

$user = User::create();       // creates a User, not a Model!
$product = Product::create(); // creates a Product, not a Model!

$user->whoAmI();     // I am User
$product->whoAmI();  // I am Product
```

### Output

```
I am User
I am Product
```

If we had used `new self()` instead of `new static()`, **both** would have incorrectly created a plain `Model` object instead of `User`/`Product`.

## Summary Table

|Feature|`self::`|`static::`|`parent::`|
|---|---|---|---|
|Resolves to|Class where written|Class actually called (runtime)|Immediate parent class|
|Respects inheritance/override?|❌ No|✅ Yes|N/A (goes up, not down)|
|Common use|Class constants, private static methods|Factory methods, `new static()`|Extending parent constructor/methods|
|Binding time|Compile-time (fixed)|Runtime (late static binding)|N/A|

## Rule of Thumb

- Use **`parent::`** when a child class needs to **extend** (not fully replace) parent behavior.
- Use **`self::`** when you specifically want to lock a reference to **this exact class**, regardless of who calls it.
- Use **`static::`** (in most modern PHP code) when you want subclasses to correctly override behavior — this is usually the **safer default** in inheritable classes.

Want to move on to **abstract classes and interfaces** next, since they build on this inheritance foundation?