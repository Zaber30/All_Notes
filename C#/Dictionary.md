# C# `Dictionary<TKey,TValue>` vs `SortedDictionary<TKey,TValue>`

## All Methods — Examples, Time Complexity, and Reasoning

---

## 🔑 The Core Idea to Remember

|Type|Internal Structure|Why it behaves the way it does|
|---|---|---|
|`Dictionary<TKey,TValue>`|**Hash table** (array of buckets, each key mapped via `GetHashCode()`)|O(1) average for lookup/insert/delete because you jump straight to a bucket instead of searching — but **no ordering** is maintained.|
|`SortedDictionary<TKey,TValue>`|**Red-Black Tree** (self-balancing binary search tree)|O(log n) for lookup/insert/delete because it must walk down a balanced tree — but keys are **always kept in sorted order**.|

This single structural difference explains almost every complexity difference below.

---

## 📋 `Dictionary<TKey,TValue>` — Full Method Table

```csharp
var dict = new Dictionary<string, int>();
```

|Method / Operation|Example|Time Complexity|Reason|
|---|---|---|---|
|`Add(key, value)`|`dict.Add("a", 1);`|**O(1)** average, O(n) worst case|Hashes the key to find its bucket directly. Worst case only happens on hash collisions or when the internal array must resize (rare, amortized away).|
|`this[key]` (get)|`int x = dict["a"];`|**O(1)** average|Hashes the key, jumps to the bucket, and scans a short chain (ideally length 1) for a match.|
|`this[key]` (set)|`dict["a"] = 5;`|**O(1)** average|Same hash-bucket lookup as get, then overwrites or inserts.|
|`TryGetValue(key, out val)`|`dict.TryGetValue("a", out int v);`|**O(1)** average|Identical mechanism to indexer get, just without throwing if missing.|
|`ContainsKey(key)`|`dict.ContainsKey("a");`|**O(1)** average|Same hash lookup — checks bucket existence without retrieving the value.|
|`ContainsValue(value)`|`dict.ContainsValue(5);`|**O(n)**|Values aren't hashed or indexed — must linearly scan every entry to find a match.|
|`Remove(key)`|`dict.Remove("a");`|**O(1)** average|Hashes to the bucket, unlinks the entry — no shifting needed (unlike an array).|
|`Remove(key, out value)`|`dict.Remove("a", out int v);`|**O(1)** average|Same as `Remove`, just also returns the removed value before deleting.|
|`Clear()`|`dict.Clear();`|**O(n)**|Must clear references in every bucket slot so the GC can reclaim any stored objects.|
|`Count`|`int n = dict.Count;`|**O(1)**|Maintained as a running field, not computed by counting entries.|
|`Keys`|`foreach (var k in dict.Keys)`|**O(1)** to get the view, **O(n)** to enumerate|Returns a lazy collection view instantly; iterating it visits every key once.|
|`Values`|`foreach (var v in dict.Values)`|**O(1)** to get the view, **O(n)** to enumerate|Same as `Keys` — instant view, O(n) full iteration.|
|`foreach` over dictionary|`foreach (var kv in dict)`|**O(n)**|Must visit every bucket slot once to yield all key-value pairs.|
|`TryAdd(key, value)`|`dict.TryAdd("a", 1);`|**O(1)** average|Same hash-bucket check as `ContainsKey` + insert, combined into one atomic-feeling call.|
|`EnsureCapacity(n)`|`dict.EnsureCapacity(100);`|**O(n)** if it triggers a resize|Pre-allocates bucket array to the target size — requires rehashing existing entries into the new array.|
|`TrimExcess()`|`dict.TrimExcess();`|**O(n)**|Shrinks internal arrays to fit current count — must rehash/copy entries into the smaller array.|
|`GetEnumerator()`|(used by `foreach`)|**O(1)** to create, O(n) to fully consume|Creating the enumerator is instant; walking it to the end is linear.|

---

## 📋 `SortedDictionary<TKey,TValue>` — Full Method Table

```csharp
var sorted = new SortedDictionary<string, int>();
```

|Method / Operation|Example|Time Complexity|Reason|
|---|---|---|---|
|`Add(key, value)`|`sorted.Add("a", 1);`|**O(log n)**|Walks down the red-black tree comparing keys to find the correct sorted position, then possibly rebalances — tree height is always O(log n).|
|`this[key]` (get)|`int x = sorted["a"];`|**O(log n)**|Binary-search-style traversal down the tree comparing keys at each node.|
|`this[key]` (set)|`sorted["a"] = 5;`|**O(log n)**|Same tree traversal as get, then updates or inserts the node.|
|`TryGetValue(key, out val)`|`sorted.TryGetValue("a", out int v);`|**O(log n)**|Same tree descent as indexer get, just without throwing if absent.|
|`ContainsKey(key)`|`sorted.ContainsKey("a");`|**O(log n)**|Tree traversal comparing keys — same cost as a lookup, minus retrieving the value.|
|`ContainsValue(value)`|`sorted.ContainsValue(5);`|**O(n)**|Values aren't ordered by the tree (only keys are) — must scan every node.|
|`Remove(key)`|`sorted.Remove("a");`|**O(log n)**|Finds the node in O(log n), then removes it and rebalances the tree (also O(log n)).|
|`Clear()`|`sorted.Clear();`|**O(n)**|Must release/clear every node in the tree.|
|`Count`|`int n = sorted.Count;`|**O(1)**|Maintained as a running field, same as `Dictionary`.|
|`Keys`|`foreach (var k in sorted.Keys)`|**O(1)** to get view, **O(n)** to enumerate — **and always in sorted order**|An in-order tree traversal naturally visits keys from smallest to largest.|
|`Values`|`foreach (var v in sorted.Values)`|**O(1)** to get view, **O(n)** to enumerate — ordered by key|Values come out in the order of their corresponding (sorted) keys, via the same in-order traversal.|
|`foreach` over dictionary|`foreach (var kv in sorted)`|**O(n)**, yields pairs in **ascending key order**|In-order traversal of the tree — this ordering guarantee is the entire reason to choose `SortedDictionary` over `Dictionary`.|
|`Min` (first entry)|`var first = sorted.First();` (via LINQ)|**O(log n)**|Just walk left-child pointers from the root to the leftmost node — no full scan needed.|
|`Max` (last entry)|`var last = sorted.Last();` (via LINQ)|**O(log n)**|Same idea, walking right-child pointers to the rightmost node.|
|`GetEnumerator()`|(used by `foreach`)|**O(1)** to create, O(n) to fully consume, **always sorted**|Sets up an in-order traversal iterator; consuming it takes linear time due to visiting every node once.|

---

## ⚖️ Side-by-Side Comparison

|Operation|`Dictionary<TKey,TValue>`|`SortedDictionary<TKey,TValue>`|Why the Difference|
|---|---|---|---|
|Add|O(1) avg|O(log n)|Hash table jumps directly to a bucket; tree must find the correct sorted position by comparing.|
|Lookup (`this[key]`, `TryGetValue`, `ContainsKey`)|O(1) avg|O(log n)|Hashing skips straight to a bucket; tree needs to walk down comparing keys level by level.|
|Remove|O(1) avg|O(log n)|Same reasoning as lookup — hash table doesn't need to search a structure, tree does.|
|Enumeration order|**Unspecified/insertion-ish, not guaranteed**|**Always ascending by key**|Hash table buckets are ordered by hash value, not by key; tree in-order traversal is inherently sorted.|
|Get Min/Max key|O(n) (no shortcut)|O(log n)|Hash table has no concept of "smallest" — must scan all; tree just follows leftmost/rightmost pointers.|
|Memory overhead|Lower|Higher|Tree nodes need extra pointers (left, right, parent, color bit) per entry; hash table just needs bucket + next-pointer for collisions.|

---

## 🎯 Quick Cheat Sheet

### `Dictionary<TKey,TValue>` — choose when:

- You need the **fastest possible** lookup/insert/delete → **O(1) average**
- You **don't care about order**

### `SortedDictionary<TKey,TValue>` — choose when:

- You need keys to **always stay sorted** automatically
- You need fast **Min/Max/range queries** → O(log n) instead of O(n)
- You're okay trading O(1) → O(log n) for that ordering guarantee

---

## 💡 Why Hashing Beats Tree Traversal (in plain English)

Think of `Dictionary` like a library where every book's call number tells you **exactly which shelf** to walk to (`hash(key) → bucket index`) — one calculation, done.

Think of `SortedDictionary` like a library organized alphabetically where you have to keep **comparing and narrowing down** ("is it before or after M?") level by level until you find the shelf — that repeated comparing is the `log n` cost. You get perfectly sorted shelves in exchange for that extra searching work.