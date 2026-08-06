## PHP Classes and Objects

**Class** = a blueprint/template for creating things.  
**Object** = an actual thing made from that blueprint.

Think of a class like a cookie cutter, and objects like the cookies you make with it.

```cpp
<?php

class Car {
    public $brand;
    public $color;

    public function drive() {
        echo "$this->brand ($this->color) is driving!\n";
    }
}

// Creating objects from the class
$car1 = new Car();
$car1->brand = "Toyota";
$car1->color = "Red";

$car2 = new Car();
$car2->brand = "Honda";
$car2->color = "Blue";

$car1->drive(); // Toyota (Red) is driving!
$car2->drive(); // Honda (Blue) is driving!

?>
```
