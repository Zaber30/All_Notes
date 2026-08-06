# C# Built-in API Design Strategies

Instead of memorizing thousands of methods, memorize **how each type is designed**. Almost every built-in .NET type follows one of these four strategies.

---

# 1. Immutability Strategy (Return-by-Value)

## Description

The object **cannot be modified**.

Every method returns a **new object** containing the changes.

You must assign the returned value.

## Format

```csharp
variable = variable.Method();
```

## Important Types

### Text

- string

### Date & Time

- DateTime
- DateTimeOffset
- TimeSpan

### Numeric Value Types (Structs)

- int
- long
- short
- byte
- float
- double
- decimal
- bool
- char

> Primitive value types don't usually have many modifying methods, but they are immutable.

### GUID

- Guid

### Big Numbers

- BigInteger

### Immutable Collections (System.Collections.Immutable)

- ImmutableList<T>
- ImmutableArray<T>
- ImmutableDictionary<TKey,TValue>
- ImmutableHashSet<T>
- ImmutableQueue<T>
- ImmutableStack<T>

## Examples

```csharp
name = name.ToLower();

today = today.AddDays(1);

text = text.Replace("A", "B");
```

---

# 2. External Utility Strategy (Static Helper Classes)

## Description

The object contains raw data.

A separate **static class** performs operations.

## Format

```csharp
UtilityClass.Method(data);
```

## Arrays

- int[]
- string[]
- char[]
- double[]
- float[]
- decimal[]
- bool[]
- object[]

Uses:

```csharp
Array.Sort(arr);
Array.Reverse(arr);
Array.Clear(arr);
Array.Copy(arr1, arr2);
Array.Resize(ref arr, size);
Array.BinarySearch(arr, value);
Array.IndexOf(arr, value);
```

---

## Math Utilities

Class:

- Math

Methods:

- Abs()
- Max()
- Min()
- Pow()
- Sqrt()
- Ceiling()
- Floor()
- Round()
- Clamp()
- Log()
- Exp()
- Sin()
- Cos()
- Tan()

---

## File System

Classes:

- File
- Directory
- Path
- DriveInfo

Examples

```csharp
File.ReadAllText();

File.WriteAllText();

Directory.GetFiles();

Path.Combine();
```

---

## Conversion

Classes

- Convert

Methods

```csharp
Convert.ToInt32()

Convert.ToDouble()

Convert.ToBoolean()

Convert.ToString()
```

---

## Environment

Class

- Environment

Examples

```csharp
Environment.CurrentDirectory

Environment.Exit()

Environment.GetFolderPath()
```

---

## Console

Class

- Console

Examples

```csharp
Console.WriteLine();

Console.ReadLine();

Console.Clear();
```

---

## Random

Class

- Random

Example

```csharp
Random random = new Random();

random.Next();
```

---

# 3. Encapsulated Object Strategy (In-Place Mutation)

## Description

The object owns both data and behavior.

Methods directly modify the object.

No reassignment is required.

## Format

```csharp
object.Method();
```

---

## Generic Collections

### List<T>

Common methods

- Add()
- AddRange()
- Remove()
- RemoveAt()
- RemoveAll()
- Insert()
- InsertRange()
- Sort()
- Reverse()
- Clear()
- Contains()
- IndexOf()
- Find()
- FindAll()

---

### Dictionary<TKey,TValue>

Methods

- Add()
- Remove()
- Clear()
- ContainsKey()
- ContainsValue()
- TryGetValue()

---

### HashSet<T>

Methods

- Add()
- Remove()
- Contains()
- UnionWith()
- IntersectWith()
- ExceptWith()
- Clear()

---

### Queue<T>

Methods

- Enqueue()
- Dequeue()
- Peek()
- Clear()
- Contains()

---

### Stack<T>

Methods

- Push()
- Pop()
- Peek()
- Clear()
- Contains()

---

### LinkedList<T>

Methods

- AddFirst()
- AddLast()
- Remove()
- RemoveFirst()
- RemoveLast()
- Find()
- Clear()

---

### SortedSet<T>

Methods

- Add()
- Remove()
- Contains()
- UnionWith()
- IntersectWith()

---

### PriorityQueue<TElement,TPriority>

Methods

- Enqueue()
- Dequeue()
- Peek()
- Clear()

---

## Mutable Text

### StringBuilder

Methods

- Append()
- AppendLine()
- Insert()
- Replace()
- Remove()
- Clear()

---

## Streams

- MemoryStream
- FileStream
- BufferedStream

Methods

- Read()
- Write()
- Flush()
- Seek()

---

# 4. Extension / LINQ Strategy

## Description

Extension methods add extra functionality to collections.

They **do not modify** the original collection.

Instead they return a new sequence (`IEnumerable<T>`).

## Format

```csharp
var result = collection.Method(...);
```

---

## Works With

- Arrays
- List<T>
- Dictionary<TKey,TValue>
- HashSet<T>
- Queue<T>
- Stack<T>
- LinkedList<T>
- SortedSet<T>
- Immutable Collections
- Any IEnumerable<T>

---

## Filtering

- Where()
- OfType()

---

## Projection

- Select()
- SelectMany()

---

## Sorting

- OrderBy()
- OrderByDescending()
- ThenBy()
- ThenByDescending()
- Reverse()

---

## Searching

- First()
- FirstOrDefault()
- Last()
- LastOrDefault()
- Single()
- SingleOrDefault()

---

## Element Operations

- ElementAt()
- ElementAtOrDefault()

---

## Quantifiers

- Any()
- All()
- Contains()

---

## Counting

- Count()
- LongCount()

---

## Aggregation

- Sum()
- Average()
- Min()
- Max()
- Aggregate()

---

## Set Operations

- Distinct()
- Union()
- Intersect()
- Except()

---

## Paging

- Skip()
- Take()
- SkipWhile()
- TakeWhile()

---

## Grouping

- GroupBy()
- ToLookup()

---

## Joining

- Join()
- GroupJoin()
- Zip()

---

## Conversion

- ToList()
- ToArray()
- ToDictionary()
- ToHashSet()

---

## Generation

- Empty()
- Range()
- Repeat()

---

## Examples

```csharp
var even = numbers.Where(x => x % 2 == 0);

var sorted = numbers.OrderBy(x => x);

var names = students.Select(x => x.Name);

var total = numbers.Sum();

var list = numbers.ToList();
```

---

# Summary Table

| Category | Strategy | Examples | Changes Original? |
|-----------|----------|----------|-------------------|
| Text | Immutability | string | ❌ No |
| Date & Time | Immutability | DateTime, TimeSpan | ❌ No |
| Primitive Types | Immutability | int, double, bool, char | ❌ No |
| Immutable Collections | Immutability | ImmutableList, ImmutableDictionary | ❌ No |
| Arrays | External Utility | Array.Sort(), Array.Reverse() | ✅ Yes |
| Math | External Utility | Math.Sqrt(), Math.Pow() | N/A |
| File System | External Utility | File, Directory, Path | Depends |
| Conversion | External Utility | Convert | N/A |
| Console | External Utility | Console.WriteLine() | N/A |
| List | Encapsulated | Add(), Remove(), Sort() | ✅ Yes |
| Dictionary | Encapsulated | Add(), Remove() | ✅ Yes |
| HashSet | Encapsulated | Add(), UnionWith() | ✅ Yes |
| Queue | Encapsulated | Enqueue(), Dequeue() | ✅ Yes |
| Stack | Encapsulated | Push(), Pop() | ✅ Yes |
| LinkedList | Encapsulated | AddFirst(), RemoveLast() | ✅ Yes |
| SortedSet | Encapsulated | Add(), Remove() | ✅ Yes |
| PriorityQueue | Encapsulated | Enqueue(), Dequeue() | ✅ Yes |
| StringBuilder | Encapsulated | Append(), Replace() | ✅ Yes |
| Streams | Encapsulated | Read(), Write() | ✅ Yes |
| LINQ | Extension Methods | Where(), Select(), OrderBy(), GroupBy(), Sum() | ❌ No |