# Late Static Binding (`static::`)

## Definition

**Late static binding** is a PHP feature that lets `static::` refer to the class that was **actually called at runtime**, instead of the class where the method was originally **defined**.

The word "late" means the resolution happens **late** — at the moment of the call — rather than "early" (at compile-time, like `self::` does).

## The Problem It Solves

Without late static binding, `self::` always points to the class where the code is **written**, even if a child class calls it. This breaks inheritance in certain patterns.

```php
<?php

class ParentClass {
    public static function create(): self {
        return new self(); // "self" = hardcoded to ParentClass
    }
}

class ChildClass extends ParentClass {
}

$obj = ChildClass::create();
echo get_class($obj) . "\n"; // ParentClass  <-- WRONG! We wanted ChildClass
```

### Output

```
ParentClass
```

Even though we called `ChildClass::create()`, we got a `ParentClass` object back — because `self` was locked to `ParentClass` at write-time.

## The Fix: `static::`

```php
<?php

class ParentClass {
    public static function create(): static {
        return new static(); // "static" = resolved at RUNTIME
    }
}

class ChildClass extends ParentClass {
}

$obj = ChildClass::create();
echo get_class($obj) . "\n"; // ChildClass  <-- Correct!
```

### Output

```
ChildClass
```

Now `static` correctly resolves to whichever class was actually called.

## Key Features

1. **Resolved at runtime**, based on the calling class — not where the code is defined.
2. Works with `new static()`, `static::method()`, and `static::CONSTANT`.
3. **Propagates down** the inheritance chain — grandchildren classes get it too.
4. Only matters in **inherited static context** — if you never subclass, `self::` and `static::` behave identically.

## Full Example: Constants + Methods

```php
<?php

class Vehicle {
    const NAME = "Vehicle";

    public static function selfName(): string {
        return self::NAME; // fixed to Vehicle, always
    }

    public static function staticName(): string {
        return static::NAME; // resolves based on actual called class
    }
}

class Car extends Vehicle {
    const NAME = "Car"; // overrides the constant
}

class SportsCar extends Car {
    const NAME = "SportsCar"; // overrides again
}

echo Vehicle::selfName()   . "\n"; // Vehicle
echo Car::selfName()       . "\n"; // Vehicle  <-- self:: ignores override
echo SportsCar::selfName() . "\n"; // Vehicle  <-- still ignores override

echo Vehicle::staticName()   . "\n"; // Vehicle
echo Car::staticName()       . "\n"; // Car        <-- static:: respects override
echo SportsCar::staticName() . "\n"; // SportsCar  <-- propagates all the way down
```

### Output

```
Vehicle
Vehicle
Vehicle
Vehicle
Car
SportsCar
```

This clearly shows late static binding **carrying through multiple levels** of inheritance, while `self::` stays frozen.

## Real-World Use Case: Active Record / ORM Pattern

This is the classic reason late static binding exists — used heavily in frameworks like Laravel:

```php
<?php

class Model {
    protected static array $records = [];

    public static function create(string $data): static {
        $obj = new static();
        $obj->data = $data;
        static::$records[] = $data;
        return $obj;
    }

    public static function all(): array {
        return static::$records;
    }
}

class User extends Model {
    protected static array $records = []; // separate storage per subclass
}

class Product extends Model {
    protected static array $records = [];
}

User::create("Alice");
User::create("Bob");
Product::create("Laptop");

print_r(User::all());    // [Alice, Bob]
print_r(Product::all()); // [Laptop]
```

### Output

```
Array
(
    [0] => Alice
    [1] => Bob
)
Array
(
    [0] => Laptop
)
```

Each subclass correctly manages its **own** data because `static::` points to whichever subclass is actually in use.

## Where Late Static Binding Applies

|Used with|Late static binding?|
|---|---|
|`static::method()`|✅ Yes|
|`new static()`|✅ Yes|
|`static::CONSTANT`|✅ Yes|
|`static::$property`|✅ Yes|
|`self::method()`|❌ No (fixed to defining class)|
|Return type `: static`|✅ Yes (resolves to called class)|

## Summary

||`self::`|`static::`|
|---|---|---|
|Binding time|Early (compile-time)|Late (runtime)|
|Follows the calling class?|❌ No|✅ Yes|
|Best for|Things that should **never** change per subclass|Things subclasses should be able to **override/extend**|

