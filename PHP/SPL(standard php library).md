# SPL (Standard PHP Library)

## `SplStack`

**Definition:** A **LIFO** (Last In, First Out) data structure — like a stack of plates, you add/remove from the **top**. Built on `SplDoublyLinkedList`.

```php
<?php
$stack = new SplStack();

$stack->push(1);
$stack->push(2);
$stack->push(3);

echo $stack->top() . "\n";    // 3 - peek top without removing
echo $stack->count() . "\n";  // 3

echo $stack->pop() . "\n";    // 3 - removes and returns top
echo $stack->pop() . "\n";    // 2

foreach ($stack as $value) {
    echo $value . "\n"; // iterates top-to-bottom by default
}
```

**Output:**

```
3
3
3
2
1
```

**Common methods:** `push()`, `pop()`, `top()`, `count()`, `isEmpty()`, `unshift()`, `shift()`.

---

## `SplQueue`

**Definition:** A **FIFO** (First In, First Out) data structure — like a checkout line, first added is first removed. Also built on `SplDoublyLinkedList`.

```php
<?php
$queue = new SplQueue();

$queue->enqueue("Alice");
$queue->enqueue("Bob");
$queue->enqueue("Charlie");

echo $queue->count() . "\n";   // 3
echo $queue->dequeue() . "\n"; // Alice - removes from front
echo $queue->dequeue() . "\n"; // Bob

foreach ($queue as $value) {
    echo $value . "\n"; // remaining items
}
```

**Output:**

```
3
Alice
Bob
Charlie
```

**Common methods:** `enqueue()`, `dequeue()`, `push()`, `isEmpty()`, `count()`.

---

## `SplHeap`

**Definition:** An **abstract class** for building a heap (tree-based structure that keeps items sorted by priority). You extend it and define your own `compare()` logic. PHP provides `SplMinHeap` and `SplMaxHeap` as ready-made versions.

```php
<?php
// Custom heap - extends abstract SplHeap
class MinHeap extends SplHeap {
    protected function compare($value1, $value2): int {
        return $value2 <=> $value1; // reversed for min-heap behavior
    }
}

$heap = new MinHeap();
$heap->insert(5);
$heap->insert(1);
$heap->insert(3);

while (!$heap->isEmpty()) {
    echo $heap->extract() . "\n"; // always extracts smallest first
}
```

**Output:**

```
1
3
5
```

**Built-in versions (no need to write `compare()`):**

```php
<?php
$minHeap = new SplMinHeap();
$minHeap->insert(10);
$minHeap->insert(2);
$minHeap->insert(7);
echo $minHeap->extract() . "\n"; // 2 (smallest first)

$maxHeap = new SplMaxHeap();
$maxHeap->insert(10);
$maxHeap->insert(2);
$maxHeap->insert(7);
echo $maxHeap->extract() . "\n"; // 10 (largest first)
```

**Output:**

```
2
10
```

**Common methods:** `insert()`, `extract()`, `top()`, `isEmpty()`, `count()`.

---

## `SplDoublyLinkedList`

**Definition:** A linked list allowing insertion/removal from **both ends** efficiently. Both `SplStack` and `SplQueue` extend this class.

```php
<?php
$list = new SplDoublyLinkedList();

$list->push("B");     // add to end
$list->push("C");
$list->unshift("A");  // add to beginning

foreach ($list as $item) {
    echo $item . "\n"; // A, B, C
}

echo $list->top() . "\n";    // C - last element
echo $list->bottom() . "\n"; // A - first element
```

**Output:**

```
A
B
C
C
A
```

**Common methods:** `push()`, `pop()`, `shift()`, `unshift()`, `top()`, `bottom()`, `isEmpty()`, `count()`.

---

## `SplPriorityQueue`

**Definition:** A queue where each item has a **priority** — higher priority items are extracted **first**, regardless of insertion order.

```php
<?php
$queue = new SplPriorityQueue();

$queue->insert("Low priority task", 1);
$queue->insert("High priority task", 10);
$queue->insert("Medium priority task", 5);

while (!$queue->isEmpty()) {
    echo $queue->extract() . "\n"; // extracted by priority, highest first
}
```

**Output:**

```
High priority task
Medium priority task
Low priority task
```

**Common methods:** `insert($value, $priority)`, `extract()`, `top()`, `isEmpty()`, `count()`, `setExtractFlags()`.

---

## `SplFixedArray` (Memory Efficient)

**Definition:** An array with a **fixed size** set at creation — uses **less memory** than regular PHP arrays because it only allows integer keys within the defined size (no dynamic resizing overhead).

```php
<?php
$arr = new SplFixedArray(3); // fixed size of 3

$arr[0] = "Apple";
$arr[1] = "Banana";
$arr[2] = "Cherry";

echo $arr->getSize() . "\n"; // 3
echo $arr[1] . "\n";         // Banana

foreach ($arr as $index => $value) {
    echo "$index: $value\n";
}

$arr->setSize(5); // can resize if needed
echo $arr->getSize() . "\n"; // 5

$regularArray = $arr->toArray(); // convert to normal PHP array
print_r($regularArray);
```

**Output:**

```
3
Banana
0: Apple
1: Banana
2: Cherry
5
[Apple, Banana, Cherry, null, null]
```

**Common methods:** `getSize()`, `setSize()`, `toArray()`, `count()`.

---

## `SplObjectStorage`

**Definition:** A container for storing **objects as keys**, each optionally paired with associated data — like a map where the keys are objects instead of strings/ints.

```php
<?php
class User {
    public function __construct(public string $name) {}
}

$storage = new SplObjectStorage();

$alice = new User("Alice");
$bob = new User("Bob");

$storage->attach($alice, "Admin role");
$storage->attach($bob, "Editor role");

echo $storage->count() . "\n"; // 2
echo $storage[$alice] . "\n";  // Admin role

var_dump($storage->contains($alice)); // true

foreach ($storage as $user) {
    echo $user->name . ": " . $storage->getInfo() . "\n";
}

$storage->detach($bob);
echo $storage->count() . "\n"; // 1
```

**Output:**

```
2
Admin role
bool(true)
Alice: Admin role
Bob: Editor role
1
```

**Common methods:** `attach()`, `detach()`, `contains()`, `count()`, `getInfo()`, `offsetGet()`/`offsetSet()` (via `[]`).

---

## Iterators — `Iterator`, `IteratorAggregate`, `RecursiveIterator`

### `Iterator` Interface

**Definition:** Requires implementing 5 methods to make a class manually loopable with `foreach`.

```php
<?php
class NumberCollection implements Iterator {
    private array $numbers;
    private int $position = 0;

    public function __construct(array $numbers) {
        $this->numbers = $numbers;
    }

    public function current(): mixed { return $this->numbers[$this->position]; }
    public function key(): mixed { return $this->position; }
    public function next(): void { $this->position++; }
    public function rewind(): void { $this->position = 0; }
    public function valid(): bool { return isset($this->numbers[$this->position]); }
}

$collection = new NumberCollection([10, 20, 30]);
foreach ($collection as $key => $value) {
    echo "$key => $value\n";
}
```

**Output:**

```
0 => 10
1 => 20
2 => 30
```

### `IteratorAggregate` Interface

**Definition:** Simpler alternative — just implement **one** method (`getIterator()`) that returns an iterator (often using a generator or `ArrayIterator`).

```php
<?php
class NumberCollection implements IteratorAggregate {
    private array $numbers;

    public function __construct(array $numbers) {
        $this->numbers = $numbers;
    }

    public function getIterator(): Iterator {
        return new ArrayIterator($this->numbers); // delegate to built-in iterator
    }
}

$collection = new NumberCollection([1, 2, 3]);
foreach ($collection as $value) {
    echo $value . "\n";
}
```

**Output:**

```
1
2
3
```

### `RecursiveIterator`

**Definition:** An iterator for **nested/tree-like structures**, allowing traversal into child elements — usually paired with `RecursiveIteratorIterator`.

```php
<?php
$data = new RecursiveArrayIterator([
    "fruits" => ["apple", "banana"],
    "veggies" => ["carrot", "potato"],
]);

$iterator = new RecursiveIteratorIterator($data, RecursiveIteratorIterator::SELF_FIRST);

foreach ($iterator as $key => $value) {
    if (is_array($value)) {
        echo "Category: $key\n";
    } else {
        echo "  Item: $value\n";
    }
}
```

**Output:**

```
Category: fruits
  Item: apple
  Item: banana
Category: veggies
  Item: carrot
  Item: potato
```

---

## `ArrayObject`, `ArrayIterator`

### `ArrayObject`

**Definition:** Wraps a regular array so it behaves like an **object** — supports `foreach`, `count()`, array access (`[]`), and can be extended.

```php
<?php
$arrObj = new ArrayObject(["a" => 1, "b" => 2, "c" => 3]);

echo $arrObj->count() . "\n"; // 3
echo $arrObj["a"] . "\n";     // 1

$arrObj["d"] = 4; // add like a normal array
$arrObj->append(5); // wait - append works with numeric-style arrays only

foreach ($arrObj as $key => $value) {
    echo "$key => $value\n";
}

$plainArray = $arrObj->getArrayCopy(); // convert back to plain array
print_r($plainArray);
```

**Output:**

```
3
1
a => 1
b => 2
c => 3
d => 4
[a=>1, b=>2, c=>3, d=>4]
```

### `ArrayIterator`

**Definition:** Provides iteration for an array via the `Iterator` interface — often returned from `getIterator()` in custom `IteratorAggregate` classes (as shown above).

```php
<?php
$iterator = new ArrayIterator(["x", "y", "z"]);

foreach ($iterator as $key => $value) {
    echo "$key: $value\n";
}

echo $iterator->count() . "\n"; // 3
```

**Output:**

```
0: x
1: y
2: z
3
```

---

## `Countable` Interface

**Definition:** Lets a custom class work with PHP's built-in `count()` function by implementing a single `count()` method.

```php
<?php
class Playlist implements Countable {
    private array $songs = [];

    public function addSong(string $song): void {
        $this->songs[] = $song;
    }

    public function count(): int {
        return count($this->songs);
    }
}

$playlist = new Playlist();
$playlist->addSong("Song A");
$playlist->addSong("Song B");
$playlist->addSong("Song C");

echo count($playlist) . "\n"; // uses OUR count() method automatically
```

**Output:** `3`

---

## Quick Summary Table

|Class/Interface|Purpose|Key Methods|
|---|---|---|
|`SplStack`|LIFO (last in, first out)|`push()`, `pop()`, `top()`|
|`SplQueue`|FIFO (first in, first out)|`enqueue()`, `dequeue()`|
|`SplHeap` (+ Min/Max)|Priority-ordered extraction|`insert()`, `extract()`|
|`SplDoublyLinkedList`|Insert/remove from both ends|`push()`, `unshift()`, `pop()`, `shift()`|
|`SplPriorityQueue`|Extract by custom priority value|`insert($val, $priority)`, `extract()`|
|`SplFixedArray`|Fixed-size, memory-efficient array|`getSize()`, `setSize()`, `toArray()`|
|`SplObjectStorage`|Map objects to data|`attach()`, `detach()`, `contains()`|
|`Iterator`|Manual 5-method foreach support|`current/key/next/rewind/valid`|
|`IteratorAggregate`|Simple 1-method foreach support|`getIterator()`|
|`RecursiveIterator`|Traverse nested/tree structures|used with `RecursiveIteratorIterator`|
|`ArrayObject`|Array wrapped as an object|`count()`, `getArrayCopy()`, `[]` access|
|`ArrayIterator`|Iterator for arrays|`count()`, standard iterator methods|
|`Countable`|Enable `count()` on custom classes|`count()`|

Want **error handling** next (`try/catch/finally`, custom exceptions, SPL exception classes like `InvalidArgumentException`), since SPL also includes a whole family of exception classes?