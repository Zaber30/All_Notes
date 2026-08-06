`$this` is a **special variable** available inside a class's methods. It refers to **the current object instance** — i.e., "this specific object that the method is being called on."

It only exists inside **non-static** methods, because static methods don't belong to a specific object.

### Key Features

1. **Refers to the calling object** — lets a method access that object's own properties and other methods.
2. **Not available in static methods** — since static methods belong to the class, not an instance.
3. **Used with `->` operator** — `$this->propertyName` or `$this->methodName()`.
4. **Each object gets its own `$this`** — it's dynamic, not fixed.

```cpp
<?php

class Person {
    public string $name;
    public int $age;

    public function __construct(string $name, int $age) {
        // $this refers to the object being created
        $this->name = $name;
        $this->age = $age;
    }

    public function greet(): void {
        // $this refers to whichever object called greet()
        echo "Hi, I'm {$this->name} and I'm {$this->age} years old.\n";
    }

    public function haveBirthday(): void {
        $this->age++; // modifies THIS object's age
        echo "{$this->name} is now {$this->age}!\n";
    }
}

$alice = new Person("Alice", 25);
$bob   = new Person("Bob", 30);

$alice->greet(); // Hi, I'm Alice and I'm 25 years old.
$bob->greet();   // Hi, I'm Bob and I'm 30 years old.

$alice->haveBirthday(); // Alice is now 26!
$bob->greet();          // Bob is still 30 — untouched
```