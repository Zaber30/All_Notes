# PHP Internal Execution Process (Step by Step)

PHP does not directly execute PHP source code. It converts PHP code into internal instructions (opcode), then the Zend Engine executes those instructions.

## Complete Flow

```
Browser Request
       |
       ↓
Web Server (Nginx / Apache)
       |
       ↓
PHP Handler (PHP-FPM)
       |
       ↓
PHP Engine
       |
       ↓
Lexer
       |
       ↓
Tokens
       |
       ↓
Parser
       |
       ↓
AST (Abstract Syntax Tree)
       |
       ↓
Compiler
       |
       ↓
Opcode
       |
       ↓
OPcache
       |
       ↓
Zend Engine / Zend VM
       |
       ↓
Memory Management
       |
       ↓
Output Response
```

---

# Step 1: User Sends Request

Example:

```
http://example.com/index.php
```

Browser sends HTTP request.

Flow:

```
Client
  |
  ↓
Web Server
```

The web server receives the request.

---

# Step 2: Web Server Detects PHP File

Example:

```
index.php
```

Nginx or Apache sees:

```
.php extension
```

and sends it to PHP.

Example:

```
Nginx
   |
   ↓
PHP-FPM
```

The web server does not execute PHP code itself.

---

# Step 3: PHP-FPM Receives Request

PHP-FPM = PHP FastCGI Process Manager.

Responsibilities:

- manages PHP worker processes
- sends PHP code to PHP interpreter
- returns output back to web server


Architecture:

```
Nginx

  |
  ↓

PHP-FPM Worker

  |
  ↓

PHP Interpreter
```

---

# Step 4: PHP File is Loaded

PHP reads the source code.

Example:

```php
<?php

$name = "Zaber";

echo $name;

?>
```

The code is loaded into memory.

---

# Step 5: Lexical Analysis (Tokenizer)

The lexer breaks PHP code into tokens.

Example:

PHP:

```php
$name = "Zaber";
```

Tokens:

```
T_VARIABLE      $name

=

T_STRING        Zaber

;
```

The lexer only identifies pieces.

It does not understand logic.

---

# Step 6: Parsing

Parser checks PHP grammar.

It verifies:

- syntax
- brackets
- statements
- expressions


Example:

Correct:

```php
echo "Hello";
```

Wrong:

```php
echo "Hello"
```

Parser generates:

```
Syntax Error
```

---

# Step 7: AST Creation

Parser creates:

```
AST
(Abstract Syntax Tree)
```

Example:

PHP:

```php
$x = 10 + 20;
```

AST:

```
Assignment
      |
      x
      |
     Add
    /   \
   10   20
```

AST represents the meaning of code.

---

# Step 8: Compilation

Compiler converts AST into:

```
Opcode
```

Opcode is an instruction for Zend Engine.

Example:

PHP:

```php
$a = 10;

echo $a;
```

Opcode:

```
ASSIGN $a 10

ECHO $a

RETURN
```

Opcode is not CPU machine code.

It is for Zend Virtual Machine.

---

# Step 9: OPcache Check

PHP checks OPcache.

## Without OPcache

Every request:

```
Read PHP file

↓

Tokenize

↓

Parse

↓

Compile

↓

Create Opcode

↓

Execute
```

---

## With OPcache

First request:

```
PHP File

↓

Compile

↓

Store Opcode in RAM

↓

Execute
```

Next requests:

```
PHP File

↓

Load existing Opcode

↓

Execute
```

This improves performance.

---

# Step 10: Zend Engine Executes Opcode

Zend Engine is the core PHP execution engine.

It contains:

```
Zend Virtual Machine
```

It reads opcode instructions.

Example:

Opcode:

```
ASSIGN $x 100

ECHO $x
```

Zend VM:

```
Read instruction

↓

Execute instruction

↓

Move to next instruction
```

---

# Step 11: Memory Management

Zend Engine manages memory.

PHP internally stores variables using:

```
zval
```

Example:

PHP:

```php
$name = "Zaber";
```

Internal:

```
zval

type:
string

value:
"Zaber"

reference count:
1
```

Zend manages:

- variable storage
- objects
- arrays
- garbage collection

---

# Step 12: Function Execution

Example:

```php
function add($a,$b)
{
    return $a+$b;
}

echo add(5,10);
```

Internal process:

```
Call function

↓

Create stack frame

↓

Store parameters

↓

Execute function opcode

↓

Return result
```

---

# Step 13: Output Generation

Example:

```php
echo "Hello";
```

Flow:

```
Zend Engine

↓

Output Buffer

↓

PHP-FPM

↓

Nginx/Apache

↓

Browser
```

Browser receives:

```
Hello
```

---

# PHP Internal Components

```
PHP Runtime

|
|-- Lexer
|
|-- Parser
|
|-- AST
|
|-- Compiler
|
|-- Opcode
|
|-- OPcache
|
|-- Zend Engine
|
|-- Memory Manager
|
|-- Extensions
      |
      |-- MySQL
      |-- Redis
      |-- GD
```

---

# Simple Real World Analogy

PHP code is like a book.

## Lexer

Reads words.

```
PHP Code
```

↓

## Parser

Understands sentences.

```
AST
```

↓

## Compiler

Creates instructions.

```
Opcode
```

↓

## Zend Engine

Executes instructions.

```
Result
```

---

# Production PHP Execution

Modern PHP application:

```
Browser

↓

Nginx

↓

PHP-FPM

↓

OPcache

↓

Zend Engine

↓

Laravel / PHP Code

↓

Database / Redis

↓

Response
```

---

# Key Terms

| Term              | Meaning                               |
| ----------------- | ------------------------------------- |
| PHP-FPM           | Process manager that runs PHP workers |
| Lexer             | Converts source code into tokens      |
| Parser            | Checks syntax and creates structure   |
| AST               | Tree representation of code           |
| Compiler          | Converts AST into opcode              |
| Opcode            | Internal PHP instructions             |
| OPcache           | Stores compiled opcode in memory      |
| Zend Engine       | Executes PHP opcode                   |
| zval              | Internal PHP variable container       |
| Garbage Collector | Removes unused memory                 |
