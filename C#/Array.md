# C# `Array` — All Methods, Examples, Time Complexity, and Reasoning

---

## 🔑 The Core Idea to Remember

An array is the **most primitive collection** — a single fixed-size, contiguous block of memory. Everything about its performance comes from two facts:

|Fact|Why it matters|
|---|---|
|**Fixed size, set at creation**|You can never "add" or "remove" — resizing means creating a whole new array and copying everything (O(n))|
|**Contiguous memory + known element size**|Any index can be reached instantly via `base_address + (index * element_size)` — no searching needed|

Because of this, arrays are either **the fastest possible option** (direct indexing) or **the most expensive option** (any operation that implies resizing), with nothing in between.

---

## 📋 Full Method / Operation Table

```csharp
int[] arr = new int[10];
```

|Method / Operation|Example|Time Complexity|Reason|
|---|---|---|---|
|`arr[i]` (get/set)|`int x = arr[3]; arr[3] = 9;`|**O(1)**|Direct memory offset calculation — no traversal needed. This is the entire reason arrays exist.|
|`Length`|`int n = arr.Length;`|**O(1)**|Stored as a fixed field set at creation time — never recomputed.|
|`Array.IndexOf(arr, item)`|`Array.IndexOf(arr, 5);`|**O(n)**|No ordering/hashing info to exploit — must linearly scan from the start until found.|
|`Array.LastIndexOf(arr, item)`|`Array.LastIndexOf(arr, 5);`|**O(n)**|Same linear scan, just starting from the end.|
|`Array.Find(arr, predicate)`|`Array.Find(arr, x => x > 5);`|**O(n)**|Must test the predicate against elements in order until a match is found (or the array ends).|
|`Array.FindAll(arr, predicate)`|`Array.FindAll(arr, x => x > 5);`|**O(n)**|Must check every single element against the predicate — no early exit possible since all matches are needed.|
|`Array.FindIndex(arr, predicate)`|`Array.FindIndex(arr, x => x > 5);`|**O(n)**|Same as `Find`, just returns the index instead of the value.|
|`Array.Exists(arr, predicate)`|`Array.Exists(arr, x => x > 5);`|**O(n)** worst case|Stops as soon as a match is found, but worst case (no match) checks every element.|
|`Array.TrueForAll(arr, predicate)`|`Array.TrueForAll(arr, x => x > 0);`|**O(n)** worst case|Stops at the first failure, but worst case (all pass) checks every element.|
|`Array.Sort(arr)`|`Array.Sort(arr);`|**O(n log n)** average|Uses **Introspective Sort** (QuickSort + HeapSort + InsertionSort hybrid) — same algorithm `List<T>.Sort()` uses under the hood.|
|`Array.Sort(arr, comparer)`|`Array.Sort(arr, Comparer<int>.Create((a,b) => b - a));`|**O(n log n)**|Same sorting algorithm, just using a custom comparison function at each comparison step.|
|`Array.BinarySearch(arr, item)`|`Array.BinarySearch(arr, 5);`|**O(log n)**|⚠️ Only correct if the array is **already sorted** — halves the search range at each step instead of scanning linearly.|
|`Array.Reverse(arr)`|`Array.Reverse(arr);`|**O(n)**|Swaps elements from both ends moving toward the middle — touches each element once.|
|`Array.Clear(arr, start, len)`|`Array.Clear(arr, 0, arr.Length);`|**O(n)**|Must reset every targeted slot to its default value one at a time.|
|`Array.Copy(src, dst, len)`|`Array.Copy(arr, newArr, 5);`|**O(n)**|Must copy each element individually into the destination memory block.|
|`Array.CopyTo(dst, index)`|`arr.CopyTo(newArr, 0);`|**O(n)**|Same as `Array.Copy` — element-by-element copy into the target array.|
|`Array.ConstrainedCopy(...)`|`Array.ConstrainedCopy(arr, 0, dst, 0, 5);`|**O(n)**|Same copying cost as `Array.Copy`, with added rollback guarantees if it fails partway (the extra safety doesn't change the big-O).|
|`Array.Clone()`|`int[] copy = (int[])arr.Clone();`|**O(n)**|Allocates a brand-new array and copies every element into it (shallow copy).|
|`Array.Resize(ref arr, newSize)`|`Array.Resize(ref arr, 20);`|**O(n)**|Since arrays are fixed-size, this secretly **creates an entirely new array** and copies all old elements into it — nothing is "resized" in place.|
|`Array.CreateInstance(type, len)`|`Array.CreateInstance(typeof(int), 5);`|**O(n)**|Allocates and zero-initializes the requested number of slots.|
|`Array.Fill(arr, value)`|`Array.Fill(arr, 0);`|**O(n)**|Must write the value into every targeted slot individually.|
|`Array.Reverse(arr, index, len)`|`Array.Reverse(arr, 2, 4);`|**O(k)** (k = len)|Same two-pointer swap approach as full `Reverse`, just scoped to the specified range.|
|`Array.AsReadOnly(arr)`|`var ro = Array.AsReadOnly(arr);`|**O(1)**|Just wraps the existing array reference in a read-only view — no copying involved.|
|`Array.ConvertAll(arr, converter)`|`int[] doubled = Array.ConvertAll(arr, x => x * 2);`|**O(n)**|Allocates a new array and applies the converter function to every element once.|
|`Array.ForEach(arr, action)`|`Array.ForEach(arr, x => Console.WriteLine(x));`|**O(n)**|Must invoke the action delegate once per element — no way to skip any.|
|`Array.TrueForAll` _(listed above)_|—|O(n) worst case|—|
|`Array.Equals` (reference equality, inherited from `object`)|`arr1.Equals(arr2);`|**O(1)**|Compares object references only, not contents — this is a common gotcha since it does **not** compare elements.|
|`Array.GetLength(dimension)`|`arr.GetLength(0);`|**O(1)**|Stored per-dimension metadata, read directly — relevant mainly for multi-dimensional arrays.|
|`Array.Rank`|`int dims = arr.Rank;`|**O(1)**|Fixed metadata set at creation (1 for a normal array, 2+ for multi-dimensional).|
|`Array.GetValue(index)` / `SetValue(index, val)`|`arr.GetValue(3); arr.SetValue(9, 3);`|**O(1)**|Same direct offset calculation as `arr[i]`, just via the non-generic `Array` base API (slightly slower in practice due to boxing/reflection overhead, but still O(1)).|
|`foreach` iteration|`foreach (var x in arr)`|**O(n)**|Must visit every element exactly once.|
|`GetEnumerator()`|(used by `foreach`)|**O(1)** to create, O(n) to fully consume|Creating the enumerator is instant; walking through it is linear.|

---

## 🎯 Quick Cheat Sheet (Grouped by Speed)

### ⚡ O(1) — Fastest

- `arr[i]` get/set, `Length`, `GetLength`, `Rank`
- `AsReadOnly` (just a wrapper, no copy)
- `Equals` (but only compares references — be careful!)

### 🚶 O(log n) — Fast (only if sorted)

- `BinarySearch`

### 🐢 O(n) — Linear (touches every element)

- `IndexOf`, `LastIndexOf`, `Find`, `FindAll`, `FindIndex`, `Exists`, `TrueForAll`
- `Reverse`, `Clear`, `Copy`, `CopyTo`, `Clone`, `Fill`, `ConvertAll`, `ForEach`
- `Resize` (secretly allocates a whole new array!)
- `foreach`

### 🐌 O(n log n) — Slowest common one

- `Sort`

---

## 💡 The Biggest Gotcha: `Array.Resize` Is Not Really "Resizing"

```csharp
int[] arr = { 1, 2, 3 };
Array.Resize(ref arr, 5);
```

Since a plain array is a **fixed-size block of memory**, there is no way to actually grow it in place. Under the hood, `Array.Resize`:

1. Allocates a **brand new array** of the requested size.
2. **Copies every old element** into it — O(n).
3. Reassigns your reference variable to point at the new array.

This is exactly the same cost as `List<T>`'s internal doubling when it runs out of capacity — the difference is `List<T>` does this automatically and hides it behind an "amortized O(1) `Add`," while with a raw array **you pay the full O(n) cost every single time you call `Resize`**, since there's no internal "extra capacity" buffer like `List<T>` keeps.

---

## ⚖️ Array vs `List<T>` — When to Use Which

| Need                                                       | Best Choice                     | Why                                                                            |
| ---------------------------------------------------------- | ------------------------------- | ------------------------------------------------------------------------------ |
| Fixed, known size, max raw performance                     | `Array`                         | No overhead from resizing logic, slightly faster direct access                 |
| Size will grow/shrink dynamically                          | `List<T>`                       | Handles resizing internally with amortized O(1) `Add`                          |
| Multi-dimensional data (grids, matrices)                   | `Array` (`int[,]` or `int[,,]`) | Built-in multi-dimensional support; `List<T>` has no direct equivalent         |
| Need rich search/filter/sort helpers with less boilerplate | `List<T>`                       | Same underlying complexity as `Array`'s static methods, but instance-based API |