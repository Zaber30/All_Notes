# PHP Error Handling — Quick Notes

---

## 1. Error Types — Notice, Warning, Fatal Error, Parse Error

**Definition:** PHP has different "severity levels" of problems. Not every error stops your script — some are just warnings.

**Real-life analogy:** Think of a car dashboard:

- **Notice** = a small blue light saying "low washer fluid" — car still runs fine, just a heads-up.
- **Warning** = an orange light saying "check engine" — something's wrong but the car still drives.
- **Fatal Error** = the engine completely stops — car can't move at all, script execution halts.
- **Parse Error** = the car won't even start because the key is broken (syntax is wrong before the car/script even runs).

| Type            | Meaning                                           | Script continues?             |
| --------------- | ------------------------------------------------- | ----------------------------- |
| **Notice**      | Minor issue (e.g., undefined variable)            | ✅ Yes                         |
| **Warning**     | Bigger issue (e.g., `include()` missing file)     | ✅ Yes                         |
| **Fatal Error** | Critical issue (e.g., calling undefined function) | ❌ No, script stops            |
| **Parse Error** | Broken syntax (e.g., missing semicolon)           | ❌ No, script won't even start |

```php
echo $undefinedVar;      // Notice: undefined variable
include('missing.php');  // Warning: file not found
nonExistentFunction();   // Fatal Error: function not found
```

---

## 2. `try`, `catch`, `finally`

**Definition:** A way to "attempt" risky code, "catch" it if it fails, and "always run" cleanup code regardless of outcome.

**Real-life analogy:** Like trying to withdraw money from an ATM:

- `try` = insert your card and attempt withdrawal.
- `catch` = if the ATM shows "insufficient funds," handle that gracefully instead of the machine breaking down.
- `finally` = the ATM **always** ejects your card back, whether the withdrawal succeeded or failed.

```php
try {
    $balance = 100;
    $withdrawAmount = 500;

    if ($withdrawAmount > $balance) {
        throw new Exception("Insufficient funds!");
    }
    echo "Withdrawal successful!";
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
} finally {
    echo "Card ejected."; // always runs
}
```

---

## 3. Multiple Catch Blocks

**Definition:** You can catch different **types** of exceptions differently, similar to sorting problems by category and handling each one uniquely.

**Real-life analogy:** A hospital's emergency room triage: chest pain patients go to cardiology, broken bones go to orthopedics, and everything else goes to general care — each problem type gets routed to the right handler.

```php
try {
    $data = json_decode('{invalid json}', true, 512, JSON_THROW_ON_ERROR);
    $result = 10 / 0;
} catch (JsonException $e) {
    echo "JSON error: " . $e->getMessage();
} catch (DivisionByZeroError $e) {
    echo "Math error: " . $e->getMessage();
} catch (Exception $e) {
    echo "General error: " . $e->getMessage();
}
```

⚠️ Order matters — put **specific** exceptions first, **general** ones last (like `Exception`).

---

## 4. Exception Hierarchy

**Definition:** All exceptions/errors in PHP extend from base classes, organized like a family tree. Understanding this helps you know what to `catch`.

**Real-life analogy:** Like a company org chart — everyone reports up to a CEO (`Throwable`). If you want to "catch" any problem regardless of department, you catch at the top (`Throwable`); if you only care about a specific department's issues, catch that specific class.

```
Throwable (interface - top of everything)
├── Error (fatal issues, e.g. TypeError, DivisionByZeroError)
│   ├── TypeError
│   ├── ValueError
│   └── DivisionByZeroError
└── Exception (recoverable issues, your normal try/catch use)
    ├── InvalidArgumentException
    ├── RuntimeException
    ├── PDOException
    └── JsonException
```

```php
try {
    strlen(); // wrong number of arguments → throws ArgumentCountError (extends Error)
} catch (Throwable $e) {
    // catches BOTH Error and Exception types
    echo "Caught: " . get_class($e) . " — " . $e->getMessage();
}
```

---

## 5. Custom Exceptions

**Definition:** You can create your **own exception classes** for specific business logic errors, making error handling more meaningful and readable.

**Real-life analogy:** Instead of a generic "Something went wrong" sign at a bank, you create specific signs: "Insufficient Funds," "Account Frozen," "Invalid PIN" — each clearly named for exactly what happened.

```php
class InsufficientFundsException extends Exception {
    private float $shortfall;

    public function __construct(float $shortfall) {
        $this->shortfall = $shortfall;
        parent::__construct("Insufficient funds! You need $shortfall more.");
    }

    public function getShortfall(): float {
        return $this->shortfall;
    }
}

try {
    $balance = 100;
    $withdraw = 150;
    if ($withdraw > $balance) {
        throw new InsufficientFundsException($withdraw - $balance);
    }
} catch (InsufficientFundsException $e) {
    echo $e->getMessage() . " Shortfall: " . $e->getShortfall();
}
```

---

## 6. `set_error_handler()`, `set_exception_handler()`

**Definition:**

- `set_error_handler()` → intercepts normal PHP errors (Notices, Warnings) and lets you handle them your own way (e.g., log to file instead of showing on screen).
- `set_exception_handler()` → catches any **uncaught exception** globally, as a last-resort safety net before the script dies.

**Real-life analogy:** Like a building's fire alarm system:

- `set_error_handler()` = a smoke detector that alerts you to small issues (smoke) before they become big fires.
- `set_exception_handler()` = the final sprinkler system that activates when everything else failed and fire is already out of control — last line of defense.

```php
// Custom handler for regular errors (Notice, Warning)
set_error_handler(function ($errno, $errstr, $errfile, $errline) {
    error_log("Error [$errno]: $errstr in $errfile on line $errline");
    // return true to prevent PHP's default error handler from also running
    return true;
});

echo $undefinedVar; // triggers custom handler instead of default notice

// Global safety net for exceptions nobody caught
set_exception_handler(function ($exception) {
    error_log("Uncaught Exception: " . $exception->getMessage());
    echo "Something went wrong. We're looking into it.";
});

throw new Exception("Oops, nobody caught me!");
```

---

## 7. `trigger_error()`

**Definition:** Lets **you** manually raise a Notice, Warning, or User-level Fatal Error inside your own code — useful for flagging bad usage of your functions/classes.

**Real-life analogy:** Like a teacher raising a hand-written warning note to a student: "Hey, you're using this the wrong way!" — without stopping the whole class (unless it's severe).

```php
function divide($a, $b) {
    if ($b === 0) {
        trigger_error("Division by zero attempted!", E_USER_WARNING);
        return null;
    }
    return $a / $b;
}

$result = divide(10, 0);
// Output: Warning: Division by zero attempted! in ... on line ...
```

|Level|Meaning|
|---|---|
|`E_USER_NOTICE`|Minor, informational|
|`E_USER_WARNING`|Something's wrong, but continue|
|`E_USER_ERROR`|Critical, stops script|

---

## ⭐ Quick Cheat Sheet Summary

|Concept|Purpose|
|---|---|
|Notice/Warning/Fatal/Parse|Know severity levels of PHP errors|
|`try/catch/finally`|Handle risky code gracefully, always clean up|
|Multiple catch blocks|Handle different error types differently|
|Exception hierarchy|`Throwable` → `Error` \| `Exception` (know what to catch)|
|Custom exceptions|Meaningful, specific error classes for your app logic|
|`set_error_handler()`|Intercept regular PHP errors (Notice/Warning)|
|`set_exception_handler()`|Global safety net for uncaught exceptions|
|`trigger_error()`|Manually raise your own warnings/notices in code|

---

_Golden Rule: Catch specific exceptions first, use `finally` for cleanup, and always have a global exception handler as your last line of defense._