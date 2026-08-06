### Constructor

**Definition:** A constructor is a special method that runs **automatically when an object is created**. It's used to initialize properties or set up anything the object needs from the start.

- Defined using `__construct()`
- Called automatically — you never call it manually
- Can accept parameters
- A class can have only **one** constructor

### Destructor

**Definition:** A destructor is a special method that runs **automatically when an object is destroyed** — either explicitly (`unset()`), or automatically when the script ends / no references to the object remain.

- Defined using `__destruct()`
- Called automatically — you never call it manually
- Takes **no parameters**
- Used for cleanup (closing files, database connections, logging, etc.)
```cpp
<?php

class Connection {
    public string $dbName;

    // Constructor - runs when object is created
    public function __construct(string $dbName) {
        $this->dbName = $dbName;
        echo "Connecting to database: {$this->dbName}\n";
    }

    public function query(string $sql): void {
        echo "Running query on {$this->dbName}: $sql\n";
    }

    // Destructor - runs when object is destroyed
    public function __destruct() {
        echo "Closing connection to {$this->dbName}\n";
    }
}

echo "Start of script\n";

$db = new Connection("MyDatabase");
$db->query("SELECT * FROM users");

unset($db); // manually destroys the object here

echo "End of script\n";
```