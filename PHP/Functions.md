# PHP Functions

## Function Declaration and Calling

**Definition:** A function is a reusable block of code defined with `function` and executed by calling its name with parentheses.

```php
<?php
function greet($name) {
    echo "Hello, $name!\n";
}

greet("Alice"); // calling the function
```

**Output:** `Hello, Alice!`

---

## Default Parameter Values

**Definition:** A parameter can have a default value, used automatically if the caller doesn't provide one.

```php
<?php
function greet($name, $greeting = "Hello") {
    echo "$greeting, $name!\n";
}

greet("Alice");           // uses default greeting
greet("Bob", "Good morning");
```

**Output:**

```
Hello, Alice!
Good morning, Bob!
```

---

## Named Arguments ⭐

**Definition:** Pass arguments by **parameter name** instead of position — useful for skipping optional params or improving readability.

```php
<?php
function createUser(string $name, int $age = 18, string $country = "Unknown") {
    echo "$name, $age, $country\n";
}

createUser(name: "Alice", country: "Bangladesh"); // skip $age, use default
```

**Output:** `Alice, 18, Bangladesh`

---

## Variadic Functions (`...$args`)

**Definition:** Allows a function to accept an **unlimited number of arguments**, collected into an array.

```php
<?php
function sum(...$numbers) {
    return array_sum($numbers);
}

echo sum(1, 2, 3, 4) . "\n";
```

**Output:** `10`

---

## Pass by Reference (`&$var`)

**Definition:** Normally arguments are passed by value (a copy). Using `&` passes the **actual variable**, so changes inside the function affect the original.

```php
<?php
function addTen(&$num) {
    $num += 10;
}

$value = 5;
addTen($value);
echo $value . "\n";
```

**Output:** `15`

---

## Return Types and Type Declarations

**Definition:** Specifies the expected type of a function's parameters and return value; PHP enforces it and throws a `TypeError` if violated.

```php
<?php
function add(int $a, int $b): int {
    return $a + $b;
}

$result = add(3, 4);
echo $result . "\n";
```

**Output:** `7`

---

## Union Types `int|string`

**Definition:** Allows a parameter or return value to accept **more than one specific type**.

```php
<?php
function formatId(int|string $id): string {
    return "ID-" . $id;
}

echo formatId(101) . "\n";
echo formatId("A101") . "\n";
```

**Output:**

```
ID-101
ID-A101
```

---

## Nullable Types `?string`

**Definition:** A shorthand for allowing a type **or `null`**, written with a `?` prefix. Equivalent to `string|null`.

```php
<?php
function greet(?string $name): string {
    return "Hello, " . ($name ?? "Guest");
}

echo greet("Alice") . "\n";
echo greet(null) . "\n";
```

**Output:**

```
Hello, Alice
Hello, Guest
```

---

## `void` Return Type

**Definition:** Declares that a function **returns nothing**. Trying to return a value (other than empty `return;`) causes an error.

```php
<?php
function logMessage(string $msg): void {
    echo "LOG: $msg\n";
    // no return value allowed
}

logMessage("System started");
```

**Output:** `LOG: System started`

---

## `never` Return Type 🔴

**Definition:** (PHP 8.1+) Declares that a function **never returns normally** — it always throws an exception, exits, or loops forever.

```php
<?php
function stopExecution(string $message): never {
    echo "Fatal: $message\n";
    exit;
}

function process(int $value): int {
    if ($value < 0) {
        stopExecution("Negative value not allowed");
    }
    return $value * 2;
}

echo process(5) . "\n"; // 10
process(-1); // triggers stopExecution(), function never "returns"
```

**Output:**

```
10
Fatal: Negative value not allowed
```

---

## Anonymous Functions (Closures)

**Definition:** Functions **without a name**, often assigned to variables or passed as arguments. Can capture outer variables using `use()`.

```php
<?php
$multiplier = 3;

$multiply = function ($num) use ($multiplier) {
    return $num * $multiplier;
};

echo $multiply(5) . "\n";
```

**Output:** `15`

---

## Arrow Functions `fn() =>`

**Definition:** A shorter closure syntax (PHP 7.4+) that **automatically captures** outer variables (no `use()` needed) and implicitly returns the expression.

```php
<?php
$multiplier = 3;

$multiply = fn($num) => $num * $multiplier; // auto-captures $multiplier

echo $multiply(5) . "\n";
```

**Output:** `15`

---

## First-Class Callables `strlen(...)` 🔴

**Definition:** (PHP 8.1+) A concise syntax to create a **closure reference** to any function or method, without wrapping it manually.

```php
<?php
// Old way
$oldStyle = 'strlen';
echo $oldStyle("Hello") . "\n";

// PHP 8.1+ first-class callable syntax
$strlen = strlen(...);
echo $strlen("Hello") . "\n";

class Calculator {
    public function add(int $a, int $b): int {
        return $a + $b;
    }
}

$calc = new Calculator();
$addFn = $calc->add(...); // first-class callable from a method
echo $addFn(3, 4) . "\n";
```

**Output:**

```
5
5
7
```

---

## Recursive Functions

**Definition:** A function that **calls itself** to solve a problem by breaking it into smaller sub-problems, until reaching a base case that stops the recursion.

```php
<?php
function factorial(int $n): int {
    if ($n <= 1) {
        return 1; // base case - stops recursion
    }
    return $n * factorial($n - 1); // calls itself
}

echo factorial(5) . "\n"; // 5*4*3*2*1
```

**Output:** `120`

---

## Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|Function declaration/calling|Define with `function`, run by calling its name|
|Default parameter values|Fallback value used if argument not given|
|Named arguments ⭐|Pass args by parameter name, in any order|
|Variadic (`...$args`)|Accept unlimited arguments as an array|
|Pass by reference (`&$var`)|Function modifies the original variable, not a copy|
|Return types|Enforce expected type of parameters/return|
|Union types (`int|string`)|
|Nullable types (`?string`)|Accept the type OR `null`|
|`void`|Function returns nothing|
|`never` 🔴|Function never returns normally (throws/exits)|
|Anonymous functions|Nameless function, can capture vars with `use()`|
|Arrow functions (`fn() =>`)|Short closure syntax, auto-captures outer variables|
|First-class callables 🔴|`func(...)` — clean syntax to get a function/method reference|
|Recursive functions|A function that calls itself until a base case is reached|

Want **string functions** next, or move to **closures in depth** (binding, `Closure::bind`, etc.)?