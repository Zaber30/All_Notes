# C# `HashSet<T>` — All Methods, Examples, Time Complexity, and Reasoning

---

## 🔑 The Core Idea to Remember

`HashSet<T>` is backed by the **same hash-table structure as `Dictionary<TKey,TValue>`** — just storing only keys, no values. Every element's `GetHashCode()` determines which bucket it lands in, so membership checks, adds, and removes can jump **directly** to the right bucket instead of scanning.

|Concept|Why it matters|
|---|---|
|Elements are stored by hash bucket, not by position|No indexing (`set[i]` doesn't exist) — you can only check membership, not "get the 3rd item"|
|No duplicates allowed by definition|`Add()` internally does a hash lookup first to check if the value already exists|
|No guaranteed order|Enumeration order depends on hash values and internal bucket layout, not insertion order|

---

## 📋 Full Method Table

```csharp
var set = new HashSet<int>();
```

|Method / Operation|Example|Time Complexity|Reason|
|---|---|---|---|
|`Add(item)`|`set.Add(5);`|**O(1)** average, O(n) worst case|Hashes the item to find its bucket, checks for duplicates in that bucket, then inserts if not found. Worst case only on heavy hash collisions or an internal resize (amortized, so rare).|
|`Contains(item)`|`set.Contains(5);`|**O(1)** average|Hashes the item and jumps directly to its bucket, then scans a short chain (ideally length 1) for a match — no full scan needed.|
|`Remove(item)`|`set.Remove(5);`|**O(1)** average|Same hash-bucket lookup as `Contains`, then unlinks the entry — no shifting required since it's not array-backed like `List<T>`.|
|`RemoveWhere(predicate)`|`set.RemoveWhere(x => x > 10);`|**O(n)**|Must check every element against the predicate once — no shortcut, since matches could be anywhere.|
|`Clear()`|`set.Clear();`|**O(n)**|Must clear every bucket slot so the GC can reclaim any referenced objects.|
|`Count`|`int n = set.Count;`|**O(1)**|Maintained as a running field, not computed by counting elements.|
|`foreach` iteration|`foreach (var x in set)`|**O(n)**|Must walk every bucket slot once to visit all elements — order is not guaranteed.|
|`CopyTo(array)`|`set.CopyTo(arr);`|**O(n)**|Must copy every element into the target array, one at a time.|
|`UnionWith(other)`|`set.UnionWith(other);`|**O(m)** (m = size of `other`)|Adds each element of `other` via the same O(1) average `Add` logic — total cost proportional to the size of the incoming set.|
|`IntersectWith(other)`|`set.IntersectWith(other);`|**O(n + m)**|Typically builds/uses a hash-based check against `other`, then removes anything from `set` not found in it — proportional to both set sizes.|
|`ExceptWith(other)`|`set.ExceptWith(other);`|**O(m)**|Iterates `other` and removes each matching item from `set` via O(1) average `Remove` — cost proportional to size of `other`, not `set`.|
|`SymmetricExceptWith(other)`|`set.SymmetricExceptWith(other);`|**O(n + m)**|Must compare both sets to figure out "in one but not both" — effectively touches every element in each set once.|
|`IsSubsetOf(other)`|`set.IsSubsetOf(other);`|**O(n)** (n = size of `set`)|Checks each element of `set` for membership in `other` — each check is O(1) average via hashing, so total is linear in `set`'s size.|
|`IsSupersetOf(other)`|`set.IsSupersetOf(other);`|**O(m)** (m = size of `other`)|Checks each element of `other` for membership in `set` — same reasoning as `IsSubsetOf`, mirrored.|
|`IsProperSubsetOf(other)`|`set.IsProperSubsetOf(other);`|**O(n + m)**|Does the subset check plus a size comparison to confirm the sets aren't equal — needs to know both sizes.|
|`IsProperSupersetOf(other)`|`set.IsProperSupersetOf(other);`|**O(n + m)**|Same as above, mirrored — superset check plus strict size comparison.|
|`Overlaps(other)`|`set.Overlaps(other);`|**O(m)** worst case, stops early on first match|Iterates `other` checking membership in `set`; can return `true` the moment one shared element is found.|
|`SetEquals(other)`|`set.SetEquals(other);`|**O(n + m)**|Must confirm both sets have the same count and that every element of one exists in the other.|
|`TryGetValue(equalValue, out actualValue)`|`set.TryGetValue(5, out int v);`|**O(1)** average|Same hash-bucket lookup as `Contains`, but also retrieves the actual stored instance (useful when equality ignores some fields but you want the original object).|
|`EnsureCapacity(n)`|`set.EnsureCapacity(100);`|**O(n)** if it triggers a resize|Pre-allocates the bucket array to fit the target size, which requires rehashing all existing elements into the new array.|
|`TrimExcess()`|`set.TrimExcess();`|**O(n)**|Shrinks internal arrays to fit the current count — requires rehashing/copying elements into the smaller array.|
|`GetEnumerator()`|(used by `foreach`)|**O(1)** to create, O(n) to fully consume|Creating the enumerator is instant; walking it to the end visits every element once.|

---

## 🎯 Quick Cheat Sheet (Grouped by Speed)

### ⚡ O(1) average — the whole point of `HashSet<T>`

- `Add`, `Contains`, `Remove`, `TryGetValue`
- `Count`

### 🐢 O(n) or O(n + m) — anything touching the whole set / both sets

- `RemoveWhere`, `Clear`, `CopyTo`, `foreach`
- `UnionWith`, `IntersectWith`, `ExceptWith`, `SymmetricExceptWith`
- `IsSubsetOf`, `IsSupersetOf`, `IsProperSubsetOf`, `IsProperSupersetOf`, `SetEquals`
- `Overlaps` (worst case, though it short-circuits on first match)

---

## 💡 Why `HashSet<T>` Beats `List<T>` for Membership Checks

```csharp
// List<T>.Contains — O(n): must check every item one by one
var list = new List<int> { 1, 2, 3, /* ...thousands more... */ };
bool found = list.Contains(9999);   // walks the whole list in the worst case

// HashSet<T>.Contains — O(1) average: jumps straight to the bucket
var set = new HashSet<int> { 1, 2, 3, /* ...thousands more... */ };
bool found2 = set.Contains(9999);   // one hash calculation + tiny bucket scan
```

Think of `List<T>.Contains` like searching for a name by reading through an unsorted stack of papers one by one. `HashSet<T>.Contains` is like having a labeled filing cabinet — the item's hash code tells you **exactly which drawer** to open, so you skip straight there instead of searching page by page.

---

## ⚖️ `HashSet<T>` vs `List<T>` vs `SortedSet<T>`

| Need                                                   | Best Choice    | Why                                                                                              |
| ------------------------------------------------------ | -------------- | ------------------------------------------------------------------------------------------------ |
| Fast membership checks, no duplicates, no order needed | `HashSet<T>`   | O(1) average via hashing                                                                         |
| Order matters (insertion order or indexing)            | `List<T>`      | Preserves position, supports index access                                                        |
| Need duplicates removed **and** kept sorted            | `SortedSet<T>` | O(log n) via red-black tree, always sorted — same tradeoff as `SortedDictionary` vs `Dictionary` |