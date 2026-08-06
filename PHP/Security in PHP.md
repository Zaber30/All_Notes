# PHP Security Basics — Quick Notes

---

## 1. SQL Injection & PDO Prepared Statements

**Definition:** SQL Injection = attacker inserts malicious SQL into your query through user input, to steal/delete/modify data.

**Real-life analogy:** Imagine a hotel form asking "Your Name?" and instead of a name, someone writes "John; also give me the master key to every room." If the receptionist blindly follows instructions written on the form, disaster happens. Prepared statements = the receptionist treats whatever is written strictly as _a name_, never as a command.

**Fix:** Use `PDO` with prepared statements (placeholders `?` or `:name`) — data is never mixed with SQL code.

```php
$pdo = new PDO("mysql:host=localhost;dbname=test", "user", "pass");
$stmt = $pdo->prepare("SELECT * FROM users WHERE email = :email");
$stmt->execute(['email' => $_POST['email']]);
$user = $stmt->fetch();
```

❌ Never do: `"SELECT * FROM users WHERE email = '$_POST[email]'"`

---

## 2. XSS (Cross-Site Scripting) — `htmlspecialchars()`, `htmlentities()`

**Definition:** XSS = attacker injects malicious `<script>` into a page, which runs in another user's browser (steals cookies, sessions, etc.)

**Real-life analogy:** A guestbook website where visitors leave comments. If someone writes `<script>stealCookies()</script>` and you display it raw, every visitor who views that comment unknowingly runs the attacker's code.

**Fix:** Escape output before printing user data into HTML.

```php
$comment = "<script>alert('hacked')</script>";
echo htmlspecialchars($comment, ENT_QUOTES, 'UTF-8');
// Output: &lt;script&gt;alert('hacked')&lt;/script&gt; (harmless text, not executed)
```

- `htmlspecialchars()` → escapes `< > & " '` (most common, use this).
- `htmlentities()` → escapes even more characters (accents, symbols) — use for full HTML entity encoding.

**Rule of thumb:** Always escape data **on output**, not on input.

---

## 3. CSRF (Cross-Site Request Forgery) Basics

**Definition:** CSRF = a malicious site tricks a logged-in user's browser into submitting a request (like "transfer money") to your site _without the user's knowledge_, using their existing session/cookies.

**Real-life analogy:** You're logged into your bank in one tab. In another tab, a shady website has a hidden button that says "Claim free prize!" but is actually wired to submit a money-transfer form to your bank — and since your bank cookie is already active, the bank thinks it's really you.

**Fix:** Use a **CSRF token** — a secret random value tied to the user's session, included in every form, and verified on submit.

```php
// Generate token (store in session)
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}
```

```html
<form method="POST">
  <input type="hidden" name="csrf_token" value="<?= $_SESSION['csrf_token'] ?>">
</form>
```

```php
// On form submit, verify
if (!hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'])) {
    die('Invalid CSRF token');
}
```

---

## 4. Password Hashing — `password_hash()`, `password_verify()` ⭐

**Definition:** Never store plain-text passwords. Hashing = one-way scrambling so even you (the developer/DB admin) can't see the real password.

**Real-life analogy:** Like a paper shredder — you can shred a document (hash it), but you can't un-shred it back into the original. To "check" a password, you shred the entered password again and compare the shredded pieces, not the original text.

```php
// When user registers
$hashed = password_hash($_POST['password'], PASSWORD_DEFAULT);
// Store $hashed in database

// When user logs in
if (password_verify($_POST['password'], $hashed)) {
    echo "Login success!";
} else {
    echo "Wrong password!";
}
```

`PASSWORD_DEFAULT` currently uses **bcrypt** and may change in future PHP versions (which is why it's recommended — always up to date).

---

## 5. `password_needs_rehash()`

**Definition:** Checks if a stored password hash was made with an **older/weaker algorithm/cost**, so you can upgrade it silently.

**Real-life analogy:** Like renewing an old passport with outdated security features to the newer, more secure version — but only when the user logs in (you can't "upgrade" a hash without knowing the plain password).

```php
if (password_verify($_POST['password'], $hashedFromDB)) {
    if (password_needs_rehash($hashedFromDB, PASSWORD_DEFAULT)) {
        $newHash = password_hash($_POST['password'], PASSWORD_DEFAULT);
        // update $newHash in database
    }
}
```

---

## 6. `random_bytes()`, `random_int()` for Cryptography

**Definition:** Generate **cryptographically secure** random values — unpredictable even to attackers (unlike `rand()` or `mt_rand()`, which are predictable/weak).

**Real-life analogy:** `rand()` is like shuffling cards in a very predictable pattern a magician could guess. `random_bytes()` / `random_int()` is like a casino-grade shuffling machine — truly unpredictable.

**Use for:** tokens, password reset links, CSRF tokens, session IDs, OTPs.

```php
$token = bin2hex(random_bytes(16));      // secure random string (for tokens/links)
$otp   = random_int(100000, 999999);     // secure random number (for OTP codes)
```

❌ Never use `rand()`/`mt_rand()` for security-related randomness.

---

## 7. `hash()`, `hash_hmac()`

**Definition:**

- `hash()` → generates a one-way fingerprint (checksum) of data — used for integrity checks (e.g., "did this file change?").
- `hash_hmac()` → same, but combined with a **secret key**, so only someone with the key could have generated it — used to verify authenticity (e.g., "did this data really come from my server, or was it tampered with?").

**Real-life analogy:**

- `hash()` = a wax seal on a letter — proves the letter wasn't altered, but anyone can make a seal.
- `hash_hmac()` = a wax seal made with **your personal signet ring** (secret key) — proves it specifically came from you.

```php
$fileHash = hash('sha256', file_get_contents('data.txt')); // integrity check

$secretKey = 'my-secret-key';
$signature = hash_hmac('sha256', 'order_id=123&amount=500', $secretKey);
// Used often in webhook verification (e.g., payment gateways like Stripe, bKash)
```

---

## 8. Session Hijacking Prevention

**Definition:** Session hijacking = attacker steals a user's session ID (cookie) and impersonates them without needing a password.

**Real-life analogy:** Your session ID is like a wristband at a concert — whoever wears it gets VIP access, no ID check needed. If someone copies your wristband, they get in as "you."

**Basic prevention checklist:**

```php
// 1. Use secure session cookie settings
session_set_cookie_params([
    'httponly' => true,   // JS (document.cookie) can't read the cookie → blocks XSS theft
    'secure'   => true,   // only sent over HTTPS
    'samesite' => 'Strict' // blocks CSRF-style cross-site sending
]);
session_start();

// 2. Regenerate session ID after login (prevents session fixation)
session_regenerate_id(true);

// 3. Optionally bind session to IP/User-Agent for extra check
```

---

## 9. Directory Traversal Prevention

**Definition:** Attacker manipulates a file path (like `../../etc/passwd`) to access files outside the intended folder.

**Real-life analogy:** A hotel guest is only supposed to access their own room (folder), but by typing "go back two floors and enter room 101" (`../../`) they sneak into the manager's office and read private files.

**Fix:** Never trust user input directly in file paths — validate/sanitize it.

```php
$filename = basename($_GET['file']); // strips any "../" path tricks
$path = '/var/www/uploads/' . $filename;

if (!file_exists($path) || !str_starts_with(realpath($path), '/var/www/uploads/')) {
    die('Invalid file');
}
```

---

## 10. `filter_var()`, `filter_input()`

**Definition:** Built-in PHP functions to **validate** (check format is correct) and **sanitize** (clean unwanted characters) user input, like emails, URLs, integers.

**Real-life analogy:** Like a bouncer at a club checking ID (validate = "is this actually a real email format?") and also politely removing any weapons before letting someone in (sanitize = "strip out dangerous characters").

```php
// Validation
$email = "test@example.com";
if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
    echo "Valid email!";
}

// Sanitization
$dirty = "<b>Hello</b> World";
$clean = filter_var($dirty, FILTER_SANITIZE_STRING); // removes/encodes tags (deprecated in PHP 8.1+, use htmlspecialchars instead)

// filter_input() reads directly from GET/POST/COOKIE superglobals safely
$age = filter_input(INPUT_GET, 'age', FILTER_VALIDATE_INT);
if ($age === false) {
    echo "Invalid age!";
}
```

---

## ⭐ Quick Cheat Sheet Summary

|Threat|Defense|
|---|---|
|SQL Injection|PDO prepared statements|
|XSS|`htmlspecialchars()` on output|
|CSRF|Random token per form + `hash_equals()`|
|Weak passwords|`password_hash()` + `password_verify()`|
|Old password hashes|`password_needs_rehash()`|
|Predictable randomness|`random_bytes()`, `random_int()`|
|Data tampering|`hash()`, `hash_hmac()`|
|Session theft|`httponly`, `secure`, `samesite`, `session_regenerate_id()`|
|Path manipulation|`basename()`, `realpath()` check|
|Bad/malicious input|`filter_var()`, `filter_input()`|

---

_Golden Rule: Never trust user input. Validate on input, escape on output, hash secrets, use secure randomness._