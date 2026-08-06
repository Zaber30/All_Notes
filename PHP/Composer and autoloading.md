# Composer & Autoloading

## What is Composer

**Definition:** Composer is PHP's **dependency manager** — it downloads and manages third-party libraries/packages your project needs, and generates an **autoloader** so you don't have to manually `require` every file.

```bash
# Install Composer (one-time, system-wide)
# Then in your project folder:
composer require monolog/monolog
```

This downloads the `monolog/monolog` package into a `vendor/` folder and registers it for autoloading.

---

## `composer.json` Structure

**Definition:** The main configuration file describing your project, its dependencies, and autoloading rules.

```json
{
    "name": "myapp/project",
    "description": "My PHP application",
    "type": "project",
    "require": {
        "php": ">=8.1",
        "monolog/monolog": "^3.0"
    },
    "require-dev": {
        "phpunit/phpunit": "^10.0"
    },
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    },
    "scripts": {
        "test": "phpunit"
    }
}
```

|Key|Purpose|
|---|---|
|`name`|Package name (`vendor/project`)|
|`require`|Production dependencies|
|`require-dev`|Development-only dependencies|
|`autoload`|Rules for mapping namespaces to folders|
|`scripts`|Custom shortcut commands|

---

## `require` vs `require-dev`

**Definition:** `require` lists packages needed to **run** the app in production. `require-dev` lists packages needed only for **development** (testing, debugging) — excluded when installing with `--no-dev`.

```json
{
    "require": {
        "guzzlehttp/guzzle": "^7.0"
    },
    "require-dev": {
        "phpunit/phpunit": "^10.0",
        "fakerphp/faker": "^1.0"
    }
}
```

```bash
composer install --no-dev  # skips require-dev packages (for production)
```

---

## Semantic Versioning

**Definition:** A version numbering standard: `MAJOR.MINOR.PATCH` (e.g., `2.4.1`). Composer uses **constraint symbols** to control which versions are allowed.

```json
{
    "require": {
        "monolog/monolog": "^3.2",   // >=3.2.0 and <4.0.0 (safe minor/patch updates)
        "guzzlehttp/guzzle": "~7.5", // >=7.5.0 and <7.6.0 (patch updates only)
        "some/package": "2.1.0",     // exact version only
        "another/package": ">=1.0"   // any version 1.0 or above
    }
}
```

|Symbol|Meaning|
|---|---|
|`^3.2`|Compatible up to next major version (`3.2.0` to `<4.0.0`)|
|`~7.5`|Compatible up to next minor version (`7.5.0` to `<7.6.0`)|
|`2.1.0`|Exact version|
|`*`|Any version|

**MAJOR.MINOR.PATCH meaning:** MAJOR = breaking changes, MINOR = new features (backward-compatible), PATCH = bug fixes.

---

## Autoloading — PSR-4, PSR-0, Classmap, Files

**Definition:** Autoloading automatically **loads class files** when needed, without manual `require`/`include` statements.

### PSR-4 (modern standard)

**Definition:** Maps a **namespace prefix** to a **directory**; class names must match folder structure exactly.

```json
{
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
}
```

```php
<?php
// File: src/Models/User.php
namespace App\Models;

class User {
    public function greet(): void {
        echo "Hello from User\n";
    }
}
```

```php
<?php
// Usage elsewhere
require 'vendor/autoload.php';

use App\Models\User;

$user = new User();
$user->greet();
```

**Output:** `Hello from User`

### PSR-0 (legacy, deprecated)

**Definition:** Older autoloading standard — similar to PSR-4, but also converts underscores in class names to directory separators. Rarely used in new projects.

```json
{
    "autoload": {
        "psr-0": {
            "App\\": "src/"
        }
    }
}
```

### Classmap

**Definition:** Scans specified directories/files and builds a **direct map** of class names to file paths — useful for libraries that don't follow PSR-4 naming.

```json
{
    "autoload": {
        "classmap": ["src/Legacy/"]
    }
}
```

### Files

**Definition:** Always **includes specific files** on every request — useful for loading helper/global functions (not classes).

```json
{
    "autoload": {
        "files": ["src/helpers.php"]
    }
}
```

---

## `composer install` vs `composer update`

|Command|What it does|
|---|---|
|`composer install`|Installs packages based on the **exact versions** in `composer.lock`. Used when setting up a project (e.g., after cloning).|
|`composer update`|Checks `composer.json` constraints, finds the **latest allowed versions**, installs them, and **rewrites** `composer.lock`.|

```bash
composer install   # use this on a fresh clone / deployment (consistent versions)
composer update     # use this when you WANT to fetch newer package versions
```

---

## `composer dump-autoload`

**Definition:** Regenerates the autoloader files **without** touching dependencies — used when you add/rename classes and the autoloader needs to refresh its map (especially for `classmap`).

```bash
composer dump-autoload
```

```bash
composer dump-autoload -o   # "optimized" - faster, generates a full classmap (good for production)
```

---

## Composer Scripts

**Definition:** Custom shortcut commands defined in `composer.json`, run via `composer run-script` (or just `composer <name>` for common ones).

```json
{
    "scripts": {
        "test": "phpunit",
        "start": "php -S localhost:8000 -t public",
        "lint": "phpcs src/"
    }
}
```

```bash
composer test    # runs phpunit
composer start   # starts a dev server
composer lint    # runs code style checker
```

---

## Lock File Purpose

**Definition:** `composer.lock` records the **exact versions** of every package (and sub-dependency) that was installed — ensuring **everyone on the team, and production, gets the identical versions**, regardless of what `composer.json`'s flexible constraints (like `^3.2`) would otherwise resolve to at a later date.

```
composer.json  → "monolog/monolog": "^3.2"   (a RANGE of allowed versions)
composer.lock  → "monolog/monolog": "3.2.4"  (the EXACT version actually installed)
```

**Why it matters:**

- Without it, two developers running `composer install` on different days could get **different** package versions (since `^3.2` could resolve to `3.2.0` today and `3.5.0` next month).
- `composer.lock` **should be committed to Git** — it guarantees consistent, reproducible installs everywhere (dev, staging, production).
- Only `composer update` changes the lock file; `composer install` respects it exactly.

---

## Quick Summary Table

|Concept|One-Line Meaning|
|---|---|
|Composer|PHP dependency manager + autoloader generator|
|`composer.json`|Project config: dependencies, autoload rules, scripts|
|`require` vs `require-dev`|Production deps vs development-only deps|
|Semantic versioning|`MAJOR.MINOR.PATCH` + constraint symbols (`^`, `~`)|
|PSR-4|Modern autoloading: namespace → folder mapping|
|PSR-0|Legacy autoloading standard (deprecated)|
|Classmap|Direct class-to-file mapping (for non-PSR code)|
|Files|Always-included files (for global functions)|
|`composer install`|Install exact versions from `composer.lock`|
|`composer update`|Fetch latest allowed versions, rewrite lock file|
|`composer dump-autoload`|Regenerate autoloader without changing dependencies|
|Composer scripts|Custom shortcut commands in `composer.json`|
|`composer.lock`|Locks exact versions for consistent installs everywhere|

Want **PSR standards** next (PSR-1, PSR-12 coding style, PSR-3 logging, etc.), since PSR-4 autoloading is just one piece of a bigger set?