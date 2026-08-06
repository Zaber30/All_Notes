The 3-Step Workflow

To see how they cooperate, look at this step-by-step breakdown:

1. **Define the Delegate (The Blueprint)**: You declare what the method signature must look like.
2. **Write the Lambda (The Content)**: You write a nameless block of code matching that exact signature.
3. **Invoke the Delegate (The Execution)**: You run the delegate, which executes the lambda code

```cpp
4. Blueprint: Expects two ints, returns an int
public delegate int Calculate(int a, int b); 

static void Main()
{
    // 2. Content: A lambda expression filling the blueprint
    Calculate multiplier = (x, y) => x * y; 

    // 3. Execution: Output is 50
    Console.WriteLine(multiplier(5, 10)); 
}
```

Delegates and lambda expressions are closely related in C#, but they are **not the same thing**.

- A **delegate** is a **type** that can store a reference to a method.
- A **lambda expression** is a **short way to write an anonymous function**. A lambda is often assigned to a delegate.

Think of it like this:

- **Delegate = Container**
- **Lambda = Function you put inside the container**
# C# Built-in Delegates

C# provides three built-in generic delegates that eliminate the need to declare custom delegates in most cases.

- **Func** → Returns a value.
- **Action** → Returns nothing (`void`).
- **Predicate** → Returns a `bool`.

---

# 1. Func

`Func` represents a method that **returns a value**.

## Syntax

```csharp
Func<T1, T2, ..., TResult>
```

- `T1`, `T2`, ... = Input parameter types
- `TResult` = Return type (always the last type)

## Examples

### No Parameters

```csharp
Func<int> getNumber = () => 100;

Console.WriteLine(getNumber());
```

Output

```
100
```

---

### One Parameter

```csharp
Func<int, int> square = x => x * x;

Console.WriteLine(square(5));
```

Output

```
25
```

---

### Two Parameters

```csharp
Func<int, int, int> add = (a, b) => a + b;

Console.WriteLine(add(10, 20));
```

Output

```
30
```

---

### Three Parameters

```csharp
Func<int, int, int, int> sum = (a, b, c) => a + b + c;

Console.WriteLine(sum(2, 3, 4));
```

Output

```
9
```

---

# 2. Action

`Action` represents a method that **returns nothing (`void`)**.

## Syntax

```csharp
Action<T1, T2, ...>
```

- Takes zero or more parameters.
- Returns `void`.

---

### No Parameters

```csharp
Action hello = () => Console.WriteLine("Hello");

hello();
```

Output

```
Hello
```

---

### One Parameter

```csharp
Action<string> print = name => Console.WriteLine(name);

print("Alice");
```

Output

```
Alice
```

---

### Two Parameters

```csharp
Action<int, int> showSum = (a, b) =>
{
    Console.WriteLine(a + b);
};

showSum(5, 3);
```

Output

```
8
```

---

# 3. Predicate

`Predicate<T>` represents a method that **takes one parameter and returns `bool`**.

## Syntax

```csharp
Predicate<T>
```

Equivalent to

```csharp
Func<T, bool>
```

---

### Example

```csharp
Predicate<int> isEven = x => x % 2 == 0;

Console.WriteLine(isEven(10));
Console.WriteLine(isEven(7));
```

Output

```
True
False
```

---

# Comparison

| Delegate | Parameters | Return Type | Example |
|----------|------------|-------------|---------|
| `Func<T1,...,TResult>` | 0–16 | Any type | `Func<int,int,int>` |
| `Action<T1,...>` | 0–16 | `void` | `Action<string>` |
| `Predicate<T>` | 1 | `bool` | `Predicate<int>` |

---

# Func vs Action vs Predicate

| Feature | Func | Action | Predicate |
|---------|------|--------|-----------|
| Returns value | ✅ | ❌ | ✅ (`bool`) |
| Returns `void` | ❌ | ✅ | ❌ |
| Returns `bool` | Optional | ❌ | Always |
| Parameters | 0–16 | 0–16 | Exactly 1 |
| Common Uses | Calculations, LINQ `Select` | Printing, Logging, Event handlers | Filtering, Validation |

---

# Common LINQ Examples

## Func

```csharp
List<int> numbers = new() { 1, 2, 3, 4, 5 };

var squares = numbers.Select(x => x * x);
```

---

## Predicate

```csharp
Predicate<int> isPositive = x => x > 0;

Console.WriteLine(isPositive(5));
```

---

## Action

```csharp
numbers.ForEach(x => Console.WriteLine(x));
```

---

# Custom Delegate vs Built-in Delegate

### Custom Delegate

```csharp
delegate int Operation(int a, int b);

Operation add = (a, b) => a + b;
```

---

### Built-in Func

```csharp
Func<int, int, int> add = (a, b) => a + b;
```

---

### Built-in Action

```csharp
Action<string> print = name => Console.WriteLine(name);
```

---

### Built-in Predicate

```csharp
Predicate<int> isEven = x => x % 2 == 0;
```

---

# When to Use Which?

| Situation | Delegate |
|-----------|----------|
| Return a value | `Func` |
| Return nothing (`void`) | `Action` |
| Return `true`/`false` | `Predicate` |

---

# Memory Trick

```
Func      → Functions that RETURN something.
Action    → Performs an ACTION (returns void).
Predicate → Tests a condition (returns bool).
```

---

# Summary

| Built-in Delegate | Signature | Returns |
|-------------------|-----------|---------|
| `Func<T1,...,TResult>` | `(T1,...)->TResult` | Any type |
| `Action<T1,...>` | `(T1,...)->void` | `void` |
| `Predicate<T>` | `(T)->bool` | `bool` |

These three delegates cover the vast majority of delegate use cases in modern C#. Custom delegates are now mainly used when a meaningful, domain-specific delegate type improves code readability.