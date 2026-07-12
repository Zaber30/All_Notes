

## tags: [csharp, generics, collections, methods, cheatsheet] created: 2026-07-09

# C# Generic Collections — Classes, Interfaces & Common Methods

`using System.Collections.Generic;`

---

## 1. `List<T>`

Dynamic, resizable array. Most commonly used collection.

| Method / Property           | Description                             |     |
| --------------------------- | --------------------------------------- | --- |
| `Add(T item)`               | Adds an item to the end                 |     |
| `AddRange(IEnumerable<T>)`  | Adds multiple items                     |     |
| `Insert(int index, T item)` | Inserts item at a specific index        |     |
| `Remove(T item)`            | Removes first occurrence of item        |     |
| `RemoveAt(int index)`       | Removes item at index                   |     |
| `RemoveAll(Predicate<T>)`   | Removes all items matching condition    |     |
| `Contains(T item)`          | Checks if item exists                   |     |
| `IndexOf(T item)`           | Returns index of item (-1 if not found) |     |
| `Sort()`                    | Sorts the list                          |     |
| `Reverse()`                 | Reverses the order                      |     |
| `Clear()`                   | Removes all items                       |     |
| `Count`                     | Number of items                         |     |
| `Find(Predicate<T>)`        | Finds first matching item               |     |
| `FindAll(Predicate<T>)`     | Finds all matching items                |     |
| `ToArray()`                 | Converts to array                       |     |

```csharp
List<string> names = new List<string>();
names.Add("Rahim");
names.Add("Karim");
names.Insert(1, "Jamal");
names.Remove("Karim");

foreach (var name in names)
    Console.WriteLine(name);

bool exists = names.Contains("Rahim");   // true
int index = names.IndexOf("Jamal");      // 1
names.Sort();
Console.WriteLine(names.Count);          // 2
```

---

## 2. `Dictionary<TKey, TValue>`

Stores key-value pairs. Fast lookup by key (hash-based).

|Method / Property|Description|
|---|---|
|`Add(TKey, TValue)`|Adds a key-value pair|
|`Remove(TKey)`|Removes entry by key|
|`ContainsKey(TKey)`|Checks if key exists|
|`ContainsValue(TValue)`|Checks if value exists|
|`TryGetValue(TKey, out TValue)`|Safely gets value without exception|
|`Clear()`|Removes all entries|
|`Keys`|Collection of all keys|
|`Values`|Collection of all values|
|`Count`|Number of entries|
|`this[TKey]`|Indexer to get/set value by key|

```csharp
Dictionary<string, int> ages = new Dictionary<string, int>();
ages.Add("Rahim", 25);
ages["Karim"] = 30;   // Add or update via indexer

if (ages.ContainsKey("Rahim"))
    Console.WriteLine(ages["Rahim"]);

if (ages.TryGetValue("Karim", out int age))
    Console.WriteLine(age);

foreach (KeyValuePair<string, int> kv in ages)
    Console.WriteLine($"{kv.Key} = {kv.Value}");

ages.Remove("Rahim");
```

---

## 3. `Queue<T>`

FIFO — First In, First Out.

|Method / Property|Description|
|---|---|
|`Enqueue(T item)`|Adds item to the end|
|`Dequeue()`|Removes and returns front item|
|`Peek()`|Views front item without removing|
|`Contains(T item)`|Checks if item exists|
|`Clear()`|Removes all items|
|`Count`|Number of items|
|`ToArray()`|Converts to array|

```csharp
Queue<string> tickets = new Queue<string>();
tickets.Enqueue("Ticket1");
tickets.Enqueue("Ticket2");

string next = tickets.Dequeue();  // "Ticket1"
string front = tickets.Peek();    // "Ticket2"
Console.WriteLine(tickets.Count); // 1
```

---

## 4. `Stack<T>`

LIFO — Last In, First Out.

|Method / Property|Description|
|---|---|
|`Push(T item)`|Adds item to the top|
|`Pop()`|Removes and returns top item|
|`Peek()`|Views top item without removing|
|`Contains(T item)`|Checks if item exists|
|`Clear()`|Removes all items|
|`Count`|Number of items|

```csharp
Stack<string> history = new Stack<string>();
history.Push("Page1");
history.Push("Page2");

string last = history.Pop();   // "Page2"
string top = history.Peek();   // "Page1"
```

---

## 5. `HashSet<T>`

Unordered collection of **unique** elements. Great for fast lookups & set operations.

|Method / Property|Description|
|---|---|
|`Add(T item)`|Adds item (ignored if duplicate)|
|`Remove(T item)`|Removes item|
|`Contains(T item)`|Checks existence|
|`UnionWith(IEnumerable<T>)`|Combines two sets|
|`IntersectWith(IEnumerable<T>)`|Keeps only common elements|
|`ExceptWith(IEnumerable<T>)`|Removes elements found in other set|
|`Clear()`|Removes all items|
|`Count`|Number of items|

```csharp
HashSet<int> setA = new HashSet<int> { 1, 2, 3 };
HashSet<int> setB = new HashSet<int> { 2, 3, 4 };

setA.Add(5);
setA.UnionWith(setB);       // { 1,2,3,4,5 }
setA.IntersectWith(setB);   // { 2,3,4 }
Console.WriteLine(setA.Contains(2)); // true
```

---

## 6. `LinkedList<T>`

Doubly linked list — efficient insertion/removal at any point.

|Method / Property|Description|
|---|---|
|`AddFirst(T item)`|Adds item at the start|
|`AddLast(T item)`|Adds item at the end|
|`Remove(T item)`|Removes first matching item|
|`RemoveFirst()`|Removes first node|
|`RemoveLast()`|Removes last node|
|`Find(T item)`|Finds first matching node|
|`Count`|Number of items|

```csharp
LinkedList<string> tasks = new LinkedList<string>();
tasks.AddLast("Task1");
tasks.AddLast("Task2");
tasks.AddFirst("UrgentTask");

tasks.RemoveLast();
foreach (var t in tasks)
    Console.WriteLine(t);
```

---

## 7. `SortedList<TKey, TValue>`

Key-value pairs automatically sorted by key.

|Method / Property|Description|
|---|---|
|`Add(TKey, TValue)`|Adds sorted entry|
|`Remove(TKey)`|Removes by key|
|`ContainsKey(TKey)`|Checks key existence|
|`Keys` / `Values`|Sorted keys/values collections|

```csharp
SortedList<int, string> rank = new SortedList<int, string>();
rank.Add(3, "Karim");
rank.Add(1, "Rahim");
rank.Add(2, "Jamal");

foreach (var kv in rank)
    Console.WriteLine($"{kv.Key}: {kv.Value}"); // Auto sorted by key
```

---

## 8. `SortedDictionary<TKey, TValue>` & `SortedSet<T>`

Like `Dictionary`/`HashSet` but keep elements sorted (tree-based, faster inserts for large data than `SortedList`).

```csharp
SortedDictionary<string, int> scores = new SortedDictionary<string, int>();
scores["Zara"] = 90;
scores["Amina"] = 85;
// Iterates in sorted key order: Amina, Zara

SortedSet<int> nums = new SortedSet<int> { 5, 1, 3 };
// Iterates as: 1, 3, 5
```

---

## 9. Generic Interfaces

### `IEnumerable<T>`

Base interface for iteration (`foreach`).

```csharp
IEnumerable<int> numbers = new List<int> { 1, 2, 3 };
foreach (int n in numbers)
    Console.WriteLine(n);
```

### `ICollection<T>`

Extends `IEnumerable<T>`; adds `Add`, `Remove`, `Count`, `Contains`, `Clear`.

```csharp
ICollection<string> items = new List<string>();
items.Add("A");
items.Remove("A");
Console.WriteLine(items.Count);
```

### `IList<T>`

Extends `ICollection<T>`; adds index-based access.

```csharp
IList<string> list = new List<string> { "A", "B" };
list[0] = "Z";              // index access
list.Insert(1, "New");
```

### `IDictionary<TKey, TValue>`

Key-value pair contract.

```csharp
IDictionary<string, int> dict = new Dictionary<string, int>();
dict.Add("Rahim", 25);
dict.TryGetValue("Rahim", out int val);
```

### `IComparable<T>`

Allows an object to define its own default sort order (`CompareTo`).

```csharp
public class Employee : IComparable<Employee>
{
    public int Salary;
    public int CompareTo(Employee other) => Salary.CompareTo(other.Salary);
}
List<Employee> employees = new List<Employee> { /* ... */ };
employees.Sort(); // Uses CompareTo
```

### `IComparer<T>`

Defines a **custom** external comparison (used with `Sort(IComparer<T>)`).

```csharp
public class NameComparer : IComparer<Employee>
{
    public int Compare(Employee a, Employee b) => a.Name.CompareTo(b.Name);
}
employees.Sort(new NameComparer());
```

### `IReadOnlyList<T>` / `IReadOnlyCollection<T>`

Exposes read-only view of a collection (no Add/Remove).

```csharp
IReadOnlyList<string> readOnlyNames = new List<string> { "A", "B" };
Console.WriteLine(readOnlyNames[0]); // Read allowed
// readOnlyNames.Add("C"); ❌ Not allowed
```

---

## 10. Quick Method Cheat Sheet (Most Used Across Collections)

|Method|Available In|
|---|---|
|`Add()`|List, Dictionary, HashSet, LinkedList, SortedList|
|`Remove()`|List, Dictionary, HashSet, LinkedList, SortedList, Queue*, Stack*|
|`Contains()`|List, Dictionary, HashSet, Queue, Stack|
|`Count`|All collections|
|`Clear()`|All collections|
|`foreach` iteration|All (via `IEnumerable<T>`)|
|`TryGetValue()`|Dictionary, SortedDictionary|
|`Sort()`|List|
|`Peek()`|Queue, Stack|

*Queue/Stack don't have `Remove()` — they use `Dequeue()`/`Pop()` instead.

---

## 11. LINQ Extension Methods (bonus — work on any `IEnumerable<T>`)

```csharp
using System.Linq;

List<int> nums = new List<int> { 5, 3, 8, 1 };

var sorted   = nums.OrderBy(n => n).ToList();
var filtered = nums.Where(n => n > 3).ToList();
var first    = nums.FirstOrDefault(n => n > 5);
var sum      = nums.Sum();
var max      = nums.Max();
var any      = nums.Any(n => n > 10);   // false
```

---

## Tags

#csharp #dotnet #generics #collections #methods #cheatsheet