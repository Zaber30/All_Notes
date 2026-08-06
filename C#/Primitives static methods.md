# C# Primitive Data Types & Their Common Static Members

> Primitive data types are aliases for .NET structs/classes.
>
> Example:
>
> - `int` → `System.Int32`
> - `double` → `System.Double`

---

# 1. bool (`System.Boolean`)

## Static Methods

```csharp
Boolean.Parse()
Boolean.TryParse()
```

## Static Fields

```csharp
Boolean.TrueString
Boolean.FalseString
```

Example

```csharp
bool value = Boolean.Parse("true");
```

---

# 2. byte (`System.Byte`)

## Static Methods

```csharp
Byte.Parse()
Byte.TryParse()
```

## Static Fields

```csharp
Byte.MinValue
Byte.MaxValue
```

---

# 3. sbyte (`System.SByte`)

## Static Methods

```csharp
SByte.Parse()
SByte.TryParse()
```

## Static Fields

```csharp
SByte.MinValue
SByte.MaxValue
```

---

# 4. short (`System.Int16`)

## Static Methods

```csharp
Int16.Parse()
Int16.TryParse()
```

## Static Fields

```csharp
Int16.MinValue
Int16.MaxValue
```

---

# 5. ushort (`System.UInt16`)

## Static Methods

```csharp
UInt16.Parse()
UInt16.TryParse()
```

## Static Fields

```csharp
UInt16.MinValue
UInt16.MaxValue
```

---

# 6. int (`System.Int32`)

## Static Methods

```csharp
Int32.Parse()
Int32.TryParse()
```

## Static Fields

```csharp
Int32.MinValue
Int32.MaxValue
```

Example

```csharp
int n = Int32.Parse("100");
```

---

# 7. uint (`System.UInt32`)

## Static Methods

```csharp
UInt32.Parse()
UInt32.TryParse()
```

## Static Fields

```csharp
UInt32.MinValue
UInt32.MaxValue
```

---

# 8. long (`System.Int64`)

## Static Methods

```csharp
Int64.Parse()
Int64.TryParse()
```

## Static Fields

```csharp
Int64.MinValue
Int64.MaxValue
```

---

# 9. ulong (`System.UInt64`)

## Static Methods

```csharp
UInt64.Parse()
UInt64.TryParse()
```

## Static Fields

```csharp
UInt64.MinValue
UInt64.MaxValue
```

---

# 10. float (`System.Single`)

## Static Methods

```csharp
Single.Parse()
Single.TryParse()
Single.IsNaN()
Single.IsInfinity()
Single.IsFinite()
Single.IsPositiveInfinity()
Single.IsNegativeInfinity()
```

## Static Fields

```csharp
Single.MinValue
Single.MaxValue
Single.NaN
Single.PositiveInfinity
Single.NegativeInfinity
Single.Epsilon
```

---

# 11. double (`System.Double`)

## Static Methods

```csharp
Double.Parse()
Double.TryParse()
Double.IsNaN()
Double.IsInfinity()
Double.IsFinite()
Double.IsPositiveInfinity()
Double.IsNegativeInfinity()
```

## Static Fields

```csharp
Double.MinValue
Double.MaxValue
Double.NaN
Double.PositiveInfinity
Double.NegativeInfinity
Double.Epsilon
```

---

# 12. decimal (`System.Decimal`)

## Static Methods

```csharp
Decimal.Parse()
Decimal.TryParse()
```

## Static Fields

```csharp
Decimal.MinValue
Decimal.MaxValue
Decimal.Zero
Decimal.One
Decimal.MinusOne
```

---

# 13. char (`System.Char`)

## Static Methods

```csharp
Char.Parse()
Char.TryParse()

Char.IsDigit()
Char.IsLetter()
Char.IsLetterOrDigit()
Char.IsWhiteSpace()
Char.IsUpper()
Char.IsLower()
Char.IsPunctuation()
Char.IsSymbol()
Char.IsControl()

Char.ToUpper()
Char.ToLower()
```

## Static Fields

```csharp
Char.MinValue
Char.MaxValue
```

Example

```csharp
if (Char.IsDigit(c))
{
    Console.WriteLine("Digit");
}
```

---

# Primitive Types Summary

| C# Type | .NET Type | Common Static Methods |
|----------|-----------|-----------------------|
| `bool` | `Boolean` | `Parse()`, `TryParse()` |
| `byte` | `Byte` | `Parse()`, `TryParse()` |
| `sbyte` | `SByte` | `Parse()`, `TryParse()` |
| `short` | `Int16` | `Parse()`, `TryParse()` |
| `ushort` | `UInt16` | `Parse()`, `TryParse()` |
| `int` | `Int32` | `Parse()`, `TryParse()` |
| `uint` | `UInt32` | `Parse()`, `TryParse()` |
| `long` | `Int64` | `Parse()`, `TryParse()` |
| `ulong` | `UInt64` | `Parse()`, `TryParse()` |
| `float` | `Single` | `Parse()`, `TryParse()`, `IsNaN()`, `IsInfinity()` |
| `double` | `Double` | `Parse()`, `TryParse()`, `IsNaN()`, `IsInfinity()` |
| `decimal` | `Decimal` | `Parse()`, `TryParse()` |
| `char` | `Char` | `Parse()`, `TryParse()`, `IsDigit()`, `IsLetter()`, `ToUpper()`, `ToLower()` |

# Most Important to Remember

### Numeric Types (`int`, `long`, `double`, `decimal`, etc.)

```text
Parse()
TryParse()
MinValue
MaxValue
```

### `char`

```text
IsDigit()
IsLetter()
IsLetterOrDigit()
IsWhiteSpace()
IsUpper()
IsLower()
ToUpper()
ToLower()
```
# string (`System.String`)

> `string` is the C# alias for `System.String`.
>
> It is a **reference type** and **immutable**.

---

# Common Static Methods

## Validation

```csharp
String.IsNullOrEmpty()
String.IsNullOrWhiteSpace()
```

Example

```csharp
String.IsNullOrEmpty(name);
```

---

## Concatenation

```csharp
String.Concat()
String.Join()
```

Example

```csharp
String.Concat("Hello", "World");

String.Join(", ", names);
```

---

## Formatting

```csharp
String.Format()
```

Example

```csharp
String.Format("{0} is {1}", "John", 20);
```

---

## Comparison

```csharp
String.Compare()
String.CompareOrdinal()
String.Equals()
```

Example

```csharp
String.Compare(a, b);
```

---

## Parsing / Creation

```csharp
String.Copy()      // Obsolete
String.Intern()
String.IsInterned()
```

---

# Static Property

```csharp
String.Empty
```

Example

```csharp
string s = String.Empty;
```

Equivalent to

```csharp
string s = "";
```

---

# Most Important Instance Methods

These are called on a string object.

```csharp
ToUpper()
ToLower()
Trim()
TrimStart()
TrimEnd()

Replace()

Substring()

Contains()

StartsWith()
EndsWith()

Split()

IndexOf()
LastIndexOf()

Insert()

Remove()

PadLeft()
PadRight()

ToCharArray()
```

Example

```csharp
string name = "  Hello World  ";

name = name.Trim();
name = name.ToUpper();
```

---

# Most Important Property

```csharp
Length
```

Example

```csharp
Console.WriteLine(name.Length);
```

---

# Most Important for Interviews

## Static

```text
IsNullOrEmpty()
IsNullOrWhiteSpace()

Join()
Concat()
Format()

Compare()
Equals()

Empty
```

## Instance

```text
Length

ToUpper()
ToLower()

Trim()

Replace()

Split()

Substring()

Contains()

StartsWith()
EndsWith()

IndexOf()

ToCharArray()
```
### `bool`

```text
Parse()
TryParse()
TrueString
FalseString
```