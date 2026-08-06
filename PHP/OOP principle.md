## OOP Principles in PHP

### Encapsulation ⭐

**Definition:** Bundling data (properties) and behavior (methods) together, while **hiding internal details** using access modifiers (`private`/`protected`) and exposing controlled access via public methods.

php

```php
<?php
class BankAccount {
    private float $balance = 0;

    public function deposit(float $amount): void {
        if ($amount > 0) $this->balance += $amount; // validated internally
    }

    public function getBalance(): float {
        return $this->balance;
    }
}

$acc = new BankAccount();
$acc->deposit(100);
echo $acc->getBalance() . "\n"; // 100
// $acc->balance = -999; // ❌ can't bypass validation - it's private
```

**Output:**

```
100
```

---

### Abstraction ⭐

**Definition:** Hiding **complex implementation details** and showing only the essential features. The user interacts with a simple interface without knowing how it works internally.

php

```php
<?php
abstract class Coffee {
    abstract protected function brew(): string;

    public function serve(): void {
        echo $this->brew() . " is ready to drink!\n"; // hides HOW it brews
    }
}

class Espresso extends Coffee {
    protected function brew(): string {
        return "Espresso (complex grinding/pressure steps hidden)";
    }
}

(new Espresso())->serve(); // user doesn't need to know brewing details
```

**Output:**

```
Espresso (complex grinding/pressure steps hidden) is ready to drink!
```

---

### Inheritance ⭐

**Definition:** A class (`child`) reuses properties/methods from another class (`parent`) using `extends`, avoiding code duplication.

php

```php
<?php
class Animal {
    public function eat(): void { echo "Eating...\n"; }
}

class Dog extends Animal {
    public function bark(): void { echo "Barking...\n"; }
}

$dog = new Dog();
$dog->eat();  // inherited
$dog->bark(); // own method
```

**Output:**

```
Eating...
Barking...
```

---

### Polymorphism ⭐

**Definition:** The ability for **different classes** to be used through the **same interface/method call**, where each class provides its own specific behavior. "One interface, many forms."

#### Method Overriding

php

```php
<?php
class Animal {
    public function speak(): void { echo "Some sound\n"; }
}

class Cat extends Animal {
    public function speak(): void { echo "Meow\n"; }
}

class Dog extends Animal {
    public function speak(): void { echo "Woof\n"; }
}

$animals = [new Cat(), new Dog()];
foreach ($animals as $animal) {
    $animal->speak(); // same method call, different behavior
}
```

**Output:**

```
Meow
Woof
```

#### Interface Polymorphism

php

```php
<?php
interface PaymentMethod {
    public function pay(float $amount): void;
}

class CreditCard implements PaymentMethod {
    public function pay(float $amount): void {
        echo "Paid $amount via Credit Card\n";
    }
}

class PayPal implements PaymentMethod {
    public function pay(float $amount): void {
        echo "Paid $amount via PayPal\n";
    }
}

function checkout(PaymentMethod $method, float $amount): void {
    $method->pay($amount); // doesn't care WHICH class, just the interface
}

checkout(new CreditCard(), 50);
checkout(new PayPal(), 30);
```

**Output:**

```
Paid 50 via Credit Card
Paid 30 via PayPal
```

---

### Composition over Inheritance ⭐

**Definition:** Instead of building deep inheritance chains (`extends`), build objects by **combining smaller objects** ("has-a" relationship instead of "is-a"). More flexible and avoids fragile hierarchies.

php

```php
<?php
// Composition: Car HAS-A Engine (not IS-A Engine)
class Engine {
    public function start(): void { echo "Engine starting...\n"; }
}

class Car {
    private Engine $engine; // composed, not inherited

    public function __construct() {
        $this->engine = new Engine();
    }

    public function start(): void {
        $this->engine->start();
        echo "Car is ready to drive!\n";
    }
}

(new Car())->start();
```

**Output:**

```
Engine starting...
Car is ready to drive!
```

**Why better than inheritance here:** A `Car` isn't really "a type of" `Engine` — it just _uses_ one. Composition avoids forcing unnatural parent-child relationships.

---

### Coupling — Tight vs Loose ⭐

**Definition:** **Coupling** measures how much one class **depends on** the details of another.

- **Tight coupling** → classes are hardwired together; changing one breaks the other.
- **Loose coupling** → classes depend on abstractions (interfaces), making them easy to swap/change.

php

```php
<?php
// ❌ Tight coupling - MySQLDatabase is hardcoded inside
class MySQLDatabase {
    public function connect(): void { echo "Connected to MySQL\n"; }
}

class UserRepository {
    private MySQLDatabase $db;
    public function __construct() {
        $this->db = new MySQLDatabase(); // stuck with MySQL forever
    }
}
```

php

```php
<?php
// ✅ Loose coupling - depends on an interface, not a specific class
interface Database {
    public function connect(): void;
}

class MySQLDatabase implements Database {
    public function connect(): void { echo "Connected to MySQL\n"; }
}

class PostgresDatabase implements Database {
    public function connect(): void { echo "Connected to PostgreSQL\n"; }
}

class UserRepository {
    private Database $db;
    public function __construct(Database $db) { // injected - easily swappable
        $this->db = $db;
    }
}

$repo1 = new UserRepository(new MySQLDatabase());
$repo2 = new UserRepository(new PostgresDatabase()); // easy swap!
```

---

### Cohesion — High vs Low

**Definition:** **Cohesion** measures how **focused** a class's responsibilities are.

- **High cohesion** → class does ONE clear job well (good).
- **Low cohesion** → class does many unrelated things (bad, hard to maintain).

php

```php
<?php
// ❌ Low cohesion - one class doing too many unrelated jobs
class UserManager {
    public function saveUser(string $name): void { /* db logic */ }
    public function sendEmail(string $to): void { /* email logic */ }
    public function generatePDF(): void { /* PDF logic */ }
}
```

php

```php
<?php
// ✅ High cohesion - each class has ONE clear responsibility
class UserRepository {
    public function saveUser(string $name): void { echo "Saving $name\n"; }
}

class EmailService {
    public function sendEmail(string $to): void { echo "Emailing $to\n"; }
}

class PdfGenerator {
    public function generatePDF(): void { echo "Generating PDF\n"; }
}
```

---

### Dependency Inversion

**Definition:** High-level classes should depend on **abstractions (interfaces)**, not on **concrete/low-level classes** directly. This is the same idea as "loose coupling," applied as a design rule (the "D" in **SOLID**).

php

```php
<?php
interface Logger {
    public function log(string $message): void;
}

class FileLogger implements Logger {
    public function log(string $message): void {
        echo "Logging to file: $message\n";
    }
}

// High-level class depends on the Logger ABSTRACTION, not FileLogger directly
class OrderService {
    public function __construct(private Logger $logger) {}

    public function placeOrder(): void {
        $this->logger->log("Order placed!");
    }
}

$service = new OrderService(new FileLogger()); // inject any Logger implementation
$service->placeOrder();
```

**Output:**

```
Logging to file: Order placed!
```

**Benefit:** Swap `FileLogger` for `DatabaseLogger` or `CloudLogger` later — `OrderService` never needs to change.

---

### Law of Demeter (Don't Talk to Strangers) 🔴

**Definition:** An object should only talk to its **direct/immediate collaborators** — not reach deep into other objects' internals (avoid long chains like `$a->b()->c()->d()`). Reduces tight coupling between unrelated parts of the system.

php

```php
<?php
// ❌ Violates Law of Demeter - reaching deep through multiple objects
class Wallet {
    public float $balance = 100;
}

class Customer {
    public Wallet $wallet;
    public function __construct() { $this->wallet = new Wallet(); }
}

class Order {
    public function checkout(Customer $customer): void {
        // reaching through Customer -> Wallet -> balance (too deep!)
        if ($customer->wallet->balance > 50) {
            echo "Order approved\n";
        }
    }
}
```

php

```php
<?php
// ✅ Follows Law of Demeter - Order only talks to Customer directly
class Wallet {
    private float $balance = 100;
    public function hasEnough(float $amount): bool {
        return $this->balance >= $amount;
    }
}

class Customer {
    private Wallet $wallet;
    public function __construct() { $this->wallet = new Wallet(); }

    // Customer exposes a simple method - hides Wallet internals
    public function canAfford(float $amount): bool {
        return $this->wallet->hasEnough($amount);
    }
}

class Order {
    public function checkout(Customer $customer): void {
        if ($customer->canAfford(50)) { // talks only to Customer, not Wallet
            echo "Order approved\n";
        }
    }
}

(new Order())->checkout(new Customer());
```

**Output:**

```
Order approved
```

---

### Quick Summary Table

|Principle|One-Line Meaning|
|---|---|
|Encapsulation|Hide data, expose controlled access|
|Abstraction|Hide complexity, show only essentials|
|Inheritance|Reuse code via parent-child relationship|
|Polymorphism|Same method call, different behavior per class|
|Composition over Inheritance|Build with "has-a" objects instead of deep "is-a" chains|
|Coupling (tight vs loose)|How dependent classes are on each other's details|
|Cohesion (high vs low)|How focused a class's responsibility is|
|Dependency Inversion|Depend on interfaces, not concrete classes|
|Law of Demeter|Only talk to direct collaborators, not "strangers" deep inside|

Want **SOLID principles** next as a full set, since Dependency Inversion is already one of the five?