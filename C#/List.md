# C# `List<T>` Cheat Sheet (Most Popular Methods)

## Create

```
List<int> list = new();
List<int> list = new List<int>();
List<int> list = new() { 1, 2, 3 };
```

---

# Add

|Method|Description|
|---|---|
|`Add(item)`|Add one element|
|`AddRange(collection)`|Add multiple elements|
|`Insert(index, item)`|Insert at index|
|`InsertRange(index, collection)`|Insert multiple at index|

```
list.Add(5);
list.AddRange(new[] { 6, 7 });
list.Insert(0, 100);
```

---

# Remove

|Method|Description|
|---|---|
|`Remove(item)`|Remove first occurrence|
|`RemoveAt(index)`|Remove by index|
|`RemoveRange(index, count)`|Remove multiple|
|`RemoveAll(predicate)`|Remove matching elements|
|`Clear()`|Remove everything|

```
list.Remove(5);
list.RemoveAt(0);
list.RemoveRange(1, 3);
list.RemoveAll(x => x % 2 == 0);
list.Clear();
```

---

# Access

```
list[0]
list[^1]              // Last element (C# 8+)
list.First()
list.Last()
```

Properties

```
list.Count
list.Capacity
```

---

# Search

|Method|Description|
|---|---|
|`Contains(x)`|Exists?|
|`IndexOf(x)`|First index|
|`LastIndexOf(x)`|Last index|
|`Find(predicate)`|First matching value|
|`FindIndex(predicate)`|First matching index|
|`FindLast(predicate)`|Last matching value|
|`FindLastIndex(predicate)`|Last matching index|
|`Exists(predicate)`|Any match?|

```
list.Contains(5);
list.IndexOf(5);
list.Find(x => x > 10);
```

---

# Sort & Reverse

```
list.Sort();
list.Sort((a, b) => b.CompareTo(a)); // Descending
list.Reverse();
```

---

# Copy

```
var copy = list.ToList();
var arr = list.ToArray();
```

---

# Convert

```
string s = string.Join(" ", list);

int[] arr = list.ToArray();

List<int> list2 = arr.ToList();
```

# C# `List<T>` — Time Complexity of All Common Methods

`List<T>` in C# is backed by a **dynamic array** (internally a resizable `T[]`). Almost every time-complexity fact below follows directly from that one implementation detail.

---

## 🔑 The Core Idea to Remember

|Concept|Why it matters|
|---|---|
|`List<T>` stores elements in a contiguous array|Index access is instant (like a normal array)|
|When the array is full, .NET allocates a **new array (usually 2x size)** and copies everything over|This is why `Add()` is _usually_ O(1), but _occasionally_ O(n)|
|Inserting/removing in the **middle or beginning** requires shifting elements|This is why `Insert()`/`Remove()` are O(n)|
|Inserting/removing at the **end** requires no shifting|This is why `Add()`/`RemoveAt(last)` are O(1)|

---

## 📋 Full Method Table

|Method / Operation|Time Complexity|Reason|
|---|---|---|
|`list[i]` (index get/set)|**O(1)**|Array supports direct memory offset calculation (`base + i*size`).|
|`Add(item)`|**O(1)** amortized|Appends at the end. If capacity is full, array doubles (O(n) that one time), but averaged over many adds, cost per add is O(1).|
|`AddRange(collection)`|**O(k)** (k = items added)|Each item added like `Add()`; may trigger one resize.|
|`Insert(index, item)`|**O(n)**|All elements after `index` must shift right by one to make space.|
|`InsertRange(index, collection)`|**O(n + k)**|Shift existing elements + insert k new ones.|
|`RemoveAt(index)`|**O(n)** (O(1) if last index)|Elements after `index` shift left to fill the gap.|
|`Remove(item)`|**O(n)**|First does a linear search (`IndexOf`) to find the item — O(n) — then shifts elements — O(n).|
|`RemoveRange(index, count)`|**O(n)**|Shifts remaining elements left after removing the range.|
|`RemoveAll(predicate)`|**O(n)**|Must scan every element once to test the predicate.|
|`Contains(item)`|**O(n)**|No sorting/hashing — must linearly scan until found or end reached.|
|`IndexOf(item)` / `LastIndexOf(item)`|**O(n)**|Linear scan from front (or back).|
|`Find(predicate)` / `FindLast(predicate)`|**O(n)**|Linear scan testing the predicate.|
|`FindAll(predicate)`|**O(n)**|Must check every element; returns a new list of matches.|
|`FindIndex(predicate)`|**O(n)**|Same linear scan, just returns index instead of item.|
|`Exists(predicate)`|**O(n)**|Internally same as `Find` but stops early on first match (still worst-case O(n)).|
|`Sort()`|**O(n log n)**|Uses **Introspective Sort** (hybrid of QuickSort + HeapSort + InsertionSort).|
|`Sort(Comparison<T>)`|**O(n log n)**|Same algorithm, custom comparer.|
|`BinarySearch(item)`|**O(log n)**|⚠️ Only valid/correct if the list is **already sorted** — halves the search space each step.|
|`Reverse()`|**O(n)**|Must swap every element from both ends toward the middle.|
|`Clear()`|**O(n)**|Needs to clear references (so GC can collect objects) for each slot, even though count resets to 0.|
|`CopyTo(array)`|**O(n)**|Must copy every element into the target array.|
|`ToArray()`|**O(n)**|Allocates new array and copies all elements.|
|`GetRange(index, count)`|**O(k)** (k = count)|Copies only the requested sub-range.|
|`TrimExcess()`|**O(n)**|If shrinking, allocates smaller array and copies existing elements.|
|`Count` (property)|**O(1)**|Stored as a maintained field, not computed by counting.|
|`Capacity` (get/set)|**O(1)** get / **O(n)** set|Getting is instant; setting triggers a resize+copy if it changes the backing array size.|
|`foreach` iteration|**O(n)**|Must visit every element once.|

---

## 🧠 Why `Add()` is "amortized O(1)" — Explained Simply

Imagine your `List<T>` has capacity for 4 items and it's full.

1. You call `Add()` a 5th time.
2. .NET creates a **new array of double size** (capacity 8).
3. It **copies all 4 old elements** into the new array — this single operation is O(n).
4. Then it adds your new item.

This resize doesn't happen every time — only when capacity runs out. Because doubling means resizes become rarer and rarer as the list grows, if you **average the cost of resizing across many `Add()` calls**, each `Add()` still comes out to O(1) on average. This is called **amortized time complexity**.

---

## 🎯 Quick Cheat Sheet (Grouped by Speed)

### ⚡ O(1) — Fastest

- Index access (`list[i]`)
- `Add()` (amortized)
- `Count`
- `Capacity` (get)

### 🚶 O(log n) — Fast (only if sorted)

- `BinarySearch()`

### 🐢 O(n) — Linear (needs to touch every element)

- `Insert()`, `RemoveAt()`, `Remove()`
- `Contains()`, `IndexOf()`, `Find()`, `FindAll()`, `Exists()`
- `Reverse()`, `Clear()`, `ToArray()`, `CopyTo()`
- `foreach` loop

### 🐌 O(n log n) — Slowest common one

- `Sort()`

---

## 💡 Practical Tip

| If you need to...                                 | Use...                           | Because                          |
| ------------------------------------------------- | -------------------------------- | -------------------------------- |
| Frequently add/remove at the **end**              | `List<T>`                        | O(1) amortized                   |
| Frequently add/remove at the **beginning/middle** | `LinkedList<T>`                  | O(1) for known node, no shifting |
| Frequently **search by key**                      | `Dictionary<TKey,TValue>`        | O(1) average lookup via hashing  |
| Need **sorted order maintained** automatically    | `SortedList<T>` / `SortedSet<T>` | O(log n) insert/search           |