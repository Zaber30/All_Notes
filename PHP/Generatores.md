# PHP Generators

## `yield` Keyword ⭐

**Definition:** `yield` turns a normal function into a **generator** — instead of returning one value and ending, it **pauses** and produces a value each time it's asked for the next one (via a loop, for example). Values aren't all built in memory at once.

```php
<?php
function countUpTo(int $max) {
    for ($i = 1; $i <= $max; $i++) {
        yield $i; // pauses here, returns $i, resumes on next iteration
    }
}

foreach (countUpTo(5) as $number) {
    echo $number . "\n";
}
```

**Output:**

```
1
2
3
4
5
```

**With keys (`yield $key => $value`):**

```php
<?php
function getFruits() {
    yield "a" => "Apple";
    yield "b" => "Banana";
    yield "c" => "Cherry";
}

foreach (getFruits() as $key => $fruit) {
    echo "$key: $fruit\n";
}
```

**Output:**

```
a: Apple
b: Banana
c: Cherry
```

---

## `yield from` (Generator Delegation)

**Definition:** Delegates iteration to **another generator, array, or Traversable** — lets you combine multiple generators (or arrays) into one without manually looping over each.

```php
<?php
function letters() {
    yield "a";
    yield "b";
}

function numbers() {
    yield 1;
    yield 2;
}

function combined() {
    yield from letters(); // delegates to another generator
    yield from numbers();
    yield from ["x", "y"]; // can also delegate to a plain array
}

foreach (combined() as $value) {
    echo $value . "\n";
}
```

**Output:**

```
a
b
1
2
x
y
```

---

## Generator Return Values

**Definition:** A generator can also `return` a final value (unlike yielded values, this is **not** iterated over) — accessed via `getReturn()` **after** the generator has finished iterating.

```php
<?php
function processItems(array $items) {
    $count = 0;
    foreach ($items as $item) {
        yield strtoupper($item);
        $count++;
    }
    return "Processed $count items"; // final return value, not yielded
}

$gen = processItems(["apple", "banana", "cherry"]);

foreach ($gen as $value) {
    echo $value . "\n";
}

echo $gen->getReturn() . "\n"; // only available after loop finishes
```

**Output:**

```
APPLE
BANANA
CHERRY
Processed 3 items
```

---

## Sending Values into Generators

**Definition:** The `send()` method pushes a value **into** the generator, which becomes the result of the current `yield` expression — allowing two-way communication.

```php
<?php
function chatBot() {
    while (true) {
        $message = yield; // pauses, waits to receive a value
        echo "Bot received: $message\n";
    }
}

$bot = chatBot();
$bot->current(); // must "prime" the generator first (advances to first yield)

$bot->send("Hello");
$bot->send("How are you?");
```

**Output:**

```
Bot received: Hello
Bot received: How are you?
```

**Using sent value in a running total:**

```php
<?php
function runningTotal() {
    $total = 0;
    while (true) {
        $num = yield $total; // yields current total, receives new number
        $total += $num;
    }
}

$gen = runningTotal();
$gen->current(); // prime it

echo $gen->send(10) . "\n"; // total = 10
echo $gen->send(5) . "\n";  // total = 15
echo $gen->send(20) . "\n"; // total = 35
```

**Output:**

```
10
15
35
```

---

## Memory-Efficient Iteration

**Definition:** Generators produce values **one at a time**, on demand — they don't build the entire result set in memory like an array would. Critical for processing large datasets or files.

```php
<?php
// ❌ Memory-heavy: builds a 10 million element array all at once
function getNumbersArray(int $max): array {
    $result = [];
    for ($i = 1; $i <= $max; $i++) {
        $result[] = $i;
    }
    return $result; // all 10 million numbers sit in memory at once
}

// ✅ Memory-efficient: only ONE number exists in memory at a time
function getNumbersGenerator(int $max) {
    for ($i = 1; $i <= $max; $i++) {
        yield $i;
    }
}

// Safe even for huge numbers - memory usage stays flat
foreach (getNumbersGenerator(10000000) as $number) {
    if ($number > 3) break; // just showing it works, not looping all 10M
    echo $number . "\n";
}
```

**Output:**

```
1
2
3
```

**Real-world example: reading a huge file line-by-line**

```php
<?php
function readLargeFile(string $path) {
    $handle = fopen($path, 'r');
    while (!feof($handle)) {
        yield fgets($handle); // yields ONE line at a time, not the whole file
    }
    fclose($handle);
}

foreach (readLargeFile('huge_log.txt') as $line) {
    // process one line at a time - even a 10GB file won't blow up memory
    echo trim($line) . "\n";
}
```

---

## Generators vs Arrays vs Iterators

|Feature|Array|Iterator (class)|Generator|
|---|---|---|---|
|Memory usage|Holds ALL elements at once|Depends on implementation|Only current value in memory|
|Syntax complexity|Simple|Requires implementing 5 methods (`current`, `key`, `next`, `rewind`, `valid`)|Just use `yield`|
|Lazy evaluation|❌ No — built immediately|✅ Possible|✅ Yes, always|
|Can be looped with `foreach`?|✅ Yes|✅ Yes|✅ Yes|
|Can rewind/loop twice?|✅ Yes|Depends|❌ No (single-use only)|
|Two-way communication (`send()`)|❌ No|❌ No|✅ Yes|

### Iterator class example (for comparison)

```php
<?php
class NumberIterator implements Iterator {
    private int $position = 0;
    private array $numbers;

    public function __construct(array $numbers) {
        $this->numbers = $numbers;
    }

    public function current(): mixed { return $this->numbers[$this->position]; }
    public function key(): mixed { return $this->position; }
    public function next(): void { $this->position++; }
    public function rewind(): void { $this->position = 0; }
    public function valid(): bool { return isset($this->numbers[$this->position]); }
}

$iterator = new NumberIterator([10, 20, 30]);
foreach ($iterator as $num) {
    echo $num . "\n";
}
```

**Output:**

```
10
20
30
```

**Compare to the generator version** — same result, far less code:

```php
<?php
function numberGenerator(array $numbers) {
    foreach ($numbers as $num) {
        yield $num;
    }
}

foreach (numberGenerator([10, 20, 30]) as $num) {
    echo $num . "\n";
}
```

**Output:**

```
10
20
30
```

### Important gotcha — generators can't be reused

```php
<?php
function simpleGen() {
    yield 1;
    yield 2;
}

$gen = simpleGen();
foreach ($gen as $v) { echo $v . "\n"; } // works fine

// foreach ($gen as $v) { echo $v . "\n"; } // ❌ Error! Cannot rewind a generator
```

---

## Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|`yield` ⭐|Pauses function, produces one value at a time|
|`yield from`|Delegates to another generator/array/iterable|
|Generator return values|Final `return` value, accessed via `getReturn()` after iteration|
|Sending values (`send()`)|Push values INTO a running generator|
|Memory efficiency|Only current value in memory — ideal for large datasets/files|
|Generators vs Arrays vs Iterators|Generator = simple syntax + lazy + low memory, but single-use only|

## Rule of Thumb

> Use **generators** whenever you're iterating over something **large or unknown in size** (big files, database results, infinite sequences) — you get array-like simplicity without loading everything into memory.

Want **error handling** next (`try/catch/finally`, custom exceptions), since generators often need exception handling for cleanup (`finally` blocks inside generator loops)?