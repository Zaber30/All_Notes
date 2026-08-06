### Anonymous Class Syntax

**Definition:** A class **without a name**, created and used **on the spot** using `new class { ... }`. Useful when you need a simple, one-time object and don't want to create a whole separate named class file for it.

php

```php
<?php
$greeter = new class {
    public string $message = "Hello!";

    public function greet(): void {
        echo $this->message . "\n";
    }
};

$greeter->greet();
```

**Output:**

```
Hello!
```

**With constructor arguments:**

php

```php
<?php
$person = new class("Alice", 25) {
    public function __construct(
        public string $name,
        public int $age
    ) {}

    public function introduce(): void {
        echo "I'm {$this->name}, {$this->age} years old.\n";
    }
};

$person->introduce();
```

**Output:**

```
I'm Alice, 25 years old.
```

---

### Use Cases

**Definition:** Anonymous classes are handy when a class is only needed **once**, in one place — avoiding cluttering your codebase with tiny named classes you'll never reuse.

#### 1. Quick one-off objects (e.g., for testing)

php

```php
<?php
function processLogger($logger): void {
    $logger->log("Processing started");
}

// No need to create a whole "TestLogger" class file just for this test
processLogger(new class {
    public function log(string $msg): void {
        echo "[TEST LOG] $msg\n";
    }
});
```

**Output:**

```
[TEST LOG] Processing started
```

#### 2. Simple implementations passed directly as arguments

php

```php
<?php
interface Comparator {
    public function compare($a, $b): int;
}

function sortItems(array $items, Comparator $comparator): array {
    usort($items, [$comparator, 'compare']);
    return $items;
}

$numbers = [5, 2, 8, 1];

$sorted = sortItems($numbers, new class implements Comparator {
    public function compare($a, $b): int {
        return $a <=> $b; // ascending order
    }
});

print_r($sorted);
```

**Output:**

```
Array
(
    [0] => 1
    [1] => 2
    [2] => 5
    [3] => 8
)
```

#### 3. Mocking/stubbing dependencies (common in testing)

Anonymous classes are frequently used to quickly fake a dependency without a full mock library.

---

### Anonymous Class with Interface

**Definition:** An anonymous class **can implement an interface** just like a normal class — useful for quickly satisfying a type-hint requirement without writing a named class.

php

```php
<?php
interface Shape {
    public function area(): float;
}

function printArea(Shape $shape): void {
    echo "Area: " . $shape->area() . "\n";
}

// Create an on-the-spot class that implements Shape
printArea(new class implements Shape {
    public function area(): float {
        return 10 * 5; // simple rectangle area
    }
});
```

**Output:**

```
Area: 50
```

**Can also extend a class:**

php

```php
<?php
abstract class Logger {
    abstract public function log(string $message): void;
}

$myLogger = new class extends Logger {
    public function log(string $message): void {
        echo "LOG: $message\n";
    }
};

$myLogger->log("System started");
```

**Output:**

```
LOG: System started
```

---

### Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|Anonymous class syntax|`new class { ... }` — a class with no name, used immediately|
|Use cases|One-off objects, quick interface implementations, simple test doubles|
|Anonymous class + interface|Can `implements`/`extends` just like a normal class|

### Rule of Thumb

> Use anonymous classes for **small, throwaway, one-time-use** objects. If the logic needs to be reused elsewhere, create a **proper named class** instead.

Want **namespaces** next, or move on to a different topic like **magic methods** (`__get`, `__set`, `__call`, etc.)?