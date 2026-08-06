In PHP, a **class** is a blueprint for objects. **Properties** are variables that belong to a class (they hold data), and **methods** are functions that belong to a class (they define behavior).
```cpp
<?php

class Car {
    // Properties (variables)
    public string $brand;
    public string $color;
    private int $speed = 0;

    // Constructor method - runs when object is created
    public function __construct(string $brand, string $color) {
        $this->brand = $brand;
        $this->color = $color;
    }

    // Method (function)
    public function accelerate(int $amount): void {
        $this->speed += $amount;
        echo "{$this->brand} is now going {$this->speed} km/h\n";
    }

    public function brake(int $amount): void {
        $this->speed = max(0, $this->speed - $amount);
        echo "{$this->brand} slowed down to {$this->speed} km/h\n";
    }

    public function getInfo(): string {
        return "This is a {$this->color} {$this->brand}";
    }
}

// Creating an object (instance) of the class
$myCar = new Car("Toyota", "Red");

// Accessing a property
echo $myCar->brand . "\n";       // Output: Toyota

// Calling methods
echo $myCar->getInfo() . "\n";   // Output: This is a Red Toyota
$myCar->accelerate(50);          // Output: Toyota is now going 50 km/h
$myCar->brake(20);               // Output: Toyota slowed down to 30 km/h

?>
```

### we can declare property in following ways
|Feature|Example|
|---|---|
|Public|`public string $name;`|
|Protected|`protected string $name;`|
|Private|`private string $name;`|
|Static|`public static int $count;`|
|Readonly|`public readonly int $id;`|
|Typed|`public int $age;`|
|Untyped|`public $value;`|
|Nullable|`public ?string $email;`|
|Union Type|`public int|
|Intersection Type|`public Countable&Iterator $obj;`|
|Mixed|`public mixed $data;`|
|Constructor Promotion|`public function __construct(public string $name) {}`|
|Default Value|`public string $name = "Guest";`|
|Final (PHP 8.4+)|`final public string $id;`|