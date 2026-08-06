### Definition

**Static** properties and methods belong to the **class itself**, not to any individual object. This means:

- You don't need to create an object (`new ClassName()`) to use them
- All objects of the class **share the same** static property (there's only one copy)
- Accessed using `::` (the **scope resolution operator**), not `->`

### Key Features

1. **Shared across all instances** — changing a static property affects it everywhere.
2. **Accessed without an object** — `ClassName::$property` or `ClassName::method()`.
3. **`$this` is NOT available** inside static methods (since there's no specific object).
4. Use **`self::`** to refer to static members from within the class.
5. Useful for things like **counters, shared configuration, or utility/helper functions**.

```cpp
<?php

class Car {
    public string $brand;
    public static int $totalCars = 0; // shared by ALL Car objects

    public function __construct(string $brand) {
        $this->brand = $brand;
        self::$totalCars++; // increment the shared counter
    }

    // Static method - utility function, not tied to one object
    public static function getTotalCars(): int {
        return self::$totalCars;
    }
}

$car1 = new Car("Toyota");
$car2 = new Car("Honda");
$car3 = new Car("Ford");

// Accessing static property/method WITHOUT creating an object reference
echo Car::getTotalCars() . "\n";   // 3
echo Car::$totalCars . "\n";       // 3
```
### `self::` vs `$this->`

||`self::`|`$this->`|
|---|---|---|
|Refers to|The **class**|The **current object instance**|
|Used for|Static properties/methods|Non-static (instance) properties/methods|
|Available in static methods?|✅ Yes|❌ No|