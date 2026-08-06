### Trait Definition and Usage

**Definition:** A **trait** is a way to **reuse code (methods)** across multiple unrelated classes — solving the problem that a class can only `extends` **one** parent. Traits are included using `use`.

php

```php
<?php
trait Greetable {
    public function greet(): void {
        echo "Hello, I'm {$this->name}\n";
    }
}

class Person {
    use Greetable;
    public function __construct(public string $name) {}
}

class Robot {
    use Greetable; // same trait, unrelated class
    public function __construct(public string $name) {}
}

(new Person("Alice"))->greet();
(new Robot("R2D2"))->greet();
```

**Output:**

```
Hello, I'm Alice
Hello, I'm R2D2
```

---

### Conflict Resolution `insteadof`, `as`

**Definition:** If a class uses **two traits with the same method name**, PHP can't decide which to use — this causes a conflict. You resolve it using `insteadof` (pick one) and `as` (create an alias for the other).

php

```php
<?php
trait English {
    public function greet(): void { echo "Hello!\n"; }
}

trait Spanish {
    public function greet(): void { echo "Hola!\n"; }
}

class Person {
    use English, Spanish {
        English::greet insteadof Spanish; // use English's version
        Spanish::greet as greetSpanish;    // keep Spanish's version under new name
    }
}

$p = new Person();
$p->greet();        // Hello!
$p->greetSpanish();  // Hola!
```

**Output:**

```
Hello!
Hola!
```

---

### Abstract Trait Methods

**Definition:** A trait can declare an **abstract method** — no body — forcing any class that uses the trait to implement it.

php

```php
<?php
trait Comparable {
    abstract public function getValue(): int;

    public function isGreaterThan(Comparable $other): bool {
        return $this->getValue() > $other->getValue();
    }
}

class Money {
    use Comparable;
    public function __construct(private int $amount) {}

    public function getValue(): int {
        return $this->amount; // required implementation
    }
}

$a = new Money(100);
$b = new Money(50);
var_dump($a->isGreaterThan($b));
```

**Output:**

```
bool(true)
```

---

### Static Methods in Traits

**Definition:** Traits can also include **static** properties/methods, which behave the same as static members defined directly in a class.

php

```php
<?php
trait Counter {
    private static int $count = 0;

    public static function increment(): int {
        return ++self::$count;
    }
}

class PageA {
    use Counter;
}

class PageB {
    use Counter; // gets its OWN separate static counter
}

echo PageA::increment() . "\n"; // 1
echo PageA::increment() . "\n"; // 2
echo PageB::increment() . "\n"; // 1 (independent from PageA)
```

**Output:**

```
1
2
1
```

---

### Trait Constants (PHP 8.2)

**Definition:** Since PHP 8.2, traits can define **constants** directly, which become available to any class using the trait.

php

```php
<?php
trait HasVersion {
    const VERSION = "1.0.0"; // PHP 8.2+ feature

    public function showVersion(): void {
        echo "Version: " . self::VERSION . "\n";
    }
}

class App {
    use HasVersion;
}

(new App())->showVersion();
echo App::VERSION . "\n";
```

**Output:**

```
Version: 1.0.0
1.0.0
```

---

### Trait vs Multiple Inheritance

|Feature|Traits|True Multiple Inheritance (not in PHP)|
|---|---|---|
|Share code from many sources|✅ Yes, via `use Trait1, Trait2;`|Would need `extends Class1, Class2` (PHP doesn't allow this)|
|Conflict handling|✅ Explicit `insteadof` / `as`|Ambiguous / language-dependent|
|Can traits be instantiated?|❌ No (`new Trait()` is illegal)|N/A|
|Purpose|Reuse **implementation** across unrelated classes|Reuse **implementation + identity**|
|PHP's actual solution|**Traits + Interfaces** together|PHP intentionally avoids true multiple inheritance for classes|

php

```php
<?php
// PHP simulates "multiple inheritance" using traits + interfaces
trait CanFly {
    public function fly(): void { echo "Flying\n"; }
}

trait CanSwim {
    public function swim(): void { echo "Swimming\n"; }
}

class Duck {
    use CanFly, CanSwim; // gets behavior from BOTH
}

$duck = new Duck();
$duck->fly();
$duck->swim();
```

**Output:**

```
Flying
Swimming
```

---

### Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|Trait|Reusable block of methods/properties for unrelated classes|
|`insteadof` / `as`|Resolve method name conflicts between multiple traits|
|Abstract trait methods|Force using-class to implement a specific method|
|Static methods in traits|Each using-class gets its own independent static state|
|Trait constants (8.2+)|Constants defined in a trait, usable by the class|
|Trait vs multiple inheritance|Traits = PHP's safe alternative to true multiple inheritance|

Want **namespaces** next, since large projects with many traits/classes usually need them to avoid name collisions?