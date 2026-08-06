**Dependency Injection (DI)** is a architectural design pattern (the concept), while a **DI Container** is a software tool (the framework) that automates that pattern for you.

# Dependency Injection (DI)

## Definition

> **Dependency Injection is a design pattern where an object receives its dependencies from an external source instead of creating them itself.**

### Without DI

```
class Engine
{
}

class Car
{
    private Engine $engine;

    public function __construct()
    {
        $this->engine = new Engine();
    }
}
```

### Problem

`Car` is tightly coupled to `Engine`.

```
Car
 │
 └── creates Engine
```

---

## With DI

```
class Engine
{
}

class Car
{
    public function __construct(private Engine $engine)
    {
    }
}

$engine = new Engine();
$car = new Car($engine);
```

Now:

```
Engine
   │
   ▼
Car
```

The `Car` does **not create** the `Engine`; it **receives** it.

---

## Types of DI

### 1. Constructor Injection ✅ (Most Common)

```
class UserService
{
    public function __construct(private UserRepository $repo)
    {
    }
}
```

---

### 2. Setter Injection

```
class UserService
{
    private UserRepository $repo;

    public function setRepository(UserRepository $repo)
    {
        $this->repo = $repo;
    }
}
```

---

### 3. Method Injection

```
class UserService
{
    public function save(UserRepository $repo)
    {
    }
}
```

---

# Dependency Injection Container (DI Container)

## Definition

> **A DI Container is a tool or object that automatically creates objects and injects their dependencies.**

Instead of writing:

```
$engine = new Engine();
$car = new Car($engine);
```

you ask the container:

```
$car = $container->make(Car::class);
```

The container figures out:

1. Car needs Engine.
2. Create Engine.
3. Pass Engine to Car.
4. Return Car.

---

## Example

Suppose:

```
class Engine
{
}

class Car
{
    public function __construct(Engine $engine)
    {
    }
}
```

Container resolves it:

```
Request Car
     │
     ▼
Container
     │
     ▼
Create Engine
     │
     ▼
Create Car
     │
     ▼
Return Car
```

---

# Laravel Example

Controller:

```
class UserController
{
    public function __construct(
        private UserService $service
    ) {
    }
}
```

Service:

```
class UserService
{
    public function __construct(
        private UserRepository $repo
    ) {
    }
}
```

Repository:

```
class UserRepository
{
}
```

Laravel does:

```
UserController
      │
needs UserService
      │
      ▼
UserService
      │
needs UserRepository
      │
      ▼
UserRepository
```

The Laravel **Service Container** creates everything automatically.

---

# Without DI Container

You must do it manually.

```
$repo = new UserRepository();

$service = new UserService($repo);

$controller = new UserController($service);
```

---

# With DI Container

```
$controller = app(UserController::class);
```

or

```
$controller = resolve(UserController::class);
```

Laravel automatically creates:

- `UserRepository`
- `UserService`
- `UserController`

---

# Relationship

```
          Dependency Injection
                 │
         (Design Pattern)
                 │
                 ▼
      Dependency Injection Container
        (Implements/automates DI)
```

The container exists to **automate** dependency injection.

---

# Comparison

|Dependency Injection (DI)|Dependency Injection Container|
|---|---|
|Design pattern|Framework/tool|
|Concept|Implementation|
|You inject dependencies|Automatically injects dependencies|
|Can be done manually|Does it automatically|
|No special library required|Requires a container|
|Example: `new Car($engine)`|Example: `app(Car::class)`|

# Dependency Injection (DI) ⭐ — Quick Notes

**Definition:** Dependency Injection means a class doesn't create the objects it needs by itself — instead, those objects (dependencies) are **given ("injected") to it from outside**. This makes code flexible, testable, and loosely coupled.

**Real-life analogy (overall):** Think of a **coffee machine**. Instead of the machine growing its own coffee beans (creating dependencies internally — bad), you **supply** it with beans from outside (injecting dependencies — good). Tomorrow you can supply different beans (a different brand/type) without rebuilding the machine.

---

## Why NOT to create dependencies inside a class (the problem)

```php
class Order {
    private PDO $db;

    public function __construct() {
        // ❌ Tightly coupled — Order is now stuck with this exact DB connection
        $this->db = new PDO("mysql:host=localhost;dbname=shop", "root", "1234");
    }
}
```

**Problem:** If you want to test `Order` without a real database, or switch to a different database later, you must edit the `Order` class itself. Bad design — like welding the coffee beans permanently inside the machine.

---

## 1. Constructor Injection

**Definition:** Dependencies are passed in through the class's **constructor** when the object is created. The most common and recommended type of DI.

**Real-life analogy:** Like ordering a burger with your choice of ingredients specified **at the time of ordering** (construction) — "I want a burger with cheese and no onions" — the ingredients are fixed in from the start.

```php
interface Logger {
    public function log(string $message): void;
}

class FileLogger implements Logger {
    public function log(string $message): void {
        echo "Logging to file: $message\n";
    }
}

class Order {
    // ✅ Dependency is injected via constructor, not created inside
    public function __construct(private Logger $logger) {}

    public function placeOrder() {
        // business logic...
        $this->logger->log("Order placed successfully!");
    }
}

$logger = new FileLogger();
$order = new Order($logger); // inject the dependency here
$order->placeOrder();
```

**Benefit:** Tomorrow, you can inject a `DatabaseLogger` or `EmailLogger` instead — `Order` class code never changes.

---

## 2. Method Injection

**Definition:** Dependency is passed in through a **specific method**, not the constructor — used when a dependency is only needed for one particular action, not the whole object's lifetime.

**Real-life analogy:** Like a restaurant chef (object) who doesn't own a blender permanently — instead, whenever a smoothie order comes in, someone hands the chef a blender **just for that task** (method), and it's not needed for other dishes.

```php
class ReportGenerator {
    // No dependency needed in constructor

    public function generate(Logger $logger, string $data) {
        // dependency injected directly into this method only
        $logger->log("Generating report for: $data");
        return "Report: $data";
    }
}

$report = new ReportGenerator();
$logger = new FileLogger();
echo $report->generate($logger, "Sales Data"); // inject logger only for this call
```

**Use case:** When different methods of the same class need different implementations of a dependency (e.g., one method logs to file, another logs to email — each call decides).

---

## 3. Interface Binding

**Definition:** Instead of depending on a **concrete class**, your code depends on an **interface** (a contract). Then you "bind" (tell the system) which actual class to use for that interface — usually via a **DI Container** (common in frameworks like Laravel).

**Real-life analogy:** Like a job posting that says "We need someone who can Drive" (interface `Driver`), not "We need specifically John" (concrete class). Today you hire John, tomorrow you can hire Sarah — as long as they fulfill the "Drive" contract, the company (your code) doesn't care who's used.

```php
interface PaymentGateway {
    public function pay(float $amount): string;
}

class BkashGateway implements PaymentGateway {
    public function pay(float $amount): string {
        return "Paid $amount BDT via bKash";
    }
}

class StripeGateway implements PaymentGateway {
    public function pay(float $amount): string {
        return "Paid $amount USD via Stripe";
    }
}

class Checkout {
    // ✅ Depends on the INTERFACE, not a specific gateway
    public function __construct(private PaymentGateway $gateway) {}

    public function pay(float $amount) {
        echo $this->gateway->pay($amount);
    }
}

// Binding decision happens OUTSIDE the Checkout class
$checkout = new Checkout(new BkashGateway());
$checkout->pay(500); // Paid 500 BDT via bKash

$checkout2 = new Checkout(new StripeGateway());
$checkout2->pay(20); // Paid 20 USD via Stripe
```

**With a DI Container (Laravel example concept):**

```php
// In a service provider, you "bind" the interface to a concrete class
$container->bind(PaymentGateway::class, BkashGateway::class);

// Anywhere the container builds a class needing PaymentGateway,
// it automatically injects BkashGateway — no manual `new` needed!
```

**Benefit:** Swap `BkashGateway` → `StripeGateway` in ONE place (the binding), and every class using `PaymentGateway` automatically gets the new behavior. No code inside `Checkout` ever changes.

---

## ⭐ Quick Cheat Sheet Summary

|Concept|What it means|Real-life example|
|---|---|---|
|**Without DI**|Class creates its own dependencies|Coffee machine grows its own beans (bad)|
|**Constructor Injection**|Dependency given at object creation|Burger ordered with fixed ingredients|
|**Method Injection**|Dependency given only for one specific method call|Chef borrows a blender just for a smoothie order|
|**Interface Binding**|Code depends on a contract, not a specific class|Job posting needs "a Driver," not "John specifically"|

---

_Golden Rule: Depend on abstractions (interfaces), not concrete classes. Let dependencies be "injected" from outside instead of created inside — this makes your code flexible, swappable, and easy to test._
