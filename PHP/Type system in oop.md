# Type System in OOP — Quick Notes

---

## 1. Type-Hinting for Parameters and Return Types

**Definition:** Telling PHP exactly what data type a function/method **expects as input** (parameters) and **gives back as output** (return type) — PHP then enforces this and throws a `TypeError` if violated.

**Real-life analogy:** Like a **vending machine slot** — it's shaped specifically for coins (not paper bills or random objects). If you try to insert the wrong shape, the machine rejects it immediately instead of malfunctioning later.

```php
class Calculator {
    // Parameter type hints: int, int
    // Return type hint: int
    public function add(int $a, int $b): int {
        return $a + $b;
    }

    // Nullable type — accepts int OR null
    public function findUser(?int $id): ?string {
        return $id ? "User #$id" : null;
    }

    // Union types (PHP 8+) — accepts multiple types
    public function process(int|string $input): string {
        return "Processed: $input";
    }
}

$calc = new Calculator();
echo $calc->add(5, 10); // 15
$calc->add("5", "10");   // TypeError in strict mode, auto-converts in weak mode
```

**Why it matters:** Catches bugs early (at the function boundary) instead of deep inside your code where it's harder to trace. Also makes your code **self-documenting** — anyone reading `add(int $a, int $b): int` instantly knows what to pass and expect.

```php
declare(strict_types=1); // enforces STRICT type checking (recommended!)
```

---

## 2. Covariant Return Types 🔴

**Definition:** When you **override a method in a child class**, the child's return type is allowed to be a **more specific (narrower) subtype** than the parent's declared return type. This is called **covariance**.

**Real-life analogy:** Imagine a parent recipe book says "This recipe returns _a Dessert_." A child recipe (a specific cake recipe) can promise to return "_a Chocolate Cake_" specifically — which is fine, because a Chocolate Cake **IS-A** Dessert. You're being **more specific**, not breaking the promise.

```php
class Animal {}
class Dog extends Animal {}

class AnimalShelter {
    public function adopt(): Animal {
        return new Animal();
    }
}

class DogShelter extends AnimalShelter {
    // ✅ Covariant return type — Dog is a MORE SPECIFIC type of Animal
    public function adopt(): Dog {
        return new Dog();
    }
}

$shelter = new DogShelter();
$pet = $shelter->adopt(); // returns Dog, which IS-A Animal — totally valid
```

**Why it's allowed:** Anyone using `AnimalShelter` and expecting an `Animal` back will still be happy getting a `Dog` — because a `Dog` can do everything an `Animal` can do (and more). No broken promises.

❌ **Not allowed (would break the contract):**

```php
class DogShelter extends AnimalShelter {
    public function adopt(): string { // ❌ Error! string is NOT related to Animal
        return "a dog";
    }
}
```

---

## 3. Contravariant Parameter Types 🔴

**Definition:** When overriding a method, a child class's **parameter type** is allowed to be a **broader (more general) type** than the parent's declared parameter type. This is called **contravariance**.

**Real-life analogy:** A parent job posting says: "This role must be able to feed _a Dog_." A more flexible child version of that role says: "I can feed _any Animal_ (including dogs)." Being able to handle **more general cases** doesn't break the original promise — if you can feed any Animal, you can definitely feed a Dog specifically.

```php
class Animal {}
class Dog extends Animal {}

class DogFeeder {
    public function feed(Dog $dog): void {
        echo "Feeding a dog\n";
    }
}

class AnimalFeeder extends DogFeeder {
    // ✅ Contravariant parameter — Animal is BROADER than Dog
    public function feed(Animal $animal): void {
        echo "Feeding any animal\n";
    }
}

$feeder = new AnimalFeeder();
$feeder->feed(new Dog()); // works fine — Animal-accepting method handles Dog too
```

**Why it's allowed:** If code expects `DogFeeder::feed(Dog $dog)`, and gets `AnimalFeeder` instead (which accepts ANY Animal), it still works perfectly — because `AnimalFeeder` can handle at least everything `DogFeeder` could, plus more.

❌ **Not allowed (would break the contract — narrowing parameters):**

```php
class AnimalFeeder {
    public function feed(Animal $animal): void {}
}

class DogFeeder extends AnimalFeeder {
    public function feed(Dog $dog): void {} // ❌ Error! Narrower than parent — breaks substitutability
}
// If someone calls feed(new Cat()) expecting AnimalFeeder behavior, DogFeeder would fail
```

**Memory trick:**

- Return types → can get **narrower** (covariant) ✅ — "more specific output is fine"
- Parameter types → can get **wider** (contravariant) ✅ — "accepting more input is fine"
- Never narrow parameters, never widen return types — both would break the parent's promise.

---

## 4. Liskov Substitution Principle (LSP) in PHP Types

**Definition:** The "L" in **SOLID** principles. States: **objects of a child class should be substitutable for objects of the parent class without breaking the program.** Covariant returns and contravariant parameters are actually the _technical rules_ PHP enforces to help you follow LSP correctly.

**Real-life analogy:** If your car (parent class) has a **remote key** that starts the engine, and you build a "SmartCar" (child class), the SmartCar's key must **also** be able to start the engine the same way — maybe with extra features, but never LESS capability. Anyone who knows how to use "a car key" should be able to use the SmartCar key too, without needing special instructions.

### ✅ Good Example — Follows LSP

```php
class Bird {
    public function eat(): string {
        return "Eating food";
    }
}

class Sparrow extends Bird {
    public function fly(): string {
        return "Flying high";
    }
}

function feedBird(Bird $bird) {
    echo $bird->eat();
}

feedBird(new Sparrow()); // ✅ Works — Sparrow IS-A Bird, no surprises
```

### ❌ Bad Example — Violates LSP

```php
class Bird {
    public function fly(): string {
        return "Flying";
    }
}

class Penguin extends Bird {
    public function fly(): string {
        throw new Exception("Penguins can't fly!"); // ❌ Breaks the parent's promise!
    }
}

function makeBirdFly(Bird $bird) {
    echo $bird->fly(); // Expects ALL birds to fly successfully
}

makeBirdFly(new Penguin()); // 💥 Crashes! Violates Liskov Substitution
```

**The fix:** Don't force `Penguin` to inherit `fly()` if it can't honestly fulfill that behavior. Redesign with proper abstraction:

```php
abstract class Bird {
    abstract public function eat(): string;
}

interface FlyingBird {
    public function fly(): string;
}

class Sparrow extends Bird implements FlyingBird {
    public function eat(): string { return "Eating seeds"; }
    public function fly(): string { return "Flying high"; }
}

class Penguin extends Bird {
    public function eat(): string { return "Eating fish"; } // no fly() promised, no broken contract
}
```

**How PHP's type system enforces LSP:**

- Covariant returns + Contravariant parameters are PHP's built-in **guard rails** that stop you from accidentally breaking substitutability at the type-checking level.
- But LSP is bigger than just types — it's also about **behavior** (like the Penguin example) which PHP can't check automatically; that's on you as the developer.

---

## ⭐ Quick Cheat Sheet Summary

|Concept|Rule|Real-life Analogy|
|---|---|---|
|Type-hinting|Enforce input/output types at function boundary|Vending machine coin slot|
|Covariant return 🔴|Child method can return a MORE SPECIFIC type|"Returns a Chocolate Cake" instead of "a Dessert"|
|Contravariant parameter 🔴|Child method can accept a MORE GENERAL type|"Feeds any Animal" instead of just "a Dog"|
|Liskov Substitution|Child objects must work anywhere parent is expected, without breaking behavior|SmartCar key must work like a normal car key|

**Golden Rule:** Return types can narrow ⬇, parameter types can widen ⬆ — both directions preserve the parent class's promise. Never do the opposite (widen return / narrow parameter), and always make sure a subclass truly "IS-A" substitutable version of its parent — not just in types, but in real behavior too.