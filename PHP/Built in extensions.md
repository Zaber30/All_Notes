PHP extensions are **modules** that add classes, functions, constants, and sometimes interfaces to PHP. An extension itself does **not** have "methods"—it usually provides **functions** (and sometimes classes with methods).

Below are the most important built-in extensions and their commonly used functions.

---

# 1. Core Extension

**Purpose:** Basic PHP language functionality.

|Function|Purpose|
|---|---|
|`isset()`|Check if variable exists|
|`empty()`|Check if variable is empty|
|`define()`|Define constant|
|`defined()`|Check constant|
|`gettype()`|Get variable type|
|`is_array()`|Check array|
|`is_string()`|Check string|
|`is_int()`|Check integer|
|`var_dump()`|Display detailed information|
|`print_r()`|Print human-readable data|

---

# 2. SPL (Standard PHP Library)

**Purpose:** Data structures and iterators.

### Classes

- `ArrayObject`
- `ArrayIterator`
- `SplStack`
- `SplQueue`
- `SplHeap`
- `SplPriorityQueue`
- `SplFixedArray`

### Methods

```
$stack->push();
$stack->pop();

$queue->enqueue();
$queue->dequeue();

$arrayObject->append();
$arrayObject->count();
```

---

# 3. PDO Extension

**Purpose:** Database access.

### Class

```
PDO
PDOStatement
```

### Methods

```
$pdo->prepare()
$pdo->query()
$pdo->exec()
$pdo->beginTransaction()
$pdo->commit()
$pdo->rollBack()
$pdo->lastInsertId()

$stmt->execute()
$stmt->fetch()
$stmt->fetchAll()
$stmt->fetchColumn()
```

---

# 4. MySQLi Extension

### Functions

```
mysqli_connect()
mysqli_query()
mysqli_fetch_assoc()
mysqli_fetch_array()
mysqli_close()
```

### Methods

```
$mysqli->query()
$mysqli->prepare()
$stmt->bind_param()
$stmt->execute()
$stmt->get_result()
```

---

# 5. cURL Extension

**Purpose:** HTTP requests.

### Functions

```
curl_init()
curl_setopt()
curl_exec()
curl_error()
curl_close()
curl_getinfo()
```

---

# 6. OpenSSL Extension

**Purpose:** Encryption.

### Functions

```
openssl_encrypt()
openssl_decrypt()
openssl_sign()
openssl_verify()
openssl_random_pseudo_bytes()
```

---

# 7. mbstring Extension

**Purpose:** Multibyte strings.

### Functions

```
mb_strlen()
mb_substr()
mb_strtoupper()
mb_strtolower()
mb_convert_encoding()
```

---

# 8. GD Extension

**Purpose:** Image manipulation.

### Functions

```
imagecreate()
imagecreatetruecolor()
imagecolorallocate()
imagepng()
imagejpeg()
imagecopy()
imagedestroy()
```

---

# 9. FileInfo Extension

**Purpose:** Detect file MIME type.

### Functions

```
finfo_open()
finfo_file()
finfo_buffer()
```

---

# 10. ZIP Extension

### Class

```
ZipArchive
```

### Methods

```
$zip->open()
$zip->addFile()
$zip->addFromString()
$zip->extractTo()
$zip->close()
```

---

# 11. XML Extension

### Functions

```
xml_parser_create()
xml_parse()
xml_parser_free()
```

---

# 12. DOM Extension

### Class

```
DOMDocument
```

### Methods

```
$dom->loadXML()
$dom->loadHTML()
$dom->saveXML()
$dom->saveHTML()
$getElementById()
$getElementsByTagName()
```

---

# 13. SimpleXML Extension

### Functions

```
simplexml_load_file()
simplexml_load_string()
```

### Methods

```
$xml->children()
$xml->attributes()
$xml->xpath()
```

---

# 14. JSON Extension

### Functions

```
json_encode()
json_decode()
json_last_error()
```

---

# 15. Session Extension

### Functions

```
session_start()
session_destroy()
session_id()
session_name()
session_regenerate_id()
```

---

# 16. Date Extension

### Functions

```
date()
time()
strtotime()
mktime()
checkdate()
```

### Classes

```
DateTime
DateInterval
DatePeriod
```

### Methods

```
$date->format()
$date->modify()
$date->add()
$date->sub()
$date->diff()
```

---

# 17. Reflection Extension

### Classes

```
ReflectionClass
ReflectionMethod
ReflectionProperty
```

### Methods

```
$getMethods()
$getProperties()
$getConstructor()
$newInstance()
```

---

# 18. Filter Extension

### Functions

```
filter_var()
filter_input()
filter_has_var()
```

---

# 19. Hash Extension

### Functions

```
hash()
hash_hmac()
hash_file()
password_hash()
password_verify()
```

---

# 20. Intl Extension

### Classes

```
NumberFormatter
IntlDateFormatter
Locale
```

### Methods

```
$formatter->format()
$formatter->parse()
```

---

# 21. Socket Extension

### Functions

```
socket_create()
socket_bind()
socket_listen()
socket_accept()
socket_connect()
socket_send()
socket_recv()
socket_close()
```

---

# 22. PCRE Extension

### Functions

```
preg_match()
preg_match_all()
preg_replace()
preg_split()
preg_grep()
```

---

# 23. BCMath Extension

### Functions

```
bcadd()
bcsub()
bcmul()
bcdiv()
bccomp()
```

---

# 24. GMP Extension

### Functions

```
gmp_add()
gmp_sub()
gmp_mul()
gmp_pow()
```

---

# 25. Random Extension (PHP 8.2+)

### Functions

```
random_int()
random_bytes()
```

### Classes

```
Random\Randomizer
```

---

# Most Important Extensions for Laravel

|Extension|Common Functions/Classes|
|---|---|
|PDO|`prepare()`, `execute()`, `fetch()`|
|OpenSSL|`openssl_encrypt()`|
|mbstring|`mb_strlen()`, `mb_substr()`|
|cURL|`curl_exec()`, `curl_setopt()`|
|JSON|`json_encode()`, `json_decode()`|
|FileInfo|`finfo_file()`|
|ZIP|`ZipArchive::open()`|
|DOM|`loadHTML()`, `saveHTML()`|
|SimpleXML|`simplexml_load_file()`|
|Date|`DateTime`, `date()`|
|Filter|`filter_var()`|
|Hash|`password_hash()`, `password_verify()`|
|PCRE|`preg_match()`, `preg_replace()`|
|SPL|`SplStack`, `SplQueue`, `ArrayObject`|

### Notes

- **Extensions** provide functionality to PHP.
- An extension may expose:
    - **Functions** (e.g., `json_encode()`, `curl_exec()`).
    - **Classes and methods** (e.g., `PDO`, `DateTime`, `ZipArchive`, `DOMDocument`).
- You can see which extensions are installed with:

```
php -m
```

and get detailed information about them with:

```
php -i
```