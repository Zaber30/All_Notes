## Interfaces in PHP

### Interface Definition and Implementation

**Definition:** An **interface** is a contract that defines **method signatures only** (no bodies). Any class that `implements` it **must** provide actual code for every method listed.

```php
<?php
interface Flyable {
    public function fly(): void; // no body, just signature
}

class Bird implements Flyable {
    public function fly(): void {
        echo "Bird is flying.\n";
    }
}

$bird = new Bird();
$bird->fly();
```

**Output:**

```
Bird is flying.
```

---

### Implementing Multiple Interfaces ⭐

**Definition:** Unlike classes (which can only `extends` **one** parent), a class **can implement multiple interfaces** at once, separated by commas.


```php
<?php
interface Flyable {
    public function fly(): void;
}

interface Swimmable {
    public function swim(): void;
}

class Duck implements Flyable, Swimmable {
    public function fly(): void {
        echo "Duck is flying.\n";
    }
    public function swim(): void {
        echo "Duck is swimming.\n";
    }
}

$duck = new Duck();
$duck->fly();
$duck->swim();
```

**Output:**

```
Duck is flying.
Duck is swimming.
```

---

### Interface Constants

**Definition:** Interfaces can define **constants** that all implementing classes automatically get. They cannot be changed by the implementing class.


```php
<?php
interface Shape {
    const UNIT = "cm"; // interface constant

    public function area(): float;
}

class Square implements Shape {
    public function __construct(private float $side) {}

    public function area(): float {
        return $this->side ** 2;
    }
}

$sq = new Square(5);
echo $sq->area() . " sq " . Square::UNIT . "\n";
```

**Output:**

```
25 sq cm
```

---

### Interface Type-Hinting

**Definition:** You can use an interface as a **type hint** in function/method parameters, so the function accepts **any object** that implements it — regardless of the actual class.


```php
<?php
interface Flyable {
    public function fly(): void;
}

class Bird implements Flyable {
    public function fly(): void { echo "Bird flying\n"; }
}

class Airplane implements Flyable {
    public function fly(): void { echo "Airplane flying\n"; }
}

// Accepts ANY object that implements Flyable
function makeItFly(Flyable $thing): void {
    $thing->fly();
}

makeItFly(new Bird());     // Bird flying
makeItFly(new Airplane()); // Airplane flying
```

**Output:**

```
Bird flying
Airplane flying
```

---

### Interface vs Abstract Class ⭐

|Feature|Interface|Abstract Class|
|---|---|---|
|Method bodies|❌ Never (all abstract)|✅ Can mix normal + abstract methods|
|Properties|❌ No (constants only)|✅ Yes|
|Multiple inheritance|✅ Class can implement many|❌ Class can extend only one|
|Constructor|❌ No|✅ Yes|
|Use when|Unrelated classes need a **shared capability** (e.g., `Flyable`)|Related classes share **common code + structure**|

php

```php
<?php
// Interface: "CAN DO" - a capability, no shared code
interface Flyable {
    public function fly(): void;
}

// Abstract class: "IS A" - shared identity + shared code
abstract class Animal {
    public string $name;
    public function __construct(string $name) {
        $this->name = $name;
    }
    abstract public function makeSound(): void;
}

class Parrot extends Animal implements Flyable {
    public function makeSound(): void {
        echo "{$this->name} says: Squawk!\n";
    }
    public function fly(): void {
        echo "{$this->name} is flying.\n";
    }
}

$parrot = new Parrot("Polly");
$parrot->makeSound();
$parrot->fly();
```

**Output:**

```
Polly says: Squawk!
Polly is flying.
```

---

### Marker Interfaces

**Definition:** An interface with **no methods at all** — it's used purely to **"tag" or label** a class as having a certain property, checked later using `instanceof`. No code is forced; it's just identification.

php

```php
<?php
// Empty interface - just a marker/tag
interface Cacheable {
}

class Report implements Cacheable {
    public string $title = "Sales Report";
}

class TempFile {
    // does NOT implement Cacheable
}

function saveIfCacheable(object $item): void {
    if ($item instanceof Cacheable) {
        echo "Caching: " . get_class($item) . "\n";
    } else {
        echo "Not cacheable: " . get_class($item) . "\n";
    }
}

saveIfCacheable(new Report());   // tagged as Cacheable
saveIfCacheable(new TempFile()); // not tagged
```

**Output:**

```
Caching: Report
Not cacheable: TempFile
```

**Real-world example:** PHP's own `Serializable`-style marker patterns, or `JsonSerializable` (though that one does have a method) — the idea is used a lot for permission/capability tagging in frameworks.

---

### Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|Interface|Contract of method signatures a class must implement|
|Multiple interfaces|A class can implement many interfaces at once|
|Interface constants|Shared fixed values available to all implementing classes|
|Interface type-hinting|Accept any object matching a capability, regardless of class|
|Interface vs Abstract|Interface = capability (no code); Abstract = shared identity (with code)|
|Marker interface|Empty interface used only to "tag" a class for `instanceof` checks|

Want **traits** next, since they solve the "share code across unrelated classes" problem that interfaces can't?