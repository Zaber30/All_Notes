# PHP-Specific: Arrays, SPL, Sorting & Strings — Quick Notes

---

## 1. PHP Array as Hash Map Usage

**Definition:** A PHP array is actually a **hybrid** — it can act as both a list (indexed) AND a hash map / dictionary (key-value pairs) at the same time. Internally, PHP arrays are **ordered hash maps**.

**Real-life analogy:** Like a **filing cabinet** where you can either number your folders 1, 2, 3... (indexed array) OR label them by name like "Invoices," "Contracts," "Receipts" (associative array/hash map) — same cabinet, different labeling style.

```php
// Indexed array (like a numbered list)
$fruits = ["Apple", "Banana", "Mango"];
echo $fruits[1]; // Banana

// Associative array (hash map — key => value)
$user = [
    "name"  => "Karim",
    "email" => "karim@example.com",
    "age"   => 25
];
echo $user["email"]; // karim@example.com

// Fast lookup — O(1) average time, just like a real hash map
if (isset($user["age"])) {
    echo "Age exists: " . $user["age"];
}
```

**Real use case:** Counting word frequency, caching results by unique key, grouping data by category — all fast because array keys work like hash map lookups internally.

```php
// Example: counting occurrences (classic hash map use)
$words = ["apple", "banana", "apple", "orange", "banana", "apple"];
$count = [];
foreach ($words as $word) {
    $count[$word] = ($count[$word] ?? 0) + 1;
}
print_r($count);
// ["apple" => 3, "banana" => 2, "orange" => 1]
```

---

## 2. SPL Data Structures

**Definition:** SPL = **Standard PHP Library** — built-in classes for specialized data structures beyond plain arrays (stacks, queues, heaps, linked lists, fixed arrays), optimized for specific use cases.

**Real-life analogy:** A plain PHP array is like a **general-purpose Swiss Army knife** — works okay for everything. SPL structures are like **specialized tools** — a stack of plates (`SplStack`), a queue at a ticket counter (`SplQueue`), a priority triage system in an ER (`SplPriorityQueue`) — each built specifically for its job, more efficient than forcing an array to do it.

### `SplStack` — Last In, First Out (LIFO)

**Analogy:** A stack of plates — you always take the top plate first.

```php
$stack = new SplStack();
$stack->push("Plate 1");
$stack->push("Plate 2");
$stack->push("Plate 3");
echo $stack->pop(); // Plate 3 (last one added, first one removed)
```

### `SplQueue` — First In, First Out (FIFO)

**Analogy:** A queue at a bank — first person in line gets served first.

```php
$queue = new SplQueue();
$queue->enqueue("Customer A");
$queue->enqueue("Customer B");
echo $queue->dequeue(); // Customer A (first one added, first one served)
```

### `SplPriorityQueue` — Served by Priority, not Order

**Analogy:** Hospital ER triage — critical patients get seen first, regardless of arrival order.

```php
$pq = new SplPriorityQueue();
$pq->insert("Flu patient", 1);
$pq->insert("Heart attack patient", 10); // higher number = higher priority
$pq->insert("Broken arm patient", 5);

echo $pq->extract(); // Heart attack patient (highest priority first)
```

### `SplFixedArray` — Fixed-Size, Memory-Efficient Array

**Analogy:** A parking lot with a **fixed number of spots** — faster and uses less memory than a regular array because the size never changes.

```php
$arr = new SplFixedArray(3);
$arr[0] = "A";
$arr[1] = "B";
$arr[2] = "C";
foreach ($arr as $val) {
    echo $val . " ";
}
// Faster/lighter than a normal PHP array for large, fixed-size datasets
```

### `SplObjectStorage` — Map Objects to Data

**Analogy:** A cloakroom ticket system — each **object (coat)** is linked to specific data (a ticket number), and you can quickly retrieve or check by the object itself, not just an ID.

```php
$storage = new SplObjectStorage();
$user1 = new stdClass();
$storage[$user1] = "Admin role";
echo $storage[$user1]; // Admin role
```

---

## 3. Array Sorting Algorithms in PHP

**Definition:** PHP provides many built-in sorting functions — no need to write bubble sort/quicksort yourself. Choose based on whether you care about keys, values, or both.

**Real-life analogy:** Like sorting a deck of cards — sometimes you only care about the card values (`sort()`), sometimes about keeping track of which player owns which card while sorting (`asort()`), sometimes sorting by custom rules like "sort by suit first, then number" (`usort()`).

|Function|Sorts By|Keeps Keys?|
|---|---|---|
|`sort()`|Values, ascending|❌ No (re-indexes)|
|`rsort()`|Values, descending|❌ No|
|`asort()`|Values, ascending|✅ Yes|
|`arsort()`|Values, descending|✅ Yes|
|`ksort()`|Keys, ascending|✅ Yes|
|`krsort()`|Keys, descending|✅ Yes|
|`usort()`|Custom rule (values)|❌ No|
|`uasort()`|Custom rule (values)|✅ Yes|
|`uksort()`|Custom rule (keys)|✅ Yes|

```php
$scores = [50, 20, 80, 10];
sort($scores);
print_r($scores); // [10, 20, 50, 80]

$prices = ["banana" => 30, "apple" => 100, "mango" => 60];
asort($prices); // sort by value, keep key-value pairing
print_r($prices); // ["banana" => 30, "mango" => 60, "apple" => 100]

// Custom sort: sort products by price descending
$products = [
    ["name" => "Phone", "price" => 500],
    ["name" => "Laptop", "price" => 1000],
    ["name" => "Mouse", "price" => 20],
];
usort($products, fn($a, $b) => $b['price'] <=> $a['price']);
print_r($products); // Laptop, Phone, Mouse
```

**Performance note:** PHP's built-in sort functions use optimized algorithms (like a hybrid quicksort/introsort under the hood) — always faster than writing your own sort manually.

---

## 4. Efficient String Operations

**Definition:** PHP has many built-in string functions optimized in C internally — using them is much faster and cleaner than manual character-by-character loops.

**Real-life analogy:** Like using a **washing machine** (built-in string function) instead of hand-washing clothes one item at a time (manual loop) — same result, dramatically less effort and time.

### Concatenation — use `.=` or arrays + `implode()` for large loops

```php
// ❌ Slow for large loops (repeated string copying)
$result = "";
for ($i = 0; $i < 1000; $i++) {
    $result .= "Item $i, ";
}

// ✅ Faster — build an array, then join once at the end
$parts = [];
for ($i = 0; $i < 1000; $i++) {
    $parts[] = "Item $i";
}
$result = implode(", ", $parts);
```

### Common efficient string functions

```php
$str = "  Hello World  ";

trim($str);                     // "Hello World" — remove whitespace both ends
strtolower($str);                // "  hello world  "
str_contains($str, "World");     // true (PHP 8+, faster than strpos check)
str_starts_with($str, "  Hello");// true (PHP 8+)
substr($str, 2, 5);               // "Hello"
str_replace("World", "PHP", $str);// "  Hello PHP  "
sprintf("Name: %s, Age: %d", "Karim", 25); // formatted string, efficient for templates
```

### `str_contains()` vs old `strpos()` way

```php
// Old way (PHP 7 and below) — confusing because strpos can return 0 (falsy!)
if (strpos($str, "World") !== false) { ... }

// New way (PHP 8+) — clean and efficient
if (str_contains($str, "World")) { ... }
```

### Multi-byte strings (for non-English text, e.g. Bangla)

```php
// Regular functions can break with UTF-8/Bangla text
echo strlen("বাংলা"); // wrong byte count, not character count

// Use mb_ (multi-byte) functions for correct results
echo mb_strlen("বাংলা"); // correct character count
echo mb_substr("বাংলা টেক্সট", 0, 5);
```

---

## ⭐ Quick Cheat Sheet Summary

|Topic|Key Takeaway|
|---|---|
|Array as hash map|PHP arrays = ordered hash maps, O(1) key lookup|
|`SplStack`|LIFO — plate stack|
|`SplQueue`|FIFO — bank line|
|`SplPriorityQueue`|Sorted by priority — ER triage|
|`SplFixedArray`|Fixed size, memory-efficient — parking lot|
|`SplObjectStorage`|Map data to objects — cloakroom tickets|
|`sort()` family|Built-in, optimized — no manual sort algorithms needed|
|`usort()`|Custom sorting logic via callback|
|String building|Use array + `implode()` for large loops, not repeated `.=`|
|`str_contains()`|Clean way to check substring (PHP 8+)|
|`mb_*` functions|Always use for non-English/UTF-8 text (like Bangla)|

---

_Golden Rule: Don't reinvent the wheel — PHP's built-in array, SPL, sorting, and string functions are written in optimized C code and are almost always faster than custom-written alternatives._