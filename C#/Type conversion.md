# C# and .NET Type Conversion - Complete Notes

> **Goal:** Understand every type of conversion in C#, when to use it, what it can do, and what it cannot do.

---

# Table of Contents

1. What is Type Conversion?
    
2. Categories of Type Conversion
    
3. Implicit Conversion
    
4. Explicit Conversion (Casting)
    
5. Convert Class
    
6. Parse()
    
7. TryParse()
    
8. Upcasting & Downcasting
    
9. User-Defined Conversion
    
10. Boxing & Unboxing
    
11. Conversion Comparison Table
    
12. Best Practices
    

---

# 1. What is Type Conversion?

Type conversion is the process of changing a value from one data type into another.

Example

```csharp
int number = 10;

double value = number;
```

The value `10` is converted from `int` to `double`.

---

# Categories of Type Conversion

```
Type Conversion
│
├── Automatic (Implicit Conversion)
│
├── Manual (Explicit Conversion)
│
├── Convert Class
│
├── Parse()
│
├── TryParse()
│
├── Reference Type Conversion
│     ├── Upcasting
│     └── Downcasting
│
├── User Defined Conversion
│
└── Boxing / Unboxing
```

---

# 2. Implicit Conversion

## Definition

The compiler automatically converts one type into another **when the conversion is guaranteed to be safe**.

No cast is required.

No data loss occurs.

---

## Syntax

```csharp
DestinationType variable = sourceValue;
```

---

## Example

```csharp
int x = 100;

double y = x;
```

Compiler automatically converts

```
100 → 100.0
```

---

## Common Implicit Conversions

|From|To|
|---|---|
|byte|short|
|byte|int|
|byte|long|
|byte|float|
|byte|double|
|short|int|
|short|long|
|int|long|
|int|float|
|int|double|
|long|float|
|long|double|
|float|double|
|char|int|
|char|long|
|Derived Class|Base Class|

---

## What Implicit Conversion CAN Do

✔ Convert smaller numeric types into larger numeric types.

✔ Convert derived objects into base objects.

✔ Preserve all information.

---

## What It CANNOT Do

❌ Convert larger numeric types into smaller ones.

```csharp
double d = 10.5;

int x = d;
```

Compile Error

---

❌ Convert unrelated types.

```csharp
string text = 10;
```

Compile Error

---

# 3. Explicit Conversion (Casting)

## Definition

The programmer manually converts one type into another.

Used when information may be lost.

---

## Syntax

```csharp
(TargetType)value
```

---

## Example

```csharp
double d = 10.8;

int x = (int)d;
```

Result

```
10
```

Decimal portion is discarded.

---

## Another Example

```csharp
long number = 5000;

int x = (int)number;
```

---

## What Explicit Conversion CAN Do

✔ Convert larger numeric types to smaller ones.

✔ Convert base classes into derived classes.

✔ Convert compatible types manually.

---

## What It CANNOT Do

❌ Convert incompatible types.

```csharp
string name = "John";

int x = (int)name;
```

Compile Error

---

❌ Prevent data loss.

```csharp
double d = 9.99;

int x = (int)d;
```

Result

```
9
```

---

# 4. Convert Class

Namespace

```csharp
using System;
```

---

## Description

The Convert class contains static methods that convert one .NET type into another.

Examples

```
Convert.ToInt32()

Convert.ToDouble()

Convert.ToBoolean()

Convert.ToDecimal()

Convert.ToChar()

Convert.ToString()

Convert.ToByte()

Convert.ToInt16()

Convert.ToInt64()
```

---

## Example

```csharp
string age = "25";

int number = Convert.ToInt32(age);
```

---

## Example

```csharp
double d = 15.7;

int x = Convert.ToInt32(d);
```

Result

```
16
```

Notice

Convert rounds

Casting truncates

---

## Convert Handles null

```csharp
string text = null;

int x = Convert.ToInt32(text);
```

Result

```
0
```

---

## What Convert CAN Do

✔ Convert between many built-in .NET types.

✔ Convert string into numeric types.

✔ Convert numeric types into strings.

✔ Convert bool.

✔ Convert char.

✔ Handle null in some cases.

---

## What Convert CANNOT Do

❌ Convert custom objects automatically.

```csharp
Student s = new Student();

Convert.ToInt32(s);
```

Runtime Exception

---

❌ Convert invalid strings.

```csharp
Convert.ToInt32("ABC");
```

Throws

```
FormatException
```

---

# 5. Parse()

## Description

Parse converts a **string** into the type that owns the Parse method.

Examples

```csharp
int.Parse()

double.Parse()

decimal.Parse()

bool.Parse()

DateTime.Parse()

long.Parse()

float.Parse()
```

---

## Syntax

```csharp
TargetType.Parse(stringValue)
```

---

## Example

```csharp
int age = int.Parse("25");
```

---

## Example

```csharp
double value = double.Parse("10.75");
```

---

## Example

```csharp
bool flag = bool.Parse("true");
```

---

## Example

```csharp
DateTime date = DateTime.Parse("2026-07-09");
```

---

## What Parse CAN Do

✔ Convert strings into supported types.

✔ Throw an exception if conversion fails.

---

## What Parse CANNOT Do

❌ Convert non-string values.

```csharp
double.Parse(10);
```

Compile Error

---

❌ Convert invalid strings.

```csharp
int.Parse("Hello");
```

Throws

```
FormatException
```

---

❌ Handle null safely.

Throws

```
ArgumentNullException
```

---

# 6. TryParse()

## Description

Safely converts a string.

Instead of throwing exceptions, returns true or false.

---

## Syntax

```csharp
TargetType.TryParse(value, out result)
```

---

## Example

```csharp
bool success = int.TryParse("100", out int number);
```

Result

```
success = true

number = 100
```

---

## Invalid Example

```csharp
bool success = int.TryParse("ABC", out int number);
```

Result

```
success = false

number = 0
```

No exception.

---

## What TryParse CAN Do

✔ Convert strings safely.

✔ Avoid exceptions.

✔ Validate user input.

---

## What TryParse CANNOT Do

❌ Convert non-string values.

❌ Convert unsupported object types.

---

# 7. Reference Type Conversion

## Upcasting

Derived → Base

Automatic

```csharp
class Animal {}

class Dog : Animal {}

Dog dog = new Dog();

Animal animal = dog;
```

---

### Can Do

✔ Safe conversion.

✔ No cast required.

---

### Cannot Do

❌ Access Dog-only members through Animal reference.

---

## Downcasting

Base → Derived

Manual

```csharp
Animal animal = new Dog();

Dog dog = (Dog)animal;
```

---

### Can Do

✔ Recover derived object.

---

### Cannot Do

❌ Convert unrelated objects.

```csharp
Animal animal = new Animal();

Dog dog = (Dog)animal;
```

Throws

```
InvalidCastException
```

---

# 8. User Defined Conversion

You can define conversions yourself.

Implicit

```csharp
public static implicit operator Meter(double value)
```

Explicit

```csharp
public static explicit operator double(Meter meter)
```

---

## Can Do

✔ Convert your own classes.

✔ Improve readability.

---

## Cannot Do

❌ Automatically convert every class.

Must be implemented.

---

# 9. Boxing

## Definition

Converting a value type into object.

Example

```csharp
int number = 10;

object obj = number;
```

Compiler boxes the integer.

---

## Can Do

✔ Store value types as objects.

---

## Cannot Do

❌ Avoid performance overhead.

---

# 10. Unboxing

Converting object back into value type.

```csharp
object obj = 10;

int number = (int)obj;
```

---

## Can Do

✔ Recover original value.

---

## Cannot Do

❌ Recover incorrect type.

```csharp
object obj = "Hello";

int x = (int)obj;
```

Throws

```
InvalidCastException
```

---

# Complete Comparison Table

|Conversion|Automatic|Requires Cast|Input|Output|Throws Exception|Data Loss|
|---|---|---|---|---|---|---|
|Implicit|✅|❌|Numeric / Reference|Numeric / Reference|❌|❌|
|Explicit|❌|✅|Numeric / Reference|Numeric / Reference|Possible|Possible|
|Convert|❌|❌|Most built-in .NET types|Most built-in .NET types|Yes (invalid values)|Possible|
|Parse|❌|❌|**String only**|Specific type|Yes|❌|
|TryParse|❌|❌|**String only**|Specific type|❌|❌|
|Upcasting|✅|❌|Derived|Base|❌|❌|
|Downcasting|❌|✅|Base|Derived|Possible|❌|
|Boxing|✅|❌|Value Type|Object|❌|❌|
|Unboxing|❌|✅|Object|Value Type|Possible|❌|

---

# Which Conversion Should You Use?

|Situation|Recommended|
|---|---|
|Safe numeric conversion|Implicit|
|Larger → smaller numeric type|Explicit Cast|
|Convert between built-in .NET types|Convert|
|Convert a string to another type when input is guaranteed valid|Parse|
|Convert user input safely|TryParse|
|Store derived object as base type|Upcasting|
|Recover derived object|Downcasting|
|Store a value type as an object|Boxing|
|Recover value from an object|Unboxing|

---

# Best Practices

- ✅ Prefer **implicit conversion** whenever possible.
    
- ✅ Use **explicit casting** only when necessary and be aware of possible data loss.
    
- ✅ Use **Convert** for general conversions between built-in .NET types.
    
- ✅ Use **Parse()** only when you're sure the input string is valid.
    
- ✅ Use **TryParse()** for user input or any untrusted data.
    
- ✅ Check types before downcasting (using `is` or `as`) to avoid `InvalidCastException`.
    
- ✅ Minimize unnecessary boxing and unboxing in performance-critical code.