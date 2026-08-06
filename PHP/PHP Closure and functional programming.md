# PHP Closures & Functional Programming

## Closures vs Anonymous Functions

**Definition:** In PHP, these terms are basically **the same thing** — an anonymous function IS a closure. Technically, a "closure" refers to the ability of an anonymous function to **capture variables from the surrounding scope**.

```php
<?php
// This IS an anonymous function AND a closure
$greet = function ($name) {
    echo "Hello, $name!\n";
};

$greet("Alice");
```

**Output:** `Hello, Alice!`

**What makes it a "closure" specifically** is capturing outside variables:

```php
<?php
$greeting = "Hi";

$greet = function ($name) use ($greeting) { // captures $greeting from outer scope
    echo "$greeting, $name!\n";
};

$greet("Bob");
```

**Output:** `Hi, Bob!`

---

## `use` Keyword in Closures

**Definition:** Explicitly imports outer-scope variables **into** the closure. By default, closures **cannot** see outer variables unless passed via `use()`.

```php
<?php
$tax = 0.15;

// By VALUE (default) - closure gets a COPY, outer variable unaffected
$calculateTotal = function ($price) use ($tax) {
    return $price + ($price * $tax);
};

echo $calculateTotal(100) . "\n"; // 115
```

**Output:** `115`

**By reference (`use (&$var)`)** — changes inside the closure affect the outer variable:

```php
<?php
$total = 0;

$addToTotal = function ($amount) use (&$total) {
    $total += $amount; // modifies the OUTER $total
};

$addToTotal(50);
$addToTotal(30);
echo $total . "\n";
```

**Output:** `80`

---

## Binding Closures — `bindTo()`, `bind()`, `call()` 🔴

**Definition:** These let you attach (or change) which **object** and **class scope** a closure has access to via `$this` — even after the closure was created outside that class.

### `bindTo()`

**Definition:** Returns a **new closure** bound to a given object.

```php
<?php
class Wallet {
    private float $balance = 100;
}

$getBalance = function () {
    return $this->balance; // needs to be bound to a Wallet instance
};

$bound = $getBalance->bindTo(new Wallet(), Wallet::class);
echo $bound() . "\n";
```

**Output:** `100`

### `Closure::bind()`

**Definition:** Static version of `bindTo()` — same effect, called differently.

```php
<?php
class Wallet {
    private float $balance = 250;
}

$getBalance = function () {
    return $this->balance;
};

$bound = Closure::bind($getBalance, new Wallet(), Wallet::class);
echo $bound() . "\n";
```

**Output:** `250`

### `call()`

**Definition:** Binds AND immediately calls the closure in one step (temporary — doesn't create a reusable bound closure).

```php
<?php
class Wallet {
    private float $balance = 500;
}

$getBalance = function () {
    return $this->balance;
};

echo $getBalance->call(new Wallet()) . "\n"; // bind + call in one step
```

**Output:** `500`

---

## `Closure::fromCallable()`

**Definition:** Converts any "callable" (string function name, array `[object, method]`, etc.) into a proper `Closure` object.

```php
<?php
// Convert a built-in function into a Closure
$strlenClosure = Closure::fromCallable('strlen');
echo $strlenClosure("Hello") . "\n";

class Calculator {
    public function add($a, $b) {
        return $a + $b;
    }
}

$calc = new Calculator();
$addClosure = Closure::fromCallable([$calc, 'add']);
echo $addClosure(3, 4) . "\n";
```

**Output:**

```
5
7
```

**Note:** PHP 8.1+'s first-class callable syntax (`strlen(...)`) is now the preferred shorthand for this.

---

## Arrow Functions and `use` — Implicit Capture

**Definition:** Arrow functions (`fn() =>`) **automatically capture** outer variables **by value** — no `use()` needed. But they **cannot** capture by reference or modify outer scope.

```php
<?php
$tax = 0.15;

// No 'use' needed - automatically captures $tax
$calculateTotal = fn($price) => $price + ($price * $tax);

echo $calculateTotal(100) . "\n";
```

**Output:** `115`

**Limitation — arrow functions only capture, never modify outer scope:**

```php
<?php
$count = 0;
$increment = fn() => $count++; // does NOT affect outer $count
$increment();
echo $count . "\n"; // still 0
```

**Output:** `0`

---

## Higher-Order Functions

**Definition:** A function that either **accepts another function as an argument** or **returns a function**. This is the foundation of functional programming in PHP.

```php
<?php
// Accepts a function as an argument
function applyOperation(array $numbers, callable $operation): array {
    return array_map($operation, $numbers);
}

$doubled = applyOperation([1, 2, 3], fn($n) => $n * 2);
print_r($doubled);

// Returns a function
function multiplier(int $factor): callable {
    return fn($n) => $n * $factor;
}

$triple = multiplier(3);
echo $triple(5) . "\n";
```

**Output:**

```
[2, 4, 6]
15
```

---

## `array_map`, `array_filter`, `array_reduce` Patterns

**Definition:** The three classic **functional programming** array operations — transform, select, and combine.

```php
<?php
$numbers = [1, 2, 3, 4, 5, 6];

// Transform every element
$squared = array_map(fn($n) => $n ** 2, $numbers);

// Keep only matching elements
$evens = array_filter($numbers, fn($n) => $n % 2 === 0);

// Combine into a single value
$sum = array_reduce($numbers, fn($carry, $n) => $carry + $n, 0);

print_r($squared);
print_r($evens);
echo $sum . "\n";

// Chaining them together (common functional pattern)
$result = array_reduce(
    array_filter(
        array_map(fn($n) => $n * 2, $numbers),
        fn($n) => $n > 5
    ),
    fn($carry, $n) => $carry + $n,
    0
);
echo $result . "\n";
```

**Output:**

```
[1, 4, 9, 16, 25, 36]
[1=>2, 3=>4, 5=>6]
21
36
```

---

## Currying and Partial Application 🔴

**Definition:**

- **Currying** — transforming a function that takes multiple arguments into a chain of functions that each take **one** argument.
- **Partial application** — "pre-filling" **some** arguments of a function, returning a new function that takes the rest.

### Currying Example

```php
<?php
function curriedAdd(int $a): callable {
    return function (int $b) use ($a): callable {
        return function (int $c) use ($a, $b): int {
            return $a + $b + $c;
        };
    };
}

echo curriedAdd(1)(2)(3) . "\n"; // called one argument at a time
```

**Output:** `6`

### Partial Application Example

```php
<?php
function partial(callable $fn, ...$fixedArgs): callable {
    return fn(...$remainingArgs) => $fn(...$fixedArgs, ...$remainingArgs);
}

function greet(string $greeting, string $name): string {
    return "$greeting, $name!";
}

$sayHello = partial('greet', 'Hello'); // pre-fill $greeting
echo $sayHello("Alice") . "\n";
echo $sayHello("Bob") . "\n";
```

**Output:**

```
Hello, Alice!
Hello, Bob!
```

---

## Memoization

**Definition:** An optimization technique that **caches** a function's results so expensive calculations aren't repeated for the same inputs — the second call with the same argument returns instantly from cache.

```php
<?php
function memoize(callable $fn): callable {
    $cache = [];

    return function (...$args) use ($fn, &$cache) {
        $key = serialize($args); // unique cache key based on arguments

        if (!isset($cache[$key])) {
            echo "Calculating for: " . implode(",", $args) . "\n";
            $cache[$key] = $fn(...$args);
        } else {
            echo "Using cached result for: " . implode(",", $args) . "\n";
        }

        return $cache[$key];
    };
}

function slowSquare(int $n): int {
    sleep(1); // simulate a slow/expensive calculation
    return $n * $n;
}

$fastSquare = memoize('slowSquare');

echo $fastSquare(5) . "\n"; // calculates (slow)
echo $fastSquare(5) . "\n"; // cached (instant)
echo $fastSquare(10) . "\n"; // calculates (slow, new input)
```

**Output:**

```
Calculating for: 5
25
Using cached result for: 5
25
Calculating for: 10
100
```

**Real-world use case:** Caching results of expensive database queries, API calls, or complex math (like Fibonacci calculations) within a single request.

---

## Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|Closures vs anonymous functions|Same thing — "closure" emphasizes capturing outer variables|
|`use` keyword|Explicitly import outer variables (by value or reference `&`)|
|`bindTo()` / `Closure::bind()`|Attach a closure to an object/class scope, get new closure|
|`call()` 🔴|Bind + immediately invoke, in one step|
|`Closure::fromCallable()`|Convert any callable into a proper Closure object|
|Arrow functions (`fn() =>`)|Auto-capture outer vars by value, no `use()` needed|
|Higher-order functions|Functions that accept/return other functions|
|`array_map/filter/reduce`|Transform / select / combine array elements functionally|
|Currying 🔴|Multi-arg function → chain of single-arg functions|
|Partial application 🔴|Pre-fill some arguments, return function for the rest|
|Memoization|Cache function results to avoid repeated expensive work|

Want **error handling** next (`try/catch/finally`, custom exceptions), or **PSR standards** to round out best practices?