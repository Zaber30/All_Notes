### Definition

**Access modifiers** (also called **visibility keywords**) control **where** a property or method can be accessed from — inside the class only, from child classes, or from anywhere in the code. PHP has three of them: `public`, `protected`, and `private`.

### The Three Modifiers

#### 1. `public`

- Accessible from **anywhere** — inside the class, from child classes, and from outside the class.
- Default if no modifier is specified.

#### 2. `protected`

- Accessible from **within the class** and **any class that extends it** (child/subclasses).
- **Not** accessible from outside the class.

#### 3. `private`

- Accessible **only within the class it's defined in**.
- **Not** accessible from child classes or from outside.

### Quick Comparison Table

|Modifier|Same Class|Child Class|Outside Class|
|---|---|---|---|
|`public`|✅|✅|✅|
|`protected`|✅|✅|❌|
|`private`|✅|❌|❌|

### Easy Example

php

```php
<?php

class Employee {
    public string $name;         // accessible everywhere
    protected float $salary;     // accessible in this class + subclasses
    private string $ssn;         // accessible only in this class

    public function __construct(string $name, float $salary, string $ssn) {
        $this->name = $name;
        $this->salary = $salary;
        $this->ssn = $ssn;
    }

    public function getSSN(): string {
        // private property CAN be accessed from within the same class
        return $this->ssn;
    }
}

class Manager extends Employee {
    public function showSalary(): void {
        // protected property CAN be accessed from a child class
        echo "Salary: {$this->salary}\n";

        // private property CANNOT be accessed from a child class
        // echo $this->ssn; // ❌ Error
    }
}

$emp = new Employee("Alice", 50000, "123-45-6789");

echo $emp->name . "\n";        // ✅ Works — public
// echo $emp->salary;          // ❌ Error — protected, not accessible outside
// echo $emp->ssn;             // ❌ Error — private, not accessible outside

echo $emp->getSSN() . "\n";    // ✅ Works — accessed via public method

$mgr = new Manager("Bob", 70000, "987-65-4321");
$mgr->showSalary();            // ✅ Works — protected accessible in child class
```

#### Output

```
Alice
123-45-6789
Salary: 70000
```

### Key Features / Why They Matter

1. **Encapsulation** — hide internal data (like `$ssn`) so it can only be changed through controlled methods (getters/setters).
2. **Data protection** — prevents accidental or unwanted modification from outside code.
3. **Controlled inheritance** — `protected` lets subclasses reuse/extend behavior without exposing data publicly.
4. **Cleaner APIs** — `public` marks what's meant to be used by other code; everything else stays internal.

### Rule of Thumb

- Use **`private`** by default for properties (safest).
- Use **`protected`** if subclasses need direct access.
- Use **`public`** only for things meant to be used from outside the class (like methods that form the "interface" of your class).

Want a follow-up example with **getters and setters** to safely access `private` properties from outside the class?

### Definition

**Getters** and **setters** are public methods that let you **safely read** and **update** `private` (or `protected`) properties from outside the class — without giving direct access to them.

- **Getter** → a method that **returns** a property's value (e.g., `getName()`)
- **Setter** → a method that **sets/updates** a property's value (e.g., `setName()`)

This way, you control **how** data is read or changed — you can add validation, formatting, or restrictions.

### Easy Example

php

```php
<?php

class BankAccount {
    private string $owner;
    private float $balance;

    public function __construct(string $owner, float $balance = 0) {
        $this->owner = $owner;
        $this->balance = $balance;
    }

    // ----- Getters -----
    public function getOwner(): string {
        return $this->owner;
    }

    public function getBalance(): float {
        return $this->balance;
    }

    // ----- Setters -----
    public function setOwner(string $owner): void {
        $this->owner = $owner;
    }

    public function setBalance(float $balance): void {
        // Validation logic - the whole point of using a setter!
        if ($balance < 0) {
            echo "Error: Balance cannot be negative.\n";
            return;
        }
        $this->balance = $balance;
    }

    public function deposit(float $amount): void {
        if ($amount <= 0) {
            echo "Error: Deposit amount must be positive.\n";
            return;
        }
        $this->balance += $amount;
    }
}

$account = new BankAccount("Alice", 1000);

// Reading private data via getters
echo $account->getOwner() . "\n";     // Alice
echo $account->getBalance() . "\n";   // 1000

// Trying to access directly would fail:
// echo $account->balance; // ❌ Error: Cannot access private property

// Updating data via setters (with validation!)
$account->deposit(500);
echo $account->getBalance() . "\n";   // 1500

$account->setBalance(-200);           // Error: Balance cannot be negative.
echo $account->getBalance() . "\n";   // 1500 (unchanged, protected from bad data)
```

#### Output

```
Alice
1000
1500
Error: Balance cannot be negative.
1500
```

### Why Use Getters/Setters Instead of `public` Properties?

|Without getters/setters (`public $balance`)|With getters/setters (`private $balance`)|
|---|---|
|Anyone can set `$balance = -9999;` directly|Setter can **block** invalid values|
|No way to run logic when reading/writing|Can add validation, logging, formatting|
|Hard to change internal logic later|Internal structure can change without breaking outside code|

### Bonus: PHP 8.4+ Property Hooks (Modern Shortcut)

If you're using **PHP 8.4+**, there's a newer, shorter way using **property hooks** instead of separate get/set methods:

php

```php
class BankAccount {
    public float $balance {
        get => $this->balance;
        set {
            if ($value < 0) {
                throw new InvalidArgumentException("Balance cannot be negative.");
            }
            $this->balance = $value;
        }
    }
}
```

This achieves the same validation but with less boilerplate. The traditional `getX()`/`setX()` style still works everywhere and is the most common pattern you'll see in real-world PHP code.

Want to see **inheritance** next (parent/child classes with `extends`), since it naturally follows access modifiers?