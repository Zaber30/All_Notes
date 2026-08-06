# PHP Operators

## Arithmetic Operators

**Definition:** Perform basic mathematical calculations on numbers.

```php
<?php
$a = 10;
$b = 3;

echo $a + $b . "\n"; // addition
echo $a - $b . "\n"; // subtraction
echo $a * $b . "\n"; // multiplication
echo $a / $b . "\n"; // division
echo $a % $b . "\n"; // modulus (remainder)
echo $a ** $b . "\n"; // exponentiation (power)
```

**Output:**

```
13
7
30
3.3333333333333
1
1000
```

---

## Comparison Operators — `==` vs `===`, `!=` vs `!==`

### `==` (loose equality)

**Definition:** Compares values only, **allowing type juggling** (converts types before comparing).

```php
<?php
var_dump(5 == "5");   // true - "5" is juggled to 5
var_dump(0 == false); // true - false juggled to 0
```

**Output:**

```
bool(true)
bool(true)
```

### `===` (strict equality)

**Definition:** Compares **both value AND type** — no conversion happens.

```php
<?php
var_dump(5 === "5");   // false - int vs string
var_dump(0 === false); // false - int vs bool
```

**Output:**

```
bool(false)
bool(false)
```

### `!=` (loose inequality) vs `!==` (strict inequality)

**Definition:** Opposite of `==` and `===` respectively.

```php
<?php
var_dump(5 != "5");   // false - they ARE loosely equal
var_dump(5 !== "5");  // true - different types, so NOT strictly equal
```

**Output:**

```
bool(false)
bool(true)
```

### Comparison Table

|Operator|Checks|Type-safe?|
|---|---|---|
|`==`|Value only|❌ No|
|`===`|Value + Type|✅ Yes|
|`!=`|Not equal (value)|❌ No|
|`!==`|Not equal (value + type)|✅ Yes|

**Rule of Thumb:** Always prefer `===` and `!==` to avoid unexpected type-juggling bugs.

---

## Logical Operators — `&&`, `||`, `and`, `or`, `xor`

### `&&` (AND) / `and`

**Definition:** True only if **both** conditions are true. `&&` has higher precedence than `and`.

```php
<?php
$age = 25;
$hasId = true;

if ($age >= 18 && $hasId) {
    echo "Can enter\n";
}
```

**Output:** `Can enter`

### `||` (OR) / `or`

**Definition:** True if **at least one** condition is true. `||` has higher precedence than `or`.

```php
<?php
$isAdmin = false;
$isOwner = true;

if ($isAdmin || $isOwner) {
    echo "Access granted\n";
}
```

**Output:** `Access granted`

### `xor` (exclusive OR)

**Definition:** True if **exactly one** condition is true, but not both.

```php
<?php
$hasDiscountCode = true;
$isMember = true;

var_dump($hasDiscountCode xor $isMember); // false - BOTH true, not exclusive
var_dump(true xor false);                 // true - only one is true
```

**Output:**

```
bool(false)
bool(true)
```

### Why `&&`/`||` vs `and`/`or` matters (precedence trap)

```php
<?php
$result = false or true; // = looks like it assigns "true", but doesn't!
var_dump($result); // false - because 'or' has LOWER precedence than '='

$result2 = false || true; // works as expected
var_dump($result2); // true
```

**Output:**

```
bool(false)
bool(true)
```

**Rule of Thumb:** Use `&&` and `||` in normal conditions; avoid `and`/`or` unless you understand precedence rules.

---

## Spaceship Operator `<=>`

**Definition:** Compares two values and returns `-1`, `0`, or `1` depending on whether the left is less than, equal to, or greater than the right. Very useful in `usort()`.

```php
<?php
echo 1 <=> 2 . "\n"; // -1 (left is smaller)
echo 2 <=> 2 . "\n"; // 0  (equal)
echo 3 <=> 2 . "\n"; // 1  (left is bigger)

// Common real-world use: sorting
$numbers = [5, 2, 8, 1];
usort($numbers, fn($a, $b) => $a <=> $b); // ascending sort
print_r($numbers);
```

**Output:**

```
-1
0
1
[1, 2, 5, 8]
```

---

## Null Coalescing Operator `??`

**Definition:** Returns the left value if it's **set and not null**, otherwise returns the right value. Safer alternative to `isset()` + ternary.

```php
<?php
$username = null;
$name = $username ?? "Guest";
echo $name . "\n";

// Great for array keys that might not exist (no warning/error)
$data = ["age" => 25];
$city = $data["city"] ?? "Unknown";
echo $city . "\n";
```

**Output:**

```
Guest
Unknown
```

**Chaining `??`:**

```php
<?php
$a = null;
$b = null;
$c = "Found me";

echo $a ?? $b ?? $c ?? "default" . "\n"; // returns first non-null
```

**Output:** `Found me`

---

## Null Coalescing Assignment `??=`

**Definition:** Assigns a value **only if** the variable is currently `null` or not set — shorthand for `$var = $var ?? value;`.

```php
<?php
$settings = ["theme" => "dark"];

$settings["theme"] ??= "light";   // already set, stays "dark"
$settings["language"] ??= "en";   // not set, becomes "en"

print_r($settings);
```

**Output:**

```
["theme"=>"dark", "language"=>"en"]
```

---

## Spread Operator `...`

**Definition:** "Unpacks" an array's elements individually — used in function calls or when building new arrays.

```php
<?php
function add($a, $b, $c) {
    return $a + $b + $c;
}

$numbers = [1, 2, 3];
echo add(...$numbers) . "\n"; // unpacked as add(1, 2, 3)

// Merging arrays
$a = [1, 2];
$b = [3, 4];
$merged = [...$a, ...$b];
print_r($merged);
```

**Output:**

```
6
[1, 2, 3, 4]
```

---

## Named Arguments

**Definition:** Pass function arguments by **parameter name** instead of position — lets you skip optional parameters and improves readability.

```php
<?php
function createUser(string $name, int $age = 18, string $country = "Unknown") {
    echo "$name, $age, $country\n";
}

// Skip $age, use default, only specify what you need
createUser(name: "Alice", country: "Bangladesh");

// Can also mix order freely
createUser(country: "USA", name: "Bob", age: 30);
```

**Output:**

```
Alice, 18, Bangladesh
Bob, 30, USA
```

---

## Quick Summary Table

|Operator/Concept|One-Line Meaning|
|---|---|
|Arithmetic (`+ - * / % **`)|Basic math calculations|
|`==` vs `===`|Loose (value only) vs strict (value + type) equality|
|`!=` vs `!==`|Loose vs strict inequality|
|`&&` / `||
|`<=>` (spaceship)|Returns -1/0/1 for comparison, great for sorting|
|`??` (null coalescing)|Use fallback if left side is null/unset|
|`??=` (null coalescing assignment)|Assign only if variable is null/unset|
|`...` (spread)|Unpack array elements into individual values|
|Named arguments|Pass args by parameter name, in any order|

Want **control structures** next (if/else, switch, match, loops), since operators are used directly inside them?