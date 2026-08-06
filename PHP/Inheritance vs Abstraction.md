## Inheritance in PHP

### Extending Classes

**Definition:** A class can inherit properties and methods from another class using `extends`. The child class gets everything the parent has, plus can add its own.

php

```php
<?php
class Animal {
    public string $name;
    public function __construct(string $name) {
        $this->name = $name;
    }
    public function eat(): void {
        echo "{$this->name} is eating.\n";
    }
}

class Dog extends Animal {
    public function bark(): void {
        echo "{$this->name} is barking.\n";
    }
}

$dog = new Dog("Rex");
$dog->eat();  // inherited from Animal
$dog->bark(); // Dog's own method
```

**Output:**

```
Rex is eating.
Rex is barking.
```

---

### Method Overriding

**Definition:** A child class can **redefine** a parent method with its own version — same name, new behavior.

php

```php
<?php
class Animal {
    public function speak(): void {
        echo "Some generic animal sound\n";
    }
}

class Cat extends Animal {
    public function speak(): void {
        echo "Meow!\n"; // overrides parent's version
    }
}

$cat = new Cat();
$cat->speak(); // Meow!
```

**Output:**

```
Meow!
```

---

### `parent::` Calls

**Definition:** Used inside an overriding method to still **call the parent's version** of that method, instead of fully replacing it.

php

```php
<?php
class Animal {
    public function speak(): void {
        echo "Generic sound... ";
    }
}

class Cat extends Animal {
    public function speak(): void {
        parent::speak(); // calls Animal's speak() first
        echo "Meow!\n";
    }
}

(new Cat())->speak();
```

**Output:**

```
Generic sound... Meow!
```

---

### `final` Keyword — Classes and Methods ⭐

**Definition:** `final` **prevents** further inheritance or overriding.

- `final class` → cannot be extended at all
- `final method` → cannot be overridden by child classes

**Feature:** Used to lock down critical behavior/security so it can't be changed later.

php

```php
<?php
class Payment {
    final public function process(): void {
        echo "Processing payment securely.\n";
    }
}

class OnlinePayment extends Payment {
    // public function process(): void {} // ❌ Error! Cannot override final method
}

final class Config {
    // settings here
}

// class MyConfig extends Config {} // ❌ Error! Cannot extend final class
```

---

### Abstract Classes ⭐

**Definition:** A class marked `abstract` **cannot be instantiated directly** — it only exists to be extended. It acts as a **template/blueprint** for child classes.

**Feature:** Can contain both normal methods (with code) AND abstract methods (no code, just a signature).

#### Abstract Methods

**Definition:** A method declared **without a body** inside an abstract class. Any child class that extends it **must** implement (fill in) that method.

php

```php
<?php
abstract class Shape {
    // Abstract method - no body, just a signature
    abstract public function area(): float;

    // Normal method - has a body, works like usual
    public function describe(): void {
        echo "This shape's area is " . $this->area() . "\n";
    }
}

class Circle extends Shape {
    public function __construct(private float $radius) {}

    // MUST implement this - it's required!
    public function area(): float {
        return pi() * $this->radius ** 2;
    }
}

$circle = new Circle(5);
$circle->describe();
```

**Output:**

```
This shape's area is 78.539816339745
```

#### Cannot Instantiate Abstract Classes

php

```php
<?php
abstract class Shape {
    abstract public function area(): float;
}

// $shape = new Shape(); // ❌ Fatal Error: Cannot instantiate abstract class
```

---

### Constructor Inheritance

**Definition:** If a child class does **not** define its own `__construct()`, it automatically **inherits the parent's constructor**. If it **does** define one, it **replaces** the parent's — unless you call `parent::__construct()` inside it.

php

```php
<?php
class Animal {
    public string $name;
    public function __construct(string $name) {
        $this->name = $name;
        echo "Animal constructor: {$this->name}\n";
    }
}

// Case 1: No own constructor -> inherits parent's automatically
class Dog extends Animal {
}
$dog = new Dog("Rex"); // uses Animal's constructor
```

**Output:**

```
Animal constructor: Rex
```

php

```php
<?php
// Case 2: Own constructor -> must call parent explicitly to reuse it
class Cat extends Animal {
    public string $color;
    public function __construct(string $name, string $color) {
        parent::__construct($name); // reuse parent logic
        $this->color = $color;
        echo "Cat constructor: {$this->color}\n";
    }
}
$cat = new Cat("Whiskers", "black");
```

**Output:**

```
Animal constructor: Whiskers
Cat constructor: black
```

---

### Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|`extends`|Child class inherits parent's properties/methods|
|Method overriding|Child redefines a parent method|
|`parent::`|Calls the parent's version of a method/constructor|
|`final` class/method|Blocks further extending/overriding|
|`abstract` class|Template class — cannot create objects from it directly|
|Abstract method|Must be implemented by every child class|
|Constructor inheritance|Child auto-inherits parent's constructor unless it defines its own|

Want **interfaces** next, since they naturally follow abstract classes?