# PHP Strings — Complete Reference

## Core String Functions

### `strlen()`

**Definition:** Returns the length (number of bytes) of a string.

```php
<?php
$str = "Hello";
$length = strlen($str);
echo $length . "\n";
```

**Output:** `5`

### `strpos()`

**Definition:** Finds the position of the **first occurrence** of a substring (returns `false` if not found).

```php
<?php
$str = "Hello World";
$position = strpos($str, "World");
echo $position . "\n";
```

**Output:** `6`

### `strrpos()`

**Definition:** Finds the position of the **last occurrence** of a substring.

```php
<?php
$str = "banana";
$position = strrpos($str, "a");
echo $position . "\n";
```

**Output:** `5`

### `substr()`

**Definition:** Extracts a portion of a string, given a starting position and optional length.

```php
<?php
$str = "Hello World";
$part = substr($str, 6, 5);
echo $part . "\n";
```

**Output:** `World`

### `str_replace()`

**Definition:** Replaces all occurrences of a search string with a replacement string.

```php
<?php
$str = "I like cats";
$result = str_replace("cats", "dogs", $str);
echo $result . "\n";
```

**Output:** `I like dogs`

### `str_ireplace()`

**Definition:** Case-insensitive version of `str_replace()`.

```php
<?php
$str = "I like CATS";
$result = str_ireplace("cats", "dogs", $str);
echo $result . "\n";
```

**Output:** `I like dogs`

### `explode()`

**Definition:** Splits a string into an array using a delimiter.

```php
<?php
$str = "apple,banana,cherry";
$parts = explode(",", $str);
print_r($parts);
```

**Output:** `["apple", "banana", "cherry"]`

### `implode()`

**Definition:** Joins array elements into a single string using a delimiter (alias: `join()`).

```php
<?php
$fruits = ["apple", "banana", "cherry"];
$result = implode(", ", $fruits);
echo $result . "\n";
```

**Output:** `apple, banana, cherry`

### `trim()`

**Definition:** Removes whitespace (or specified characters) from **both ends** of a string.

```php
<?php
$str = "   Hello World   ";
$result = trim($str);
echo "[$result]\n";
```

**Output:** `[Hello World]`

### `ltrim()` / `rtrim()`

**Definition:** Removes whitespace/characters from only the **left** (`ltrim`) or **right** (`rtrim`) side.

```php
<?php
$str = "   Hello World   ";
$left = ltrim($str);
$right = rtrim($str);

echo "[$left]\n";
echo "[$right]\n";
```

**Output:**

```
[Hello World   ]
[   Hello World]
```

### `strtolower()` / `strtoupper()`

**Definition:** Converts a string to all lowercase or all uppercase.

```php
<?php
$str = "Hello World";
$lower = strtolower($str);
$upper = strtoupper($str);

echo $lower . "\n";
echo $upper . "\n";
```

**Output:**

```
hello world
HELLO WORLD
```

### `ucfirst()`

**Definition:** Capitalizes only the **first letter** of a string.

```php
<?php
$str = "hello world";
$result = ucfirst($str);
echo $result . "\n";
```

**Output:** `Hello world`

### `ucwords()`

**Definition:** Capitalizes the **first letter of every word** in a string.

```php
<?php
$str = "hello world";
$result = ucwords($str);
echo $result . "\n";
```

**Output:** `Hello World`

### `lcfirst()`

**Definition:** Converts only the first letter of a string to lowercase.

```php
<?php
$str = "Hello World";
$result = lcfirst($str);
echo $result . "\n";
```

**Output:** `hello World`

### `sprintf()`

**Definition:** Formats a string using placeholders (`%s`, `%d`, `%f`, etc.) and returns the result (doesn't print directly).

```php
<?php
$name = "Alice";
$age = 25;
$result = sprintf("%s is %d years old", $name, $age);
echo $result . "\n";
```

**Output:** `Alice is 25 years old`

### `printf()`

**Definition:** Same as `sprintf()` but **prints directly** instead of returning the string.

```php
<?php
$price = 49.5;
printf("Price: $%.2f\n", $price);
```

**Output:** `Price: $49.50`

### `number_format()`

**Definition:** Formats a number with grouped thousands and specified decimal places.

```php
<?php
$number = 1234567.891;
$formatted = number_format($number, 2);
echo $formatted . "\n";
```

**Output:** `1,234,567.89`

---

## Heredoc and Nowdoc Syntax

### Heredoc

**Definition:** Multi-line string syntax that **allows variable interpolation** (like double quotes). Starts with `<<<TAG` and ends with matching `TAG;`.

```php
<?php
$name = "Alice";
$age = 25;

$text = <<<EOT
Hello, my name is $name.
I am $age years old.
EOT;

echo $text . "\n";
```

**Output:**

```
Hello, my name is Alice.
I am 25 years old.
```

### Nowdoc

**Definition:** Same as heredoc but with **no variable interpolation** (like single quotes). Tag is wrapped in single quotes: `<<<'TAG'`.

```php
<?php
$name = "Alice";

$text = <<<'EOT'
Hello, my name is $name.
This will NOT be replaced.
EOT;

echo $text . "\n";
```

**Output:**

```
Hello, my name is $name.
This will NOT be replaced.
```

---

## String Interpolation

**Definition:** Embedding variables directly inside a **double-quoted** string (or heredoc), so PHP replaces them with their values automatically.

```php
<?php
$name = "Alice";
$age = 25;

echo "Hello, $name! You are $age years old.\n";

// Complex interpolation with curly braces
$person = ["name" => "Bob"];
echo "Hello, {$person['name']}!\n";
```

**Output:**

```
Hello, Alice! You are 25 years old.
Hello, Bob!
```

**Note:** Single-quoted strings do **NOT** interpolate — `'Hello $name'` prints literally.

---

## Multibyte String Functions `mb_*`

**Definition:** Standard string functions (like `strlen`, `substr`) count **bytes**, which breaks with multi-byte characters (e.g., UTF-8 emojis, non-Latin scripts). `mb_*` functions handle **characters** correctly instead.

```php
<?php
$str = "héllo wörld"; // contains multi-byte characters

$wrongLength = strlen($str);      // counts bytes (wrong for special chars)
$correctLength = mb_strlen($str); // counts actual characters

echo $wrongLength . "\n";
echo $correctLength . "\n";

$upper = mb_strtoupper($str);
echo $upper . "\n";

$sub = mb_substr($str, 0, 5);
echo $sub . "\n";
```

**Output:**

```
13
11
HÉLLO WÖRLD
héllo
```

**Common `mb_*` functions:** `mb_strlen()`, `mb_substr()`, `mb_strtolower()`, `mb_strtoupper()`, `mb_strpos()`, `mb_str_split()`.

---

## Regular Expressions

### `preg_match()`

**Definition:** Checks if a pattern matches a string, and optionally captures matched groups.

```php
<?php
$str = "My email is alice@example.com";
$pattern = '/[\w.+-]+@[\w-]+\.[\w.-]+/';

$found = preg_match($pattern, $str, $matches);

echo $found . "\n";
print_r($matches);
```

**Output:**

```
1
["alice@example.com"]
```

### `preg_match_all()`

**Definition:** Like `preg_match()`, but finds **all** matches, not just the first.

```php
<?php
$str = "Call 123-456-7890 or 987-654-3210";
$pattern = '/\d{3}-\d{3}-\d{4}/';

preg_match_all($pattern, $str, $matches);
print_r($matches);
```

**Output:** `[["123-456-7890", "987-654-3210"]]`

### `preg_replace()`

**Definition:** Searches for a pattern and replaces it, using regex.

```php
<?php
$str = "Hello World 123";
$result = preg_replace('/[0-9]+/', '#', $str);
echo $result . "\n";
```

**Output:** `Hello World #`

### `preg_split()`

**Definition:** Splits a string into an array using a regex pattern as the delimiter.

```php
<?php
$str = "apple, banana;  cherry,orange";
$parts = preg_split('/[,;]\s*/', $str);
print_r($parts);
```

**Output:** `["apple", "banana", "cherry", "orange"]`

---

## String Padding, Repetition, Word Count

### `str_pad()`

**Definition:** Pads a string to a specified length using another string (left, right, or both sides).

```php
<?php
$str = "5";

$padRight = str_pad($str, 3, "0", STR_PAD_LEFT);
$padBoth = str_pad($str, 7, "-", STR_PAD_BOTH);

echo $padRight . "\n";
echo $padBoth . "\n";
```

**Output:**

```
005
---5---
```

### `str_repeat()`

**Definition:** Repeats a string a specified number of times.

```php
<?php
$str = "ab";
$result = str_repeat($str, 3);
echo $result . "\n";
```

**Output:** `ababab`

### `str_word_count()`

**Definition:** Counts the number of words in a string (or returns them as an array).

```php
<?php
$str = "The quick brown fox";

$count = str_word_count($str);
$words = str_word_count($str, 1); // returns array of words

echo $count . "\n";
print_r($words);
```

**Output:**

```
4
["The", "quick", "brown", "fox"]
```

---

## Bonus: Other Handy String Functions

### `str_contains()` (PHP 8+)

**Definition:** Checks if a string contains a given substring.

```php
<?php
$str = "Hello World";
$result = str_contains($str, "World");
var_dump($result);
```

**Output:** `bool(true)`

### `str_starts_with()` / `str_ends_with()` (PHP 8+)

**Definition:** Checks if a string starts or ends with a given substring.

```php
<?php
$str = "Hello World";

$starts = str_starts_with($str, "Hello");
$ends = str_ends_with($str, "World");

var_dump($starts);
var_dump($ends);
```

**Output:**

```
bool(true)
bool(true)
```

### `str_split()`

**Definition:** Splits a string into an array of chunks of a given length.

```php
<?php
$str = "HelloWorld";
$chunks = str_split($str, 3);
print_r($chunks);
```

**Output:** `["Hel", "loW", "orl", "d"]`

### `strrev()`

**Definition:** Reverses a string.

```php
<?php
$str = "Hello";
$reversed = strrev($str);
echo $reversed . "\n";
```

**Output:** `olleH`

### `wordwrap()`

**Definition:** Wraps a string to a given number of characters, adding line breaks.

```php
<?php
$str = "The quick brown fox jumps over the lazy dog";
$wrapped = wordwrap($str, 10, "\n", true);
echo $wrapped . "\n";
```

**Output:**

```
The quick
brown fox
jumps over
the lazy
dog
```

---

## Quick Summary Table

|Category|Function|One-Line Purpose|
|---|---|---|
|Length/Search|`strlen`, `strpos`, `strrpos`|Get length / find substring position|
|Extract/Replace|`substr`, `str_replace`, `str_ireplace`|Extract part / replace text|
|Split/Join|`explode`, `implode`|String ↔ array conversion|
|Trim|`trim`, `ltrim`, `rtrim`|Remove whitespace/chars from ends|
|Case|`strtolower`, `strtoupper`, `ucfirst`, `ucwords`, `lcfirst`|Change letter casing|
|Formatting|`sprintf`, `printf`, `number_format`|Format strings/numbers|
|Heredoc/Nowdoc|`<<<TAG` / `<<<'TAG'`|Multi-line strings (with/without interpolation)|
|Interpolation|`"$var"` / `"{$arr['key']}"`|Embed variables in double-quoted strings|
|Multibyte|`mb_strlen`, `mb_substr`, etc.|Correctly handle non-ASCII characters|
|Regex|`preg_match`, `preg_match_all`, `preg_replace`, `preg_split`|Pattern matching and manipulation|
|Padding/Repeat|`str_pad`, `str_repeat`|Pad to length / repeat a string|
|Word Count|`str_word_count`|Count or list words|
|Modern checks (8+)|`str_contains`, `str_starts_with`, `str_ends_with`|Substring presence checks|
|Other|`str_split`, `strrev`, `wordwrap`|Chunk / reverse / wrap text|

Want **date/time functions** next, since they pair naturally with string formatting?