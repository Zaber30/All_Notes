# SOLID Principles — PHP & Laravel Guide

---

## 🏠 The Big Picture Analogy

Think of SOLID like building a **house**:

- **S**: Each room has one job (kitchen cooks, bedroom sleeps) — don't put a shower in the kitchen.
- **O**: You can add a new room (extension) without demolishing existing walls.
- **L**: Any "door" brand you install must work like every other door — push, it opens.
- **I**: You don't force the plumber to also carry an electrician's full toolkit.
- **D**: The house depends on "electricity" (the abstract plug socket), not on one specific power plant.

---

## S — Single Responsibility Principle (SRP) ⭐

> **A class should have only one reason to change.**

### 🌍 Real-life example

A **restaurant waiter** takes your order, but doesn't cook the food, doesn't wash dishes, and doesn't handle billing at the register. If the waiter did all four jobs, training a new waiter would be a nightmare, and a change in the billing system would force you to retrain every waiter too.

### ❌ Violation (Fat Controller/Model)

```php
class OrderController extends Controller
{
    public function store(Request $request)
    {
        // 1. Validate
        $request->validate(['product_id' => 'required', 'qty' => 'required']);

        // 2. Calculate price
        $product = Product::find($request->product_id);
        $total = $product->price * $request->qty;

        // 3. Save order
        $order = Order::create([...]);

        // 4. Send email
        Mail::to($request->user())->send(new OrderConfirmation($order));

        // 5. Log
        Log::info("Order placed: {$order->id}");

        return response()->json($order);
    }
}
```

This controller has **5 reasons to change**: validation rules, pricing logic, DB structure, email templates, logging format.

### ✅ Fixed (Thin Controller, Fat... Service)

```php
class OrderController extends Controller
{
    public function __construct(private OrderService $orderService) {}

    public function store(StoreOrderRequest $request)
    {
        $order = $this->orderService->placeOrder($request->validated());
        return response()->json($order);
    }
}

class OrderService
{
    public function placeOrder(array $data): Order
    {
        $order = Order::create($data);
        event(new OrderPlaced($order)); // listeners handle email/logging separately
        return $order;
    }
}
```

### 🥊 "Fat Model vs Thin Model" debate

- **Fat Model** camp: put business logic in the Eloquent model (`$order->markAsPaid()`), keep controllers thin.
- **Thin Model** camp: models should only represent data/relationships; business logic belongs in **Service classes** or **Actions**.
- **Practical rule of thumb**: If logic is about _the entity's own state_ (e.g., `$user->isAdmin()`), it can live in the model. If it _orchestrates multiple things_ (payments, emails, external APIs), it belongs in a Service/Action class.

---

## O — Open/Closed Principle (OCP) ⭐

> **Software should be open for extension, but closed for modification.**

### 🌍 Real-life example

A **power strip (extension board)**. You don't rewire your house every time you buy a new appliance — you just plug it into the socket. The socket (interface) stays the same; new devices (extensions) plug in freely.

### ❌ Violation

```php
class DiscountCalculator
{
    public function calculate(string $type, float $price): float
    {
        if ($type === 'student') {
            return $price * 0.9;
        } elseif ($type === 'senior') {
            return $price * 0.8;
        } elseif ($type === 'vip') { // every new type = editing this class
            return $price * 0.7;
        }
        return $price;
    }
}
```

### ✅ Fixed (Strategy Pattern via Interface)

```php
interface DiscountStrategy
{
    public function apply(float $price): float;
}

class StudentDiscount implements DiscountStrategy
{
    public function apply(float $price): float { return $price * 0.9; }
}

class SeniorDiscount implements DiscountStrategy
{
    public function apply(float $price): float { return $price * 0.8; }
}

class DiscountCalculator
{
    public function calculate(DiscountStrategy $strategy, float $price): float
    {
        return $strategy->apply($price);
    }
}

// Adding VIP discount later? Just add a new class. No existing code touched.
class VipDiscount implements DiscountStrategy
{
    public function apply(float $price): float { return $price * 0.7; }
}
```

---

## L — Liskov Substitution Principle (LSP) ⭐

> **Subtypes must be substitutable for their base types without breaking the program.**

### 🌍 Real-life example

If you order a **"vehicle"** for a delivery job, a **car** or a **motorbike** should both work fine as replacements. But if a **bicycle** claims to be a "vehicle" yet can't carry the required 50kg package or drive on the highway, substituting it breaks the delivery contract — even though it's technically "a vehicle."

### ❌ Violation

```php
class Bird
{
    public function fly(): string { return "Flying high!"; }
}

class Penguin extends Bird
{
    public function fly(): string
    {
        throw new Exception("Penguins can't fly!"); // breaks the contract
    }
}

function makeBirdFly(Bird $bird)
{
    echo $bird->fly(); // crashes when a Penguin is passed
}
```

### ✅ Fixed (Correct Abstraction)

```php
interface Bird {}

interface FlyingBird extends Bird
{
    public function fly(): string;
}

interface SwimmingBird extends Bird
{
    public function swim(): string;
}

class Eagle implements FlyingBird
{
    public function fly(): string { return "Flying high!"; }
}

class Penguin implements SwimmingBird
{
    public function swim(): string { return "Swimming fast!"; }
}
```

Now no class promises behavior it can't deliver.

### Pre/Post-conditions & Variance (simplified)

- **Precondition**: A subclass method **cannot demand more** than the parent (e.g., parent accepts any `int`, child can't suddenly require `int > 0` only).
- **Postcondition**: A subclass method **cannot deliver less** than the parent promised (e.g., parent guarantees a non-null return, child can't return null).
- **Covariance**: A subclass method _can_ return a **more specific** type than the parent (PHP 7.4+ supports this).
- **Contravariance**: A subclass method _can_ accept a **more general** parameter type than the parent.

```php
class Animal {}
class Dog extends Animal {}

class AnimalShelter
{
    public function adopt(): Animal { return new Animal(); }
}

class DogShelter extends AnimalShelter
{
    public function adopt(): Dog { return new Dog(); } // ✅ Covariant return — allowed
}
```

---

## I — Interface Segregation Principle (ISP) ⭐

> **Clients shouldn't be forced to depend on methods they don't use.**

### 🌍 Real-life example

A **gym membership**. If the gym only offered one giant "All-Access Platinum" plan that includes swimming, boxing, and yoga, someone who only wants to lift weights is forced to pay for and "implement" pool access they'll never use. Better: separate small membership plans (weights-only, pool-only, classes-only) that people mix as needed.

### ❌ Violation

```php
interface Worker
{
    public function work();
    public function eat();
    public function sleep();
}

class RobotWorker implements Worker
{
    public function work() { /* ... */ }
    public function eat() { throw new Exception("Robots don't eat!"); } // forced junk method
    public function sleep() { throw new Exception("Robots don't sleep!"); }
}
```

### ✅ Fixed (Small, Role-based Interfaces)

```php
interface Workable { public function work(); }
interface Eatable  { public function eat(); }
interface Sleepable { public function sleep(); }

class HumanWorker implements Workable, Eatable, Sleepable
{
    public function work() { /* ... */ }
    public function eat() { /* ... */ }
    public function sleep() { /* ... */ }
}

class RobotWorker implements Workable
{
    public function work() { /* ... */ } // only implements what it actually needs
}
```

### Laravel example: Repository interfaces

```php
interface Readable { public function find(int $id); }
interface Writable { public function save(array $data); }

class ReportRepository implements Readable // read-only report — no forced save()
{
    public function find(int $id) { /* ... */ }
}
```

---

## D — Dependency Inversion Principle (DIP) ⭐

> **Depend on abstractions, not concretions.** High-level modules and low-level modules should both depend on interfaces.

### 🌍 Real-life example

A **wall socket** doesn't care whether you plug in a lamp, a phone charger, or a laptop. It only cares that the plug fits the standard interface. You (high-level) don't need to know how the power plant (low-level) generates electricity.

### ❌ Violation (tight coupling)

```php
class MySqlDatabase
{
    public function save(array $data) { /* mysql-specific code */ }
}

class UserService
{
    private MySqlDatabase $db;

    public function __construct()
    {
        $this->db = new MySqlDatabase(); // hardcoded — can't swap for MongoDB later
    }

    public function register(array $data)
    {
        $this->db->save($data);
    }
}
```

### ✅ Fixed (depend on interface)

```php
interface DatabaseInterface
{
    public function save(array $data);
}

class MySqlDatabase implements DatabaseInterface
{
    public function save(array $data) { /* mysql code */ }
}

class MongoDatabase implements DatabaseInterface
{
    public function save(array $data) { /* mongo code */ }
}

class UserService
{
    public function __construct(private DatabaseInterface $db) {} // depends on abstraction

    public function register(array $data)
    {
        $this->db->save($data);
    }
}
```

### 🔧 Interface Binding in Laravel (IoC Container)

Laravel's **Service Container** auto-resolves interfaces to concrete classes, so `UserService` never has to know which database it's using.

```php
// app/Providers/AppServiceProvider.php
public function register()
{
    $this->app->bind(DatabaseInterface::class, MySqlDatabase::class);
}
```

Now anywhere `DatabaseInterface` is type-hinted, Laravel automatically injects `MySqlDatabase`. Swapping to `MongoDatabase` later means changing **one line** in the provider — zero changes in `UserService`.

### Relationship to DI & IoC Container

- **Dependency Injection (DI)** = the _technique_ (passing dependencies via constructor/method instead of creating them inside the class).
- **IoC Container** = the _tool_ (Laravel's container) that automatically builds and injects those dependencies for you.
- **DIP** = the _principle_ that tells you _why_ you should do this (depend on abstractions).

Think of it as: **DIP is the rule, DI is the practice, IoC Container is the machine that enforces the practice automatically.**

---

## 🚨 Common SOLID Violations (Quick Reference)

|Violation|Symptom|
|---|---|
|God Class|One class handles auth, payments, emails, and PDF generation|
|If/elseif chains for types|Adding a new "type" means editing 5 different places|
|`instanceof` checks everywhere|Code branches based on concrete class instead of polymorphism|
|Fat interfaces|Implementing classes throw `NotImplementedException` for unused methods|
|`new` inside business logic|Classes directly instantiate dependencies instead of receiving them|
|Deep inheritance chains that break behavior|Child class overrides parent method to throw an error|

---

## 🛠️ SOLID in Laravel — Practical Application

|Principle|Laravel Tool|
|---|---|
|**S**|Form Requests (validation), Actions/Services (business logic), Jobs (background tasks)|
|**O**|Strategy pattern via interfaces + Service Container bindings, Laravel Events/Listeners|
|**L**|Contracts (`Illuminate\Contracts\*`) — any implementation must behave predictably|
|**I**|Small Repository/Contract interfaces (`Readable`, `Writable`) instead of one giant `RepositoryInterface`|
|**D**|Constructor injection + `bind()`/`singleton()` in Service Providers|

**Example — full mini flow:**

```php
// Contract (I + D)
interface PaymentGateway
{
    public function charge(float $amount): bool;
}

// Two interchangeable implementations (O + L)
class StripeGateway implements PaymentGateway
{
    public function charge(float $amount): bool { /* stripe logic */ return true; }
}

class PaypalGateway implements PaymentGateway
{
    public function charge(float $amount): bool { /* paypal logic */ return true; }
}

// Single-responsibility service (S), depends on abstraction (D)
class CheckoutService
{
    public function __construct(private PaymentGateway $gateway) {}

    public function checkout(float $amount): bool
    {
        return $this->gateway->charge($amount);
    }
}

// Binding (Laravel IoC)
$this->app->bind(PaymentGateway::class, StripeGateway::class);
```

Switching payment providers = change one binding line. No controller, no service, no business logic touched.

---

## 🔴 When NOT to Apply SOLID Strictly

SOLID is a **guideline**, not a religion. Over-applying it causes its own problems:

1. **Small scripts / prototypes / MVPs** Real-life analogy: You don't build a **multi-story building's foundation** for a **weekend garden shed**. If you're validating a business idea in a weekend, 5 interfaces and 3 service classes for one CRUD form is wasted effort.
    
2. **YAGNI conflict (You Aren't Gonna Need It)** Don't create a `PaymentGatewayInterface` with 4 implementations if you only ever use Stripe and have no business plans to add PayPal. Premature abstraction = extra complexity for a "maybe" future.
    
3. **Simple Laravel CRUD apps** A blog with `Post` model → controller → view doesn't need a `PostRepositoryInterface` + `PostService` + `PostAction` layer stack. Eloquent's ActiveRecord pattern is _intentionally_ a bit "fat model" for productivity — fighting it for purity slows you down.
    
4. **Team size & experience** Real-life analogy: A **solo food cart owner** doesn't need a corporate org chart with separate departments. If it's just you (or 2 devs) maintaining the code, heavy abstraction layers can add more cognitive overhead than they save.
    
5. **Performance-critical hot paths** Extra abstraction layers (interfaces, extra function calls) add tiny overhead. In 99% of web apps this is irrelevant, but in a tight loop processing millions of rows, sometimes a direct, "impure" approach is pragmatically better.
    

### 🎯 Rule of thumb

> Apply SOLID **where change is likely** (payment gateways, notification channels, third-party APIs) — those areas benefit from flexibility. Skip heavy abstraction for **stable, simple, rarely-changing code** (a static "About Us" page controller doesn't need a strategy pattern).

**Real-life summary analogy**: You childproof the areas of your house where a toddler actually goes (kitchen, stairs) — you don't put safety locks on a closet no one ever opens. Apply SOLID with the same targeted judgment.