# PHP Syntax & Basics

## PHP Tags — `<?php`, Short Echo `<?=`

**Definition:** PHP code must be wrapped in tags so the server knows where PHP starts/ends. `<?php` is the standard opening tag; `<?=` is a shorthand for `<?php echo`.

```php
<?php
echo "Hello from standard tag\n";
?>

<?= "Hello from short echo tag" ?>
```

**Output:**

```
Hello from standard tag
Hello from short echo tag
```

**Note:** Short echo `<?=` is always enabled (unlike the old short open tag `<?`, which is deprecated/removed).

---

## Variables — Naming, Scope, Type Juggling

### Naming Rules

**Definition:** Variables start with `$`, must begin with a letter or underscore, and are **case-sensitive**.

```php
<?php
$name = "Alice";
$_age = 25;
$user1 = "Bob";

echo $name . "\n";
echo $_age . "\n";
echo $user1 . "\n";

// $1name = "invalid";  // ❌ Error - can't start with a number
```

**Output:**

```
Alice
25
Bob
```

### Scope

**Definition:** Determines **where** a variable is accessible — `local` (inside a function), `global` (outside functions), or `static` (retains value between calls).

```php
<?php
$globalVar = "I'm global";

function showScope() {
    $localVar = "I'm local";
    echo $localVar . "\n";
    // echo $globalVar; // ❌ Error - not accessible here without 'global' keyword

    global $globalVar;
    echo $globalVar . "\n"; // ✅ now accessible
}

showScope();
```

**Output:**

```
I'm local
I'm global
```

**Static scope example:**

```php
<?php
function counter() {
    static $count = 0; // retains value between calls
    $count++;
    echo $count . "\n";
}

counter(); // 1
counter(); // 2
counter(); // 3
```

**Output:**

```
1
2
3
```

### Type Juggling (intro)

**Definition:** PHP automatically converts variable types depending on context (covered in detail further below).

```php
<?php
$num = "5" + 3; // string "5" is juggled into an integer
echo $num . "\n";
```

**Output:** `8`

---

## Constants — `define()`, `const`, Magic Constants

### `define()`

**Definition:** Creates a constant at **runtime**, works anywhere (inside conditions/functions too).

```php
<?php
define("SITE_NAME", "MyWebsite");
echo SITE_NAME . "\n";
```

**Output:** `MyWebsite`

### `const`

**Definition:** Creates a constant at **compile-time**; must be used at the top level of a file or inside a class — cannot be conditional.

```php
<?php
const MAX_USERS = 100;
echo MAX_USERS . "\n";

class Config {
    const VERSION = "1.0";
}
echo Config::VERSION . "\n";
```

**Output:**

```
100
1.0
```

### Magic Constants

**Definition:** Special predefined constants that change depending on **where** they are used in the code. They start and end with double underscores.

```php
<?php
echo __LINE__ . "\n"; // current line number
echo __FILE__ . "\n"; // full path of current file
echo __DIR__ . "\n";  // directory of current file

class Demo {
    public function show() {
        echo __CLASS__ . "\n";  // current class name
        echo __METHOD__ . "\n"; // Class::method name
    }
}

$demo = new Demo();
$demo->show();
```

**Output (example):**

```
2
/home/user/script.php
/home/user
Demo
Demo::show
```

|Magic Constant|Returns|
|---|---|
|`__LINE__`|Current line number|
|`__FILE__`|Full path + filename|
|`__DIR__`|Directory of the file|
|`__CLASS__`|Current class name|
|`__METHOD__`|Class + method name|
|`__FUNCTION__`|Current function name|

---

## Data Types

**Definition:** PHP is loosely typed — variables can hold different data types without explicit declaration.

```php
<?php
$string  = "Hello";        // string
$integer = 42;             // integer
$float   = 3.14;           // float (double)
$boolean = true;           // boolean
$nullVar = null;           // null
$array   = [1, 2, 3];      // array
$object  = new stdClass(); // object

echo gettype($string) . "\n";
echo gettype($integer) . "\n";
echo gettype($float) . "\n";
echo gettype($boolean) . "\n";
echo gettype($nullVar) . "\n";
echo gettype($array) . "\n";
echo gettype($object) . "\n";
```

**Output:**

```
string
integer
double
boolean
NULL
array
object
```

**Resource type:**

```php
<?php
$file = fopen("test.txt", "w");
echo gettype($file) . "\n"; // resource
fclose($file);
```

**Output:** `resource`

---

## Type Casting — `(int)`, `(string)`, `intval()`, `settype()`

**Definition:** Explicitly converting a value from one type to another.

### Cast operators

```php
<?php
$str = "123abc";
$num = "45.6";

$intVal = (int) $str;
$floatVal = (float) $num;
$strVal = (string) 100;
$boolVal = (bool) "";

var_dump($intVal);
var_dump($floatVal);
var_dump($strVal);
var_dump($boolVal);
```

**Output:**

```
int(123)
float(45.6)
string(3) "100"
bool(false)
```

### `intval()` / `floatval()` / `strval()` / `boolval()`

**Definition:** Functions that convert a value to a specific type (alternative to cast syntax).

```php
<?php
$value = "42.9 is the answer";

$intResult = intval($value);
$floatResult = floatval("3.14 apples");

echo $intResult . "\n";
echo $floatResult . "\n";
```

**Output:**

```
42
3.14
```

### `settype()`

**Definition:** Converts a variable's type **in place** (modifies the original variable, returns `true`/`false`).

```php
<?php
$value = "100";
settype($value, "integer");

var_dump($value);
```

**Output:** `int(100)`

---

## Type Juggling — Implicit Conversion Rules

**Definition:** PHP **automatically** converts types based on context — no manual casting needed. This happens especially with arithmetic and comparison operators.

```php
<?php
// String + Number = Number (string juggled to number)
$result1 = "10" + 5;
echo $result1 . "\n"; // 15

// Number in string context = String
$result2 = "Total: " . 100;
echo $result2 . "\n"; // Total: 100

// Boolean context
if ("0") {
    echo "truthy\n";
} else {
    echo "falsy\n"; // "0" is considered falsy!
}

if ("0.0") {
    echo "truthy\n"; // "0.0" IS truthy (only exact "0" string is falsy)
} else {
    echo "falsy\n";
}

// Loose comparison juggling
var_dump(0 == "abc");    // false in PHP 8+ (was true in PHP 7!)
var_dump("10" == "1e1"); // true - both juggled to numeric 10
var_dump(100 == "100abc"); // false in PHP 8+ (non-numeric string)
```

**Output:**

```
15
Total: 100
falsy
truthy
bool(false)
bool(true)
bool(false)
```

**Falsy values in PHP** (all evaluate to `false` in boolean context):

```php
<?php
$falsyValues = [0, 0.0, "0", "", null, false, []];

foreach ($falsyValues as $value) {
    var_dump((bool) $value);
}
```

**Output:** `bool(false)` × 7 (all of them)

---

## `var_dump()`, `print_r()`, `var_export()` Differences

### `var_dump()`

**Definition:** Shows a variable's **type AND value**, including nested structures. Best for debugging.

```php
<?php
$data = ["name" => "Alice", "age" => 25, "active" => true];
var_dump($data);
```

**Output:**

```
array(3) {
  ["name"]=>
  string(5) "Alice"
  ["age"]=>
  int(25)
  ["active"]=>
  bool(true)
}
```

### `print_r()`

**Definition:** Shows a **human-readable** structure of a variable, but **without exact types**. Good for quickly viewing array/object contents.

```php
<?php
$data = ["name" => "Alice", "age" => 25, "active" => true];
print_r($data);
```

**Output:**

```
Array
(
    [name] => Alice
    [age] => 25
    [active] => 1
)
```

### `var_export()`

**Definition:** Outputs a variable as **valid PHP code** that can be copy-pasted or `eval()`'d to recreate the variable.

```php
<?php
$data = ["name" => "Alice", "age" => 25, "active" => true];
var_export($data);
echo "\n";
```

**Output:**

```
array (
  'name' => 'Alice',
  'age' => 25,
  'active' => true,
)
```

### Comparison Table

|Function|Shows type?|Output format|Best for|
|---|---|---|---|
|`var_dump()`|✅ Yes|Verbose, typed|Debugging exact type/value|
|`print_r()`|❌ No|Simple, readable|Quick visual inspection|
|`var_export()`|✅ Yes (implicitly via syntax)|Valid PHP code|Copy-paste / regenerate code|

---

## Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|`<?php` / `<?=`|Open PHP code / shorthand for echo|
|Variable naming|`$` prefix, starts with letter/underscore, case-sensitive|
|Scope|local / global / static — where a variable is visible|
|`define()`|Runtime constant, usable conditionally|
|`const`|Compile-time constant, top-level or in classes|
|Magic constants|Context-aware built-in values (`__LINE__`, `__CLASS__`, etc.)|
|Data types|string, int, float, bool, null, array, object, resource|
|Type casting|Explicit conversion: `(int)`, `intval()`, `settype()`|
|Type juggling|PHP's automatic implicit type conversion|
|`var_dump/print_r/var_export`|Debug with types / readable view / valid PHP code output|

Want **operators** next (arithmetic, comparison, spaceship `<=>`, null coalescing `??`, etc.), since they tie directly into type juggling?