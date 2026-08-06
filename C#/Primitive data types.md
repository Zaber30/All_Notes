# C# Primitive Data Types

In C#, **primitive data types** (also called **built-in** or **predefined types**) are the foundational data types provided directly by the compiler. Every primitive type is represented by a reserved keyword which acts as an alias for an underlying structure in the .NET **Common Type System (CTS)**.

---

## 1. Integer Numeric Types

These types store whole positive and negative numbers. They are split into **signed** (supports negative values) and **unsigned** (strictly positive/zero values) categories.

| Keyword | CTS Type | Memory Size | Value Range |
| :--- | :--- | :--- | :--- |
| **`sbyte`** | `System.SByte` | 8 bits (1 byte) | `-128` to `127` |
| **`byte`** | `System.Byte` | 8 bits (1 byte) | `0` to `255` |
| **`short`** | `System.Int16` | 16 bits (2 bytes) | `-32,768` to `32,767` |
| **`ushort`** | `System.UInt16`| 16 bits (2 bytes) | `0` to `65,535` |
| **`int`** | `System.Int32` | 32 bits (4 bytes) | `-2,147,483,648` to `2,147,483,647` |
| **`uint`** | `System.UInt32`| 32 bits (4 bytes) | `0` to `4,294,967,295` |
| **`long`** | `System.Int64` | 64 bits (8 bytes) | `-9,223,372,036,854,775,808` to `9,223,372,036,854,775,807` |
| **`ulong`** | `System.UInt64`| 64 bits (8 bytes) | `0` to `18,446,744,073,709,551,615` |
| **`nint`** | `System.IntPtr` | 32 or 64 bits | Native-sized signed integer (depends on system architecture) |
| **`nuint`** | `System.UIntPtr`| 32 or 64 bits | Native-sized unsigned integer (depends on system architecture) |

---

## 2. Floating-Point & Real Numeric Types

These types manage fractional numbers with decimal points. They offer varying balances between performance and mathematical precision.

| Keyword | CTS Type | Memory Size | Precision | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **`float`** | `System.Single` | 32 bits (4 bytes) | ~6-9 digits | 3D graphics, game physics engines, and fast streaming data. |
| **`double`** | `System.Double` | 64 bits (8 bytes) | ~15-17 digits | General science math, geographic coordinates, and default decimals. |
| **`decimal`**| `System.Decimal`| 128 bits (16 bytes)| 28-29 digits | Financial accounting, monetary tracking, and zero rounding-error math. |

---

## 3. Textual & Logical Types

These primitives deal with non-numeric program mechanics, text processing, and conditional application flow.

| Keyword | CTS Type | Memory Size | Allowed Values / Behavior |
| :--- | :--- | :--- | :--- |
| **`bool`** | `System.Boolean` | 8 bits (1 byte) | Strictly `true` or `false` logic states. |
| **`char`** | `System.Char` | 16 bits (2 bytes) | A single UTF-16 code unit wrapped in single quotes (e.g., `'A'`, `'\n'`). |
| **`string`**| `System.String` | Variable | A reference type representing an immutable sequence of Unicode characters. |

---

## 4. The Ecosystem Root Type

*   **`object`** (`System.Object`): The ultimate base type of the entire .NET runtime ecosystem. Every primitive value type, custom class, and array inherits directly or indirectly from this type.

---

## Technical Syntax Rules

Because C# assumes default types for numeric literals (integers default to `int`, fractions default to `double`), you must use explicit **literal suffixes** to assign values to specific primitive types:

```csharp
long highValue = 9500000000L;  // 'L' or 'l' forces a 64-bit Long assignment
float speed    = 12.34f;       // 'F' or 'f' is mandatory for single-precision floats
decimal price  = 19.99m;       // 'M' or 'm' is mandatory to create precise financial decimals
uint positive  = 4000000000U;  // 'U' or 'u' designates an unsigned integer literal
```

Do you need help setting up **explicit type casting** between these types, or would you like to see how to format numeric strings using custom **culture rules**?
