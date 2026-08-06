# C# LINQ — Complete Method Reference (Grouped by Category)

## 1. Filtering

Return a subset of elements matching a condition.

|Method|Description|
|---|---|
|`Where`|Filters elements based on a predicate|
|`OfType<T>`|Filters elements based on their ability to be cast to a type|

```csharp
var evens = numbers.Where(n => n % 2 == 0);
var strings = mixedList.OfType<string>();
```

---

## 2. Projection (Transformation)

Transform each element into a new form.

|Method|Description|
|---|---|
|`Select`|Projects each element into a new form|
|`SelectMany`|Flattens sequences of sequences into one sequence|

```csharp
var names = people.Select(p => p.Name);
var allOrders = customers.SelectMany(c => c.Orders);
```

---

## 3. Ordering / Sorting

Arrange elements in a specific order.

|Method|Description|
|---|---|
|`OrderBy`|Sorts ascending by key|
|`OrderByDescending`|Sorts descending by key|
|`ThenBy`|Secondary ascending sort|
|`ThenByDescending`|Secondary descending sort|
|`Reverse`|Reverses the order of elements|

```csharp
var sorted = people
    .OrderBy(p => p.LastName)
    .ThenBy(p => p.FirstName);

var reversed = numbers.Reverse();
```

---

## 4. Grouping

Group elements sharing a common key.

|Method|Description|
|---|---|
|`GroupBy`|Groups elements by a key into `IGrouping<TKey, TElement>`|
|`ToLookup`|Similar to GroupBy but executes immediately (returns `ILookup`)|

```csharp
var byCity = people.GroupBy(p => p.City);
foreach (var g in byCity) 
    Console.WriteLine($"{g.Key}: {g.Count()}");
```

---

## 5. Joining

Combine two sequences based on matching keys.

|Method|Description|
|---|---|
|`Join`|Inner join between two sequences|
|`GroupJoin`|Join producing groups (like a left outer join)|

```csharp
var query = orders.Join(customers,
    o => o.CustomerId,
    c => c.Id,
    (o, c) => new { o.OrderId, c.Name });

var grouped = customers.GroupJoin(orders,
    c => c.Id,
    o => o.CustomerId,
    (c, ords) => new { c.Name, Orders = ords });
```

---

## 6. Set Operations

Compare or combine distinct sequences.

|Method|Description|
|---|---|
|`Distinct` / `DistinctBy`|Removes duplicate elements|
|`Union` / `UnionBy`|Combines two sequences, removing duplicates|
|`Intersect` / `IntersectBy`|Elements common to both sequences|
|`Except` / `ExceptBy`|Elements in first sequence not in second|

```csharp
var unique = list.Distinct();
var uniqueByAge = people.DistinctBy(p => p.Age);
var combined = listA.Union(listB);
var common = listA.Intersect(listB);
var diff = listA.Except(listB);
```

---

## 7. Aggregation

Compute a single value from a sequence.

|Method|Description|
|---|---|
|`Count` / `LongCount`|Number of elements (optionally matching predicate)|
|`Sum`|Sum of numeric values|
|`Min` / `MinBy`|Smallest value / element with smallest key|
|`Max` / `MaxBy`|Largest value / element with largest key|
|`Average`|Average of numeric values|
|`Aggregate`|Applies an accumulator function over a sequence|

```csharp
int total = numbers.Sum();
double avg = numbers.Average();
var oldest = people.MaxBy(p => p.Age);
int product = numbers.Aggregate((a, b) => a * b);
```

---

## 8. Quantifiers (Boolean Checks)

Test whether elements satisfy a condition.

|Method|Description|
|---|---|
|`Any`|True if any element matches (or sequence is non-empty)|
|`All`|True if all elements match|
|`Contains`|True if sequence contains a specific element|

```csharp
bool hasAdults = people.Any(p => p.Age >= 18);
bool allAdults = people.All(p => p.Age >= 18);
bool exists = numbers.Contains(5);
```

---

## 9. Element Access (Single Element Retrieval)

Retrieve one specific element.

|Method|Description|
|---|---|
|`First` / `FirstOrDefault`|First element (throws / returns default if none)|
|`Last` / `LastOrDefault`|Last element (throws / returns default if none)|
|`Single` / `SingleOrDefault`|Exactly one element (throws if 0 or >1 / returns default)|
|`ElementAt` / `ElementAtOrDefault`|Element at a given index|

```csharp
var first = list.First(p => p.Active);
var firstOrNull = list.FirstOrDefault(p => p.Active);
var only = list.Single(p => p.Id == 5);
var third = list.ElementAt(2);
```

---

## 10. Partitioning

Split a sequence into sections.

|Method|Description|
|---|---|
|`Skip`|Skips a number of elements|
|`Take`|Takes a number of elements|
|`SkipWhile`|Skips elements while condition is true|
|`TakeWhile`|Takes elements while condition is true|
|`Chunk`|Splits sequence into fixed-size chunks (returns `T[][]`)|

```csharp
var page2 = items.Skip(10).Take(10);
var untilFail = results.TakeWhile(r => r.Success);
var chunks = numbers.Chunk(3); // [[1,2,3],[4,5,6],...]
```

---

## 11. Conversion

Convert a sequence to another collection type.

|Method|Description|
|---|---|
|`ToList`|Converts to `List<T>`|
|`ToArray`|Converts to `T[]`|
|`ToDictionary`|Converts to `Dictionary<TKey, TValue>`|
|`ToHashSet`|Converts to `HashSet<T>`|
|`ToLookup`|Converts to `ILookup<TKey, TElement>`|
|`AsEnumerable`|Casts/returns as `IEnumerable<T>`|
|`AsQueryable`|Converts to `IQueryable<T>`|
|`Cast<T>`|Casts elements of a non-generic sequence to type `T`|

```csharp
var list = query.ToList();
var dict = people.ToDictionary(p => p.Id, p => p.Name);
var set = numbers.ToHashSet();
```

---

## 12. Generation

Create sequences without a source collection.

|Method|Description|
|---|---|
|`Enumerable.Range`|Generates a sequence of sequential integers|
|`Enumerable.Repeat`|Generates a sequence repeating one value|
|`Enumerable.Empty<T>`|Returns an empty sequence of type `T`|

```csharp
var range = Enumerable.Range(1, 10);       // 1..10
var repeated = Enumerable.Repeat("x", 5);  // x,x,x,x,x
var empty = Enumerable.Empty<int>();
```

---

## 13. Concatenation

Append sequences together.

|Method|Description|
|---|---|
|`Concat`|Concatenates two sequences|

```csharp
var combined = listA.Concat(listB);
```

---

## 14. Equality Comparison

Compare two sequences for equality.

|Method|Description|
|---|---|
|`SequenceEqual`|True if two sequences have equal elements in the same order|

```csharp
bool same = listA.SequenceEqual(listB);
```

---

## 15. Zipping (Merging Sequences)

Combine multiple sequences element-by-element.

|Method|Description|
|---|---|
|`Zip`|Merges two (or three) sequences using a selector function|

```csharp
var pairs = namesList.Zip(agesList, (n, a) => $"{n} is {a}");
```

---

## 16. Indexing (newer .NET)

Attach index information to elements.

|Method|Description|
|---|---|
|`Index()` (.NET 9+)|Returns sequence of `(int Index, T Item)` tuples|
|`Select((x, i) => ...)`|Classic overload with index parameter|

```csharp
foreach (var (index, item) in items.Index())
    Console.WriteLine($"{index}: {item}");

var withIndex = items.Select((item, i) => $"{i}: {item}");
```

---

## Quick Category Summary

|Category|Key Methods|
|---|---|
|Filtering|Where, OfType|
|Projection|Select, SelectMany|
|Ordering|OrderBy, OrderByDescending, ThenBy, ThenByDescending, Reverse|
|Grouping|GroupBy, ToLookup|
|Joining|Join, GroupJoin|
|Set Operations|Distinct, Union, Intersect, Except (+ `By` variants)|
|Aggregation|Count, Sum, Min, Max, Average, Aggregate, MinBy, MaxBy|
|Quantifiers|Any, All, Contains|
|Element Access|First(OrDefault), Last(OrDefault), Single(OrDefault), ElementAt(OrDefault)|
|Partitioning|Skip, Take, SkipWhile, TakeWhile, Chunk|
|Conversion|ToList, ToArray, ToDictionary, ToHashSet, ToLookup, Cast|
|Generation|Range, Repeat, Empty|
|Concatenation|Concat|
|Equality|SequenceEqual|
|Zipping|Zip|
|Indexing|Index, Select with index|

> **Note:** Most methods have two flavors — **method syntax** (`.Where(...)`) and **query syntax** (`from x in ... where ...`). Query syntax only supports a subset (Where, Select, GroupBy, Join, OrderBy, etc.) — aggregation, conversion, and quantifier methods must be called via method syntax.


# C# LINQ — Time Complexity of All Methods (with Reasons)

LINQ methods operate on `IEnumerable<T>`. Most are **O(n)** because they must walk the sequence once. The exceptions fall into three buckets:

|Bucket|Why|
|---|---|
|**O(1)**|No iteration needed — just returns a wrapped object or reads a known count|
|**O(n)**|Must visit every element exactly once (the vast majority of LINQ)|
|**O(n log n)**|Needs sorting internally|

Also important: LINQ uses **deferred execution** for most methods (`Where`, `Select`, `OrderBy`, etc.) — the complexity below is the cost **when the sequence is actually enumerated** (e.g. via `foreach`, `.ToList()`, `.Count()`), not at the moment you write the LINQ line.

---

## 1. Filtering

|Method|Complexity|Reason|
|---|---|---|
|`Where`|**O(n)**|Must test the predicate against every element once as it streams through.|
|`OfType<T>`|**O(n)**|Must type-check (`is`) every element once to decide whether to keep it.|

---

## 2. Projection

|Method|Complexity|Reason|
|---|---|---|
|`Select`|**O(n)**|Applies the selector to every element exactly once.|
|`SelectMany`|**O(n × m)**|Visits each of the n outer elements, and for each one iterates its inner sequence of average length m — total work is proportional to the total number of flattened items.|

---

## 3. Ordering / Sorting

|Method|Complexity|Reason|
|---|---|---|
|`OrderBy` / `OrderByDescending`|**O(n log n)**|Internally uses a stable sort (comparison-based, similar to a merge/quicksort hybrid) — same lower bound as any comparison sort.|
|`ThenBy` / `ThenByDescending`|**O(n log n)**|Doesn't re-sort from scratch — it only breaks ties within groups already equal from the prior sort — but overall complexity is still bounded by O(n log n) because it's still one comparison-based sort pass.|
|`Reverse`|**O(n)**|Needs to know the full sequence length first (buffers it), then outputs back-to-front — one pass to buffer, one pass to emit.|

---

## 4. Grouping

|Method|Complexity|Reason|
|---|---|---|
|`GroupBy`|**O(n)** average|Uses an internal **hash table** keyed by the grouping key — each element is hashed once and bucketed, so it's linear, not quadratic (common misconception).|
|`ToLookup`|**O(n)**|Same hash-table mechanism as `GroupBy`, but executes **immediately** (eager) instead of being deferred.|

---

## 5. Joining

|Method|Complexity|Reason|
|---|---|---|
|`Join`|**O(n + m)** average|Builds a hash table from the second (inner) sequence in O(m), then does one O(n) pass over the outer sequence doing O(1) average hash lookups — this is why LINQ `Join` is much faster than a naive nested-loop join, which would be O(n × m).|
|`GroupJoin`|**O(n + m)** average|Same hash-table strategy as `Join`, but groups all matches per outer element instead of flattening them into pairs.|

---

## 6. Set Operations

|Method|Complexity|Reason|
|---|---|---|
|`Distinct` / `DistinctBy`|**O(n)** average|Uses a hash set internally to track "seen" values — each element does an O(1) average hash insert/check.|
|`Union` / `UnionBy`|**O(n + m)** average|Concatenates conceptually, then removes duplicates the same way `Distinct` does — using a hash set.|
|`Intersect` / `IntersectBy`|**O(n + m)** average|Builds a hash set from one sequence (O(m)), then scans the other checking membership (O(n) with O(1) lookups).|
|`Except` / `ExceptBy`|**O(n + m)** average|Same hash-set membership-check strategy as `Intersect`, just keeping non-matches instead of matches.|

---

## 7. Aggregation

|Method|Complexity|Reason|
|---|---|---|
|`Count` / `LongCount` (no predicate)|**O(1)** if source implements `ICollection<T>` (like `List<T>`, arrays)|Reads a stored count field directly instead of iterating.|
|`Count` / `LongCount` (with predicate)|**O(n)**|Must test every element against the predicate — no shortcut available.|
|`Sum`|**O(n)**|Must add every element's value once.|
|`Min` / `Max`|**O(n)**|Must compare every element once to track the running smallest/largest.|
|`MinBy` / `MaxBy`|**O(n)**|Same single pass, comparing on the computed key instead of the raw value.|
|`Average`|**O(n)**|Requires a full pass to sum values (and count them if not already known).|
|`Aggregate`|**O(n)**|Applies the accumulator function once per element — inherently sequential, single pass.|

---

## 8. Quantifiers

|Method|Complexity|Reason|
|---|---|---|
|`Any` (no predicate)|**O(1)**|Just checks whether the sequence has at least one element (calls `MoveNext()` once); doesn't scan the whole thing.|
|`Any` (with predicate)|**O(n)** worst case|Stops as soon as it finds a match, but worst case (no match, or match at the end) it checks every element.|
|`All`|**O(n)** worst case|Stops as soon as it finds a failure, but worst case (all match) it checks every element.|
|`Contains`|**O(n)** — unless source is `HashSet<T>`/`Dictionary`, then O(1) average|For plain `IEnumerable<T>`/`List<T>` it's a linear scan; LINQ only gets the fast path if the underlying collection itself supports hash lookup.|

---

## 9. Element Access

|Method|Complexity|Reason|
|---|---|---|
|`First` / `FirstOrDefault` (no predicate)|**O(1)** if source is indexable (`IList<T>`), else **O(n)** worst case for plain enumerables|With direct indexing it just reads index 0; otherwise it must call `MoveNext()` once, which for lazy generators can involve real work.|
|`First` / `FirstOrDefault` (with predicate)|**O(n)** worst case|Scans until the first match is found; stops early, but worst case scans everything.|
|`Last` / `LastOrDefault` (no predicate)|**O(1)** if source is `IList<T>` (indexable), else **O(n)**|Indexable collections can jump straight to `Count - 1`; otherwise must iterate the whole sequence to find the end.|
|`Last` / `LastOrDefault` (with predicate)|**O(n)**|Must scan the whole sequence since a later match could always override an earlier one.|
|`Single` / `SingleOrDefault`|**O(n)**|Must keep scanning even after finding one match, to verify no second match exists (needed to correctly throw on duplicates).|
|`ElementAt` / `ElementAtOrDefault`|**O(1)** if source is `IList<T>`, else **O(n)**|Indexable collections do direct offset access; plain enumerables must step through `index` elements one at a time.|

---

## 10. Partitioning

|Method|Complexity|Reason|
|---|---|---|
|`Skip`|**O(k)** to skip + **O(n−k)** to enumerate rest → effectively **O(n)** total|Must advance the enumerator k times before yielding anything (no way to "jump ahead" on a general sequence).|
|`Take`|**O(k)**|Stops enumerating the source as soon as k elements have been yielded — never touches the rest.|
|`SkipWhile`|**O(n)** worst case|Must check the predicate against each element in order until it first becomes false, then yield everything after.|
|`TakeWhile`|**O(n)** worst case, but stops early|Yields elements until the predicate first fails, then stops — cost is proportional to how far into the sequence the break point is.|
|`Chunk`|**O(n)**|One full pass, buffering elements into fixed-size arrays as it goes.|

---

## 11. Conversion

|Method|Complexity|Reason|
|---|---|---|
|`ToList`|**O(n)**|Copies every element into a new internal array-backed list.|
|`ToArray`|**O(n)**|Same as `ToList` — must copy every element once into a new array.|
|`ToDictionary`|**O(n)** average|Inserts each element into a hash table keyed by the selector — O(1) average per insert.|
|`ToHashSet`|**O(n)** average|Same hash-table insertion logic as `ToDictionary`, minus values.|
|`ToLookup`|**O(n)** average|Same as `GroupBy`, but eager — bucket each element into a hash-based multi-map.|
|`AsEnumerable`|**O(1)**|Just a type-level cast/wrapper — does no iteration at all.|
|`AsQueryable`|**O(1)**|Wraps the source in a queryable provider — no iteration happens until the query executes.|
|`Cast<T>`|**O(n)** when enumerated|Must cast each element individually as it streams through (deferred, lazy per-element cost).|

---

## 12. Generation

|Method|Complexity|Reason|
|---|---|---|
|`Enumerable.Range`|**O(n)** when enumerated|Lazily yields n integers one at a time — cost is proportional to how many are actually consumed.|
|`Enumerable.Repeat`|**O(n)** when enumerated|Lazily yields the same value n times.|
|`Enumerable.Empty<T>`|**O(1)**|Returns a cached, pre-built empty sequence — no work at all.|

---

## 13. Concatenation

|Method|Complexity|Reason|
|---|---|---|
|`Concat`|**O(n + m)** when enumerated|Must stream through all of the first sequence, then all of the second — no way to combine faster than visiting every element once.|

---

## 14. Equality Comparison

|Method|Complexity|Reason|
|---|---|---|
|`SequenceEqual`|**O(n)**, short-circuits on first mismatch|Compares elements pairwise in order and stops immediately at the first unequal pair (or length mismatch).|

---

## 15. Zipping

|Method|Complexity|Reason|
|---|---|---|
|`Zip`|**O(min(n, m))**|Stops as soon as the shorter of the two (or three) sequences runs out — never reads past that.|

---

## 16. Indexing

|Method|Complexity|Reason|
|---|---|---|
|`Index()` (.NET 9+)|**O(n)** when enumerated|Just wraps each element with a running counter — one lightweight pass.|
|`Select((x, i) => ...)`|**O(n)**|Same as regular `Select`, the index parameter is just a counter maintained alongside the single pass.|

---

## 🎯 Quick Cheat Sheet (Grouped by Speed)

### ⚡ O(1)

- `AsEnumerable`, `AsQueryable`, `Enumerable.Empty<T>`
- `Any()` (no predicate)
- `Count()` — only if source is `ICollection<T>`
- `First`/`Last`/`ElementAt` — only if source is `IList<T>`

### 🐢 O(n) — the vast majority

- `Where`, `Select`, `Reverse`, `Sum`, `Min`, `Max`, `Average`, `Aggregate`
- `Distinct`, `Union`, `Intersect`, `Except` (hash-based, so O(n) not O(n²))
- `Join`, `GroupJoin`, `GroupBy`, `ToLookup` (also hash-based)
- `ToList`, `ToArray`, `ToDictionary`, `ToHashSet`
- `Skip`, `TakeWhile`, `SkipWhile`, `Chunk`, `Concat`, `SequenceEqual`

### 🐌 O(n log n)

- `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`

---

## 💡 The #1 Misconception to Avoid

A lot of people assume `Join`, `Distinct`, `GroupBy`, `Intersect`, and `Except` are **O(n²)** because "comparing two sequences sounds like nested loops." They are **not** — LINQ's implementation uses **hash tables** internally for all of these, which is exactly why they're O(n) or O(n+m) average case instead of quadratic. The only genuinely O(n²)-risk LINQ pattern is when you manually nest two `Where`/`Any` calls yourself, e.g.:

```csharp
// This IS O(n * m) — you wrote the nested loop yourself
var result = listA.Where(a => listB.Any(b => b.Id == a.Id));

// This is O(n + m) — LINQ's Join uses a hash table internally
var result = listA.Join(listB, a => a.Id, b => b.Id, (a, b) => a);
```

## ⚠️ Worst-Case Note on Hash-Based Methods

All the "O(n) average" hash-table-backed methods (`GroupBy`, `Distinct`, `Join`, `ToDictionary`, etc.) can theoretically degrade toward **O(n²)** in a pathological worst case — if the key type has a terrible `GetHashCode()` implementation causing constant hash collisions. In practice, with well-behaved keys (strings, ints, records with proper hashing), you'll always see the average-case O(n) or O(n+m) behavior.