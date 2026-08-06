
PHP has **80+ built-in array functions**. The easiest way to learn them is by grouping them by purpose.

| Type              | Purpose                      | Common Functions                                                                                                  |
| ----------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Creation          | Create arrays                | `array()`, `range()`, `compact()`, `array_fill()`, `array_fill_keys()`                                            |
| Information       | Get information about arrays | `count()`, `sizeof()`, `array_key_exists()`, `array_is_list()`                                                    |
| Access            | Get elements                 | `current()`, `next()`, `prev()`, `reset()`, `end()`, `key()`                                                      |
| Search            | Find elements                | `in_array()`, `array_search()`, `array_keys()`, `array_values()`                                                  |
| Add/Remove        | Insert or delete elements    | `array_push()`, `array_pop()`, `array_shift()`, `array_unshift()`, `unset()`                                      |
| Merge/Split       | Combine or divide arrays     | `array_merge()`, `array_merge_recursive()`, `array_combine()`, `array_chunk()`, `array_slice()`, `array_splice()` |
| Sorting           | Sort arrays                  | `sort()`, `rsort()`, `asort()`, `arsort()`, `ksort()`, `krsort()`, `usort()`, `uasort()`, `uksort()`              |
| Filtering         | Filter elements              | `array_filter()`                                                                                                  |
| Mapping           | Transform elements           | `array_map()`                                                                                                     |
| Reduce            | Reduce to one value          | `array_reduce()`                                                                                                  |
| Walk              | Traverse array               | `array_walk()`, `array_walk_recursive()`                                                                          |
| Comparison        | Compare arrays               | `array_diff()`, `array_intersect()`, `array_udiff()`, `array_uintersect()`                                        |
| Unique            | Remove duplicates            | `array_unique()`                                                                                                  |
| Reverse/Flip      | Reverse or swap              | `array_reverse()`, `array_flip()`                                                                                 |
| Random            | Random elements              | `array_rand()`, `shuffle()`                                                                                       |
| Mathematics       | Numeric operations           | `array_sum()`, `array_product()`                                                                                  |
| Extraction        | Extract variables            | `extract()`                                                                                                       |
| Variable to Array | Create array from variables  | `compact()`                                                                                                       |
| Replace           | Replace values               | `array_replace()`, `array_replace_recursive()`                                                                    |
| Padding           | Extend array                 | `array_pad()`                                                                                                     |
| Counting          | Count value occurrences      | `array_count_values()`                                                                                            |
| Column            | Extract column               | `array_column()`                                                                                                  |
# PHP Array Functions — Complete Reference (with Variable Handling)

## 1. Creation

### `array()`

**Definition:** Creates a new array.

```php
<?php
$arr = array(1, 2, 3);
print_r($arr);
```

**Output:** `[1, 2, 3]`

### `range()`

**Definition:** Creates an array containing a range of numbers or letters.

```php
<?php
$numbers = range(1, 5);
$letters = range('a', 'e');
$stepped = range(0, 10, 2);

print_r($numbers);
print_r($letters);
print_r($stepped);
```

**Output:**

```
[1,2,3,4,5]
[a,b,c,d,e]
[0,2,4,6,8,10]
```

### `compact()`

**Definition:** Builds an array from existing variable names and their values.

```php
<?php
$name = "Alice";
$age = 25;

$result = compact('name', 'age');
print_r($result);
```

**Output:** `["name"=>"Alice", "age"=>25]`

### `array_fill()`

**Definition:** Fills an array with one repeated value, starting at a given index.

```php
<?php
$result = array_fill(0, 3, "x");
print_r($result);
```

**Output:** `[0=>"x", 1=>"x", 2=>"x"]`

### `array_fill_keys()`

**Definition:** Fills an array using specific keys, all set to the same value.

```php
<?php
$keys = ["a", "b", "c"];
$result = array_fill_keys($keys, 0);
print_r($result);
```

**Output:** `["a"=>0, "b"=>0, "c"=>0]`

---

## 2. Information

### `count()`

**Definition:** Returns the number of elements in an array.

```php
<?php
$fruits = ["apple", "banana", "cherry"];
$total = count($fruits);
echo $total . "\n";
```

**Output:** `3`

### `sizeof()`

**Definition:** Alias of `count()`.

```php
<?php
$fruits = ["apple", "banana"];
$total = sizeof($fruits);
echo $total . "\n";
```

**Output:** `2`

### `array_key_exists()`

**Definition:** Checks whether a key exists (even if its value is `null`).

```php
<?php
$person = ["name" => null];
$exists = array_key_exists("name", $person);
var_dump($exists);
```

**Output:** `bool(true)`

### `array_is_list()`

**Definition:** (PHP 8.1+) Checks if an array has sequential integer keys starting at 0.

```php
<?php
$arr1 = [1, 2, 3];
$arr2 = [1 => "a", 0 => "b"];

$check1 = array_is_list($arr1);
$check2 = array_is_list($arr2);

var_dump($check1);
var_dump($check2);
```

**Output:** `true` then `false`

---

## 3. Access (Internal Pointer)

### `current()`, `next()`, `prev()`, `reset()`, `end()`, `key()`

**Definition:** These functions move and read PHP's internal array pointer.

```php
<?php
$arr = [10, 20, 30];

$c1 = current($arr); // pointer starts at first element
echo $c1 . "\n";      // 10

$c2 = next($arr);     // moves pointer forward
echo $c2 . "\n";      // 20

$c3 = prev($arr);     // moves pointer backward
echo $c3 . "\n";      // 10

$c4 = end($arr);      // jumps to last element
echo $c4 . "\n";      // 30

$c5 = reset($arr);    // jumps back to first element
echo $c5 . "\n";      // 10

$k = key($arr);       // key of current pointer position
echo $k . "\n";       // 0
```

**Output:**

```
10
20
10
30
10
0
```

---

## 4. Search

### `in_array()`

**Definition:** Checks if a value exists anywhere in an array.

```php
<?php
$fruits = ["apple", "banana"];
$found = in_array("banana", $fruits);
var_dump($found);
```

**Output:** `bool(true)`

### `array_search()`

**Definition:** Searches for a value and returns its key.

```php
<?php
$fruits = ["apple", "banana", "cherry"];
$key = array_search("banana", $fruits);
echo $key . "\n";
```

**Output:** `1`

### `array_keys()`

**Definition:** Returns all keys of an array (optionally filtered by a value).

```php
<?php
$scores = ["a" => 1, "b" => 2, "c" => 1];

$allKeys = array_keys($scores);
$filteredKeys = array_keys($scores, 1);

print_r($allKeys);
print_r($filteredKeys);
```

**Output:**

```
["a","b","c"]
["a","c"]
```

### `array_values()`

**Definition:** Returns all values of an array, re-indexed numerically.

```php
<?php
$data = ["a" => 1, "b" => 2];
$values = array_values($data);
print_r($values);
```

**Output:** `[1, 2]`

---

## 5. Add/Remove

### `array_push()`

**Definition:** Adds elements to the end of an array.

```php
<?php
$arr = [1, 2];
array_push($arr, 3, 4);
print_r($arr);
```

**Output:** `[1, 2, 3, 4]`

### `array_pop()`

**Definition:** Removes and returns the last element.

```php
<?php
$arr = [1, 2, 3];
$removed = array_pop($arr);

echo $removed . "\n";
print_r($arr);
```

**Output:**

```
3
[1, 2]
```

### `array_shift()`

**Definition:** Removes and returns the first element, re-indexing the rest.

```php
<?php
$arr = [1, 2, 3];
$removed = array_shift($arr);

echo $removed . "\n";
print_r($arr);
```

**Output:**

```
1
[2, 3]
```

### `array_unshift()`

**Definition:** Adds elements to the beginning of an array.

```php
<?php
$arr = [2, 3];
array_unshift($arr, 0, 1);
print_r($arr);
```

**Output:** `[0, 1, 2, 3]`

### `unset()`

**Definition:** Removes a specific element by key (leaves a gap).

```php
<?php
$arr = ["a", "b", "c"];
unset($arr[1]);
print_r($arr);
```

**Output:** `[0=>"a", 2=>"c"]`

---

## 6. Merge/Split

### `array_merge()`

**Definition:** Combines arrays; string keys overwrite, numeric keys re-index.

```php
<?php
$a = ["x", "y"];
$b = ["z", "w"];
$merged = array_merge($a, $b);
print_r($merged);
```

**Output:** `[x, y, z, w]`

### `array_merge_recursive()`

**Definition:** Merges shared string keys into arrays instead of overwriting.

```php
<?php
$a = ["fruit" => ["apple"]];
$b = ["fruit" => ["banana"]];
$merged = array_merge_recursive($a, $b);
print_r($merged);
```

**Output:** `["fruit"=>["apple","banana"]]`

### `array_combine()`

**Definition:** Creates an array using one array as keys, another as values.

```php
<?php
$keys = ["name", "age"];
$values = ["Alice", 25];
$combined = array_combine($keys, $values);
print_r($combined);
```

**Output:** `["name"=>"Alice", "age"=>25]`

### `array_chunk()`

**Definition:** Splits an array into smaller chunks.

```php
<?php
$nums = [1, 2, 3, 4, 5];
$chunks = array_chunk($nums, 2);
print_r($chunks);
```

**Output:** `[[1,2],[3,4],[5]]`

### `array_slice()`

**Definition:** Extracts a portion of an array (original unchanged).

```php
<?php
$nums = [1, 2, 3, 4, 5];
$slice = array_slice($nums, 1, 3);
print_r($slice);
```

**Output:** `[2, 3, 4]`

### `array_splice()`

**Definition:** Removes/replaces a portion of an array (modifies original directly).

```php
<?php
$nums = [1, 2, 3, 4, 5];
$removed = array_splice($nums, 1, 2, ["X"]);

print_r($nums);    // modified original
print_r($removed); // the removed part
```

**Output:**

```
[1, X, 4, 5]
[2, 3]
```

---

## 7. Sorting

```php
<?php
$nums = [3, 1, 2];
sort($nums);
print_r($nums); // [1,2,3]
```

**Definition of `sort()`:** Sorts values ascending, re-indexes keys.

```php
<?php
$nums = [1, 2, 3];
rsort($nums);
print_r($nums); // [3,2,1]
```

**Definition of `rsort()`:** Sorts values descending, re-indexes keys.

```php
<?php
$ages = ["Bob" => 30, "Alice" => 25];
asort($ages);
print_r($ages); // Alice=>25, Bob=>30
```

**Definition of `asort()`:** Sorts values ascending, **keeps** keys.

```php
<?php
$ages = ["Alice" => 25, "Bob" => 30];
arsort($ages);
print_r($ages); // Bob=>30, Alice=>25
```

**Definition of `arsort()`:** Sorts values descending, keeps keys.

```php
<?php
$ages = ["Bob" => 30, "Alice" => 25];
ksort($ages);
print_r($ages); // Alice=>25, Bob=>30 (by key A-Z)
```

**Definition of `ksort()`:** Sorts by key ascending.

```php
<?php
$ages = ["Alice" => 25, "Bob" => 30];
krsort($ages);
print_r($ages); // Bob=>30, Alice=>25 (by key Z-A)
```

**Definition of `krsort()`:** Sorts by key descending.

```php
<?php
$nums = [5, 2, 8];
usort($nums, fn($a, $b) => $b <=> $a);
print_r($nums); // [8, 5, 2]
```

**Definition of `usort()`:** Sorts values using custom callback logic, re-indexes.

```php
<?php
$ages = ["Bob" => 30, "Alice" => 25];
uasort($ages, fn($a, $b) => $a <=> $b);
print_r($ages); // Alice=>25, Bob=>30 (keeps keys)
```

**Definition of `uasort()`:** Sorts values using custom logic, keeps keys.

```php
<?php
$ages = ["Bob" => 30, "Alice" => 25];
uksort($ages, fn($a, $b) => strcmp($a, $b));
print_r($ages); // Alice=>25, Bob=>30 (sorted by key)
```

**Definition of `uksort()`:** Sorts by key using custom callback logic.

---

## 8. Filtering

### `array_filter()`

**Definition:** Keeps only elements passing a callback test (keeps original keys).

```php
<?php
$nums = [1, 2, 3, 4, 5, 6];
$even = array_filter($nums, fn($n) => $n % 2 === 0);
print_r($even);
```

**Output:** `[1=>2, 3=>4, 5=>6]`

---

## 9. Mapping

### `array_map()`

**Definition:** Applies a callback to every element, returning a new array.

```php
<?php
$nums = [1, 2, 3];
$squared = array_map(fn($n) => $n * $n, $nums);
print_r($squared);
```

**Output:** `[1, 4, 9]`

---

## 10. Reduce

### `array_reduce()`

**Definition:** Reduces all elements to a single value using a callback.

```php
<?php
$nums = [1, 2, 3, 4];
$sum = array_reduce($nums, fn($carry, $n) => $carry + $n, 0);
echo $sum . "\n";
```

**Output:** `10`

---

## 11. Walk

### `array_walk()`

**Definition:** Applies a callback to each element **by reference**, modifying the array.

```php
<?php
$nums = [1, 2, 3];
array_walk($nums, function (&$val) {
    $val *= 10;
});
print_r($nums);
```

**Output:** `[10, 20, 30]`

### `array_walk_recursive()`

**Definition:** Same as `array_walk()`, but also enters nested arrays.

```php
<?php
$nums = [1, [2, 3]];
array_walk_recursive($nums, function (&$val) {
    $val *= 10;
});
print_r($nums);
```

**Output:** `[10, [20, 30]]`

---

## 12. Comparison

### `array_diff()`

**Definition:** Returns values from the first array not present in the others.

```php
<?php
$a = [1, 2, 3];
$b = [2, 3];
$diff = array_diff($a, $b);
print_r($diff);
```

**Output:** `[0=>1]`

### `array_intersect()`

**Definition:** Returns values present in all given arrays.

```php
<?php
$a = [1, 2, 3];
$b = [2, 3, 4];
$common = array_intersect($a, $b);
print_r($common);
```

**Output:** `[1=>2, 2=>3]`

### `array_udiff()`

**Definition:** Like `array_diff()`, using a custom comparison callback.

```php
<?php
$a = [["id" => 1], ["id" => 2]];
$b = [["id" => 2]];

$diff = array_udiff($a, $b, fn($x, $y) => $x["id"] <=> $y["id"]);
print_r($diff);
```

**Output:** `[["id"=>1]]`

### `array_uintersect()`

**Definition:** Like `array_intersect()`, using a custom comparison callback.

```php
<?php
$a = [["id" => 1], ["id" => 2]];
$b = [["id" => 2]];

$common = array_uintersect($a, $b, fn($x, $y) => $x["id"] <=> $y["id"]);
print_r($common);
```

**Output:** `[["id"=>2]]`

---

## 13. Unique

### `array_unique()`

**Definition:** Removes duplicate values (keeps first occurrence and its key).

```php
<?php
$nums = [1, 2, 2, 3, 3, 3];
$unique = array_unique($nums);
print_r($unique);
```

**Output:** `[0=>1, 1=>2, 3=>3]`

---

## 14. Reverse/Flip

### `array_reverse()`

**Definition:** Reverses the order of elements.

```php
<?php
$nums = [1, 2, 3];
$reversed = array_reverse($nums);
print_r($reversed);
```

**Output:** `[3, 2, 1]`

### `array_flip()`

**Definition:** Swaps keys and values.

```php
<?php
$letters = ["a", "b", "c"];
$flipped = array_flip($letters);
print_r($flipped);
```

**Output:** `["a"=>0, "b"=>1, "c"=>2]`

---

## 15. Random

### `array_rand()`

**Definition:** Returns one or more random keys from an array.

```php
<?php
$fruits = ["apple", "banana", "cherry"];
$randomKey = array_rand($fruits);
echo $randomKey . "\n"; // e.g. 1
```

### `shuffle()`

**Definition:** Randomly reorders all elements (modifies original, loses keys).

```php
<?php
$nums = [1, 2, 3, 4];
shuffle($nums);
print_r($nums); // e.g. [3, 1, 4, 2]
```

---

## 16. Mathematics

### `array_sum()`

**Definition:** Returns the sum of all values.

```php
<?php
$nums = [1, 2, 3];
$total = array_sum($nums);
echo $total . "\n";
```

**Output:** `6`

### `array_product()`

**Definition:** Returns the product (multiplication) of all values.

```php
<?php
$nums = [1, 2, 3, 4];
$product = array_product($nums);
echo $product . "\n";
```

**Output:** `24`

---

## 17. Extraction

### `extract()`

**Definition:** Creates individual variables from an associative array's keys/values.

```php
<?php
$data = ["name" => "Alice", "age" => 25];
extract($data);
echo "$name is $age\n";
```

**Output:** `Alice is 25`

---

## 18. Variable to Array

### `compact()`

**Definition:** Opposite of `extract()` — builds an array from variable names.

```php
<?php
$city = "Dhaka";
$country = "Bangladesh";
$location = compact('city', 'country');
print_r($location);
```

**Output:** `["city"=>"Dhaka", "country"=>"Bangladesh"]`

---

## 19. Replace

### `array_replace()`

**Definition:** Replaces values in the first array using later arrays (matched by key).

```php
<?php
$a = ["a" => 1, "b" => 2];
$b = ["b" => 20, "c" => 3];
$result = array_replace($a, $b);
print_r($result);
```

**Output:** `["a"=>1, "b"=>20, "c"=>3]`

### `array_replace_recursive()`

**Definition:** Like `array_replace()`, but merges nested arrays instead of fully replacing them.

```php
<?php
$a = ["user" => ["name" => "Alice", "age" => 25]];
$b = ["user" => ["age" => 30]];
$result = array_replace_recursive($a, $b);
print_r($result);
```

**Output:** `["user"=>["name"=>"Alice", "age"=>30]]`

---

## 20. Padding

### `array_pad()`

**Definition:** Pads an array to a given length with a value (positive = pad right, negative = pad left).

```php
<?php
$nums = [1, 2];
$paddedRight = array_pad($nums, 5, 0);
$paddedLeft = array_pad($nums, -5, 0);

print_r($paddedRight);
print_r($paddedLeft);
```

**Output:**

```
[1, 2, 0, 0, 0]
[0, 0, 0, 1, 2]
```

---

## 21. Counting

### `array_count_values()`

**Definition:** Counts how many times each value appears in an array.

```php
<?php
$letters = ["a", "b", "a", "c", "a"];
$counts = array_count_values($letters);
print_r($counts);
```

**Output:** `["a"=>3, "b"=>1, "c"=>1]`

---

## 22. Column

### `array_column()`

**Definition:** Extracts a single "column" of values from a multidimensional array of records.

```php
<?php
$records = [
    ["id" => 1, "name" => "Alice"],
    ["id" => 2, "name" => "Bob"],
];

$names = array_column($records, "name");
$namesById = array_column($records, "name", "id");

print_r($names);
print_r($namesById);
```

**Output:**

```
["Alice", "Bob"]
[1=>"Alice", 2=>"Bob"]
```

---

Want **string functions** next in this same complete, variable-based style?