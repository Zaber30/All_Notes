

## tags: [csharp, collections, generics, dotnet] created: 2026-07-09

# C# Generic vs Non-Generic Collections

## 1. Overview

C# provides two main namespaces for working with collections:

|Namespace|Type|Introduced|
|---|---|---|
|`System.Collections`|Non-Generic|.NET 1.0|
|`System.Collections.Generic`|Generic|.NET 2.0|

The **non-generic** namespace stores everything as `object`, meaning any data type can go in, but you lose type safety. The **generic** namespace lets you specify the exact type (`T`) at compile time, giving you type safety and better performance.

---

## 2. Non-Generic Namespace — `System.Collections`

### Classes

|Class|Description|
|---|---|
|`ArrayList`|Dynamic array that can hold items of any type (stored as `object`)|
|`Hashtable`|Key-value pair collection, keys/values stored as `object`|
|`Queue`|FIFO (First-In-First-Out) collection|
|`Stack`|LIFO (Last-In-First-Out) collection|
|`SortedList`|Key-value pairs sorted by key|
|`BitArray`|Compact array of bit values (true/false)|

### Interfaces

|Interface|Description|
|---|---|
|`IEnumerable`|Exposes an enumerator to loop through a non-generic collection|
|`ICollection`|Defines size, enumeration, and sync methods for non-generic collections|
|`IList`|Represents a non-generic collection accessible by index|
|`IDictionary`|Represents a non-generic collection of key-value pairs|
|`IComparer`|Compares two objects for sorting|
|`IEnumerator`|Supports simple iteration over a non-generic collection|

### Example

```csharp
ArrayList list = new ArrayList();
list.Add(10);
list.Add("Hello");   // No type restriction — mixed types allowed
list.Add(3.14);

foreach (var item in list)
{
    Console.WriteLine(item); // requires manual casting/boxing internally
}
```

---

## 3. Generic Namespace — `System.Collections.Generic`

### Classes

|Class|Description|
|---|---|
|`List<T>`|Dynamic, strongly-typed array (replacement for `ArrayList`)|
|`Dictionary<TKey,TValue>`|Strongly-typed key-value pairs (replacement for `Hashtable`)|
|`Queue<T>`|Strongly-typed FIFO collection|
|`Stack<T>`|Strongly-typed LIFO collection|
|`SortedList<TKey,TValue>`|Sorted key-value pairs, strongly typed|
|`SortedDictionary<TKey,TValue>`|Sorted dictionary with faster inserts for large data|
|`HashSet<T>`|Unordered collection of unique elements|
|`LinkedList<T>`|Doubly linked list|

### Interfaces

|Interface|Description|
|---|---|
|`IEnumerable<T>`|Exposes a strongly-typed enumerator|
|`ICollection<T>`|Defines size, enumeration, and modification methods for a generic collection|
|`IList<T>`|Represents a strongly-typed collection accessible by index|
|`IDictionary<TKey,TValue>`|Represents a strongly-typed collection of key-value pairs|
|`IComparer<T>`|Compares two objects of the same type|
|`IComparable<T>`|Defines a type-specific comparison method|
|`IReadOnlyList<T>` / `IReadOnlyCollection<T>`|Read-only versions of generic collections|

### Example

```csharp
List<int> numbers = new List<int>();
numbers.Add(10);
numbers.Add(20);
// numbers.Add("Hello"); ❌ Compile-time error — type safety enforced

Dictionary<string, int> ages = new Dictionary<string, int>();
ages["Rahim"] = 25;
ages["Karim"] = 30;
```

---

## 4. Real-Life Benefits

### Non-Generic Collections

- ✅ Flexible — can store mixed data types in one collection.
- ✅ Useful in legacy codebases (old .NET Framework apps, COM interop).
- ❌ No compile-time type checking → runtime errors possible.
- ❌ Performance cost due to **boxing/unboxing** (value types converted to `object` and back).

**Real-life analogy:** A general storage box where you can throw in anything — clothes, tools, books — but you have to check each item yourself before using it (manual casting).

### Generic Collections

- ✅ **Type Safety** — errors caught at compile time, not runtime.
- ✅ **Performance** — no boxing/unboxing for value types (`int`, `struct`, etc.), so faster execution and less memory pressure.
- ✅ **Code Reusability** — write one generic class/method (`Repository<T>`) that works for `Repository<Customer>`, `Repository<Order>`, etc.
- ✅ **Cleaner code** — no manual casting needed.
- ✅ **IntelliSense support** — IDE knows exact type, better autocomplete.

**Real-life analogy:** A labeled storage box (e.g., "Only Books") — you know exactly what's inside without checking, and you can't accidentally put in the wrong item.

**Practical use cases:**

- `List<Product>` for an e-commerce product catalog.
- `Dictionary<string, Customer>` for fast customer lookup by ID.
- `Queue<Order>` for processing orders in the order they arrive (order processing systems).
- `Stack<UndoAction>` for implementing Undo/Redo in an application.
- Generic repository pattern: `IRepository<T>` used across Employee, Product, Invoice modules in enterprise apps.

---

## 5. Comparison Table

|Feature|Non-Generic (`System.Collections`)|Generic (`System.Collections.Generic`)|
|---|---|---|
|Type Safety|❌ No (stores as `object`)|✅ Yes (strongly typed)|
|Performance|❌ Slower (boxing/unboxing)|✅ Faster (no boxing/unboxing for value types)|
|Compile-time checking|❌ No|✅ Yes|
|Code Reusability|Limited|High (works with any type `T`)|
|Casting Required|✅ Yes, manual casting|❌ No|
|Mixed Data Types|✅ Allowed|❌ Not allowed (single type only)|
|Introduced In|.NET 1.0|.NET 2.0+|
|Recommended for New Code|❌ No (legacy only)|✅ Yes|
|Examples|`ArrayList`, `Hashtable`, `Queue`, `Stack`|`List<T>`, `Dictionary<TKey,TValue>`, `Queue<T>`, `Stack<T>`|

---

## 6. Which One Is Best?

**Generic collections (`System.Collections.Generic`) are the clear winner for modern C# development.**

Reasons:

1. Type safety prevents runtime bugs.
2. Better performance — no boxing/unboxing overhead.
3. Cleaner, more readable, and maintainable code.
4. Industry standard — almost all modern .NET code (ASP.NET Core, Entity Framework, etc.) uses generics.

**When would you still use non-generic?**

- Maintaining old legacy applications (.NET Framework 1.x code).
- Interop with old COM components that expect non-generic collections.
- Extremely rare cases needing heterogeneous (mixed-type) storage without wrapping in a custom class — though even here, `List<object>` (generic with `object` as `T`) is usually preferred over `ArrayList` since it's semantically identical but part of the modern API.

> **Rule of thumb:** Always default to `System.Collections.Generic` unless you have a specific legacy or interop reason not to.

---

## 7. Quick Reference Cheat Sheet

```csharp
using System.Collections;          // Non-generic
using System.Collections.Generic;  // Generic

// Non-generic (avoid in new code)
ArrayList al = new ArrayList();
Hashtable ht = new Hashtable();

// Generic (preferred)
List<int> list = new List<int>();
Dictionary<string, int> dict = new Dictionary<string, int>();
Queue<string> queue = new Queue<string>();
Stack<string> stack = new Stack<string>();
HashSet<int> set = new HashSet<int>();
```

---

## Tags

#csharp #dotnet #generics #collections #programming-notes