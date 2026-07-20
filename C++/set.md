# `std::set` — The Complete Reference Guide (C++)

> A single-file, exhaustive reference to `std::set`: internals, every constructor, every member function, every algorithm interaction, complexities, pitfalls, and interview-ready examples.

---

## Table of Contents

1. [Introduction](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#1-introduction)
2. [All Types of Set](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#2-all-types-of-set)
3. [All Constructors](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#3-all-constructors)
4. [All Iterator Functions](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#4-all-iterator-functions)
5. [All Capacity Functions](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#5-all-capacity-functions)
6. [All Modifier Functions](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#6-all-modifier-functions)
7. [All Lookup Functions](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#7-all-lookup-functions)
8. [Observers](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#8-observers)
9. [Traversal Methods](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#9-traversal-methods)
10. [Comparison Operators](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#10-comparison-operators)
11. [Common STL Algorithms Used with Set](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#11-common-stl-algorithms-used-with-set)
12. [Complete Time Complexity Table](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#12-complete-time-complexity-table)
13. [Interview Notes](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#13-interview-notes)
14. [Common Mistakes](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#14-common-mistakes)
15. [Practice Examples](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#15-practice-examples)
16. [Coding Interview Questions](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#16-coding-interview-questions)

---

## 1. Introduction

### 1.1 What is `std::set`?

`std::set` is an **associative container** defined in `<set>` that stores a collection of **unique**, **automatically sorted** elements. It is part of the C++ Standard Template Library (STL).

```cpp
#include <set>
#include <iostream>

int main() {
    std::set<int> s = {5, 3, 8, 1, 3, 5};
    for (int x : s) std::cout << x << " ";
    // Output: 1 3 5 8   (duplicates removed, sorted ascending)
}
```

**Key defining properties:**

|Property|Description|
|---|---|
|Uniqueness|No two elements compare equal|
|Sorted order|Elements are always kept in sorted order (by default ascending, using `std::less`)|
|Immutable elements|Once inserted, an element's _value_ cannot be modified in place (would break ordering) — you must erase + insert|
|Node-based|Internally a balanced binary search tree (Red-Black Tree in virtually every implementation: libstdc++, libc++, MSVC STL)|
|No random access|No `operator[]`, no index-based access — only iterator-based traversal|

### 1.2 Features

- **Automatic sorting** on insertion — no need to call `sort()`.
- **Uniqueness enforced** — inserting a duplicate is a silent no-op (returns `false`/end iterator via the pair result).
- **Logarithmic operations** — search, insert, erase are all `O(log n)`.
- **Bidirectional iterators** — you can traverse forward and backward, but not jump (`it + 5` doesn't compile; use `std::advance`/`std::next`).
- **Stable memory addresses** — inserting/erasing one element does **not** invalidate iterators/pointers/references to other elements (huge advantage over `vector`).
- **Customizable ordering** via comparator template parameter.
- **Header:** `#include <set>`

### 1.3 Internal Implementation — Red-Black Tree

`std::set` is (in every major standard library implementation) built on a **self-balancing binary search tree**, specifically a **Red-Black Tree (RBT)**.

**Why Red-Black Tree and not a plain BST or AVL tree?**

|Tree Type|Balance Guarantee|Insert/Delete Cost|Search Cost|Used By|
|---|---|---|---|---|
|Plain BST|None (can degrade to O(n))|O(n) worst case|O(n) worst case|Nothing production-grade|
|AVL Tree|Very strict (height diff ≤ 1)|More rotations on insert/delete|Slightly faster search|Rarely used for `set`|
|Red-Black Tree|Looser (height ≤ 2·log₂(n+1))|Fewer rotations, faster mutation|O(log n), slightly looser than AVL|`std::set`, `std::map`, Java `TreeMap`, Linux kernel scheduler|

Red-Black Trees trade a _slightly_ less balanced tree (compared to AVL) for **cheaper rebalancing on insert/delete**, which matters because `set` is mutated far more often than pure lookup structures.

**Red-Black Tree invariants:**

1. Every node is colored **red** or **black**.
2. The root is always **black**.
3. Every leaf (`nil`/`nullptr`) is considered **black**.
4. A **red** node cannot have a **red** child (no two reds in a row).
5. Every path from a node to its descendant `nil` leaves contains the **same number of black nodes** (black-height property).

These 5 rules guarantee the longest root-to-leaf path is never more than **2×** the shortest, which bounds the tree height at `O(log n)` and guarantees `O(log n)` search/insert/delete.

**What happens on insert:**

1. Standard BST insert (find position by comparator, attach new node).
2. New node is colored **red**.
3. If this violates rule 4 (red-red conflict), fix via **rotations** and **recoloring** (cases: uncle red → recolor; uncle black → rotate).
4. Fixups propagate upward at most `O(log n)` times.

**What happens on erase:**

1. Standard BST delete (may need to find in-order successor if node has 2 children).
2. If a black node is removed, the tree may violate black-height property → fixed via rotations/recoloring ("double-black" fixup cases).

Because `std::set` never needs `operator[]` or random access, a tree (not a hash table or sorted array) is ideal — it gives sorted-order guarantees with cheap logarithmic mutation, which a sorted `vector` cannot (`vector` insert is `O(n)` due to shifting).

### 1.4 Time Complexities (Summary)

|Operation|Complexity|
|---|---|
|Insert|O(log n)|
|Erase|O(log n) (amortized O(1) with a valid hint iterator in C++11+)|
|Find / count / contains|O(log n)|
|lower_bound / upper_bound|O(log n)|
|Traversal (full)|O(n)|
|Copy construction|O(n)|
|Range construction (n elements)|O(n log n) generally, O(n) if already sorted with hint-based insertion internally (implementation-defined optimization)|

(Full table with every function is in [Section 12](https://claude.ai/chat/0ef69c7f-465e-4d03-bc33-fc3ca547d8f1#12-complete-time-complexity-table).)

### 1.5 Memory Layout

Unlike `std::vector` (contiguous array), `std::set` stores each element in a **separately heap-allocated tree node**. A typical node looks conceptually like:

```cpp
struct RBTreeNode {
    Key       value;     // the actual stored element
    RBTreeNode* parent;
    RBTreeNode* left;
    RBTreeNode* right;
    bool        color;   // red or black (often packed into a pointer bit or enum)
};
```

**Consequences of node-based memory layout:**

- **Poor cache locality** — traversing a `set` jumps around memory (pointer chasing), unlike a `vector`'s contiguous scan. A `set<int>` traversal is measurably slower than a sorted `vector<int>` scan for the same data, despite both being O(n).
- **Per-element overhead** — each node carries 3 pointers + a color bit + allocator bookkeeping, on top of the value itself. For a `set<int>` (4 bytes of "real" data), the overhead can be **6–10×** the payload size (roughly 32–48 bytes/node on 64-bit systems).
- **Stable addresses** — because nodes don't move, `&(*it)` stays valid until that specific element is erased. This is _not_ true for `vector`.
- **No reallocation** — inserting the 1,000,001st element never invalidates existing iterators (unlike `vector::push_back`, which may reallocate everything).

### 1.6 Advantages

- Always sorted — no manual sorting needed.
- Fast O(log n) search/insert/erase, better than `vector` (O(n) insert/erase in the middle) for frequent mutation + lookup workloads.
- Enforces uniqueness automatically.
- Stable iterators/references/pointers across insert/erase of _other_ elements.
- Ordered traversal is "free" (in-order tree walk).
- Supports range queries efficiently (`lower_bound`, `upper_bound`, `equal_range`).

### 1.7 Disadvantages

- Higher memory overhead per element vs. `vector`/`array`.
- Poor cache locality → slower in practice than a sorted `vector` + binary search for **read-heavy, write-rarely** workloads.
- No random access (`s[3]` doesn't exist).
- No direct element modification (keys are effectively `const`) — must erase and reinsert to "update" a value.
- Slower raw insertion throughput than `unordered_set` (O(log n) vs. average O(1)) when order doesn't matter.
- Slower construction from unsorted data than sorting a `vector` once and deduplicating, for pure batch use cases.

**Real-life analogy:** Think of `std::set` like a **library's card catalog sorted alphabetically** — every time you insert a new card, a librarian finds the exact sorted slot and slides it in without needing to physically reorganize every other card (like inserting into a filing cabinet with fixed alphabetical dividers), and every card stays exactly where it was filed (stable address) even as new cards are added elsewhere. Compare this to a `vector`, which is like an **unsorted stack of index cards** — fast to add to the top, but if you want sorted order you must repeatedly shift cards.

---

## 2. All Types of Set

Real-life framing for this section: imagine you're building a **student roll-number system** for a school. Each "type" of set below solves a different flavor of that problem.

### 2.1 Empty Set

```cpp
std::set<int> s1;                 // default constructed, empty
std::cout << s1.empty();          // Output: 1 (true)
```

**Note:** No memory for nodes is allocated until the first `insert`.

### 2.2 Initialized Set (list-initialization)

```cpp
std::set<int> rollNumbers = {101, 105, 102, 101, 108};
for (int r : rollNumbers) std::cout << r << " ";
// Output: 101 102 105 108
```

**Edge case:** Duplicate `101` is silently dropped — no error, no exception.

### 2.3 Copy Set

```cpp
std::set<int> original = {1, 2, 3};
std::set<int> copy(original);      // copy constructor
std::set<int> copy2 = original;    // also copy constructor (not assignment, since it's init)

copy.insert(4);
std::cout << original.size();      // Output: 3 (original untouched — deep copy)
```

**Note:** Copying a `set<int>` of `n` elements is `O(n)` — every node is freshly allocated.

### 2.4 Assignment

```cpp
std::set<int> a = {1, 2, 3};
std::set<int> b;
b = a;                 // copy assignment — deep copy, O(n)

std::set<int> c;
c = std::move(a);       // move assignment — O(1) (just steals the tree root pointer)
std::cout << a.size();  // Output: 0 (a is now valid-but-unspecified, typically empty)
```

### 2.5 `set<char>`

```cpp
std::set<char> grades = {'B', 'A', 'C', 'A'};
for (char g : grades) std::cout << g << " ";
// Output: A B C
```

Real-life: deduplicating and sorting unique grade letters awarded in a class.

### 2.6 `set<string>`

```cpp
std::set<std::string> names = {"Karim", "Ayesha", "Karim", "Zara"};
for (auto &n : names) std::cout << n << " ";
// Output: Ayesha Karim Zara   (lexicographic order)
```

Real-life: storing unique visitor usernames on a website, alphabetically for a report.

### 2.7 `set<pair<int,int>>`

```cpp
std::set<std::pair<int,int>> coords = {{2,3}, {1,5}, {1,2}};
for (auto &p : coords) std::cout << "(" << p.first << "," << p.second << ") ";
// Output: (1,2) (1,5) (2,3)
```

**Note:** Sorted lexicographically — first by `.first`, then by `.second` (default `std::pair` `operator<`). Real-life: storing unique `(row, col)` cells visited in a grid/maze traversal (BFS/DFS visited-set).

### 2.8 `set<vector<int>>`

```cpp
std::set<std::vector<int>> paths = {{1,2,3}, {1,2}, {1,2,3}};
for (auto &v : paths) {
    for (int x : v) std::cout << x << " ";
    std::cout << "| ";
}
// Output: 1 2 | 1 2 3 |
```

**Note:** `vector` also compares lexicographically; shorter prefix sorts first. Duplicate `{1,2,3}` removed. Real-life: storing unique root-to-leaf paths found in a tree during backtracking.

### 2.9 Nested Set — `set<set<int>>`

```cpp
std::set<std::set<int>> groups = {{1,2}, {2,1}, {3,4}};
std::cout << groups.size();   // Output: 2  --> {1,2} and {2,1} are the SAME set!
```

**Edge case:** `{2,1}` normalizes to `{1,2}` internally, so it's a duplicate. Real-life: storing unique unordered groupings of students for group-project deduplication.

### 2.10 Descending Set

```cpp
std::set<int, std::greater<int>> descSet = {3, 1, 4, 1, 5};
for (int x : descSet) std::cout << x << " ";
// Output: 5 4 3 1
```

Real-life: leaderboard of top scores, highest first.

### 2.11 Custom Comparator

```cpp
struct CompareLength {
    bool operator()(const std::string &a, const std::string &b) const {
        if (a.size() != b.size()) return a.size() < b.size();
        return a < b;   // tie-breaker required for strict weak ordering
    }
};

std::set<std::string, CompareLength> words = {"banana", "kiwi", "fig", "apple"};
for (auto &w : words) std::cout << w << " ";
// Output: fig kiwi apple banana   (sorted by length, then alphabetically)
```

Real-life: autocomplete suggestions sorted by word length (shorter, simpler suggestions first).

**Lambda comparator (needs `decltype` since lambdas have unique types):**

```cpp
auto cmp = [](int a, int b) { return a > b; };
std::set<int, decltype(cmp)> s(cmp);
s.insert({5, 1, 3});
// order: 5 3 1
```

### 2.12 Iterator Constructor

```cpp
std::vector<int> v = {9, 3, 3, 7, 1};
std::set<int> s(v.begin(), v.end());
for (int x : s) std::cout << x << " ";
// Output: 1 3 7 9
```

Real-life: converting a vector of scraped/logged values into a deduplicated, sorted set for analysis.

### 2.13 Dynamic Set

A "dynamic set" simply refers to using `set` in its normal mutable form — actively inserting/erasing at runtime based on program logic (as opposed to building once and never touching again).

```cpp
std::set<int> activeUserIDs;
activeUserIDs.insert(101);
activeUserIDs.insert(102);
activeUserIDs.erase(101);   // user logged out
```

Real-life: tracking currently-online user IDs in a chat server, updated continuously.

### 2.14 Constant Set

```cpp
const std::set<int> fixedIDs = {10, 20, 30};
// fixedIDs.insert(40);   // COMPILE ERROR — insert() is non-const
for (int x : fixedIDs) std::cout << x << " ";  // reading is fine
// Output: 10 20 30
```

**Note:** On a `const set`, `find()` returns a `const_iterator`, and any mutating member function is unavailable at compile time. Real-life: a fixed set of valid HTTP status codes that should never change during program execution.

---

## 3. All Constructors

`std::set` template signature:

```cpp
template<
    class Key,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<Key>
> class set;
```

|#|Constructor|Syntax|Complexity|
|---|---|---|---|
|1|Default|`set<int> s;`|O(1)|
|2|With comparator|`set<int, greater<int>> s;`|O(1)|
|3|With comparator + allocator|`set<int, greater<int>, MyAlloc<int>> s(cmp, alloc);`|O(1)|
|4|Range (iterator pair)|`set<int> s(v.begin(), v.end());`|O(n log n)|
|5|Copy|`set<int> s2(s1);`|O(n)|
|6|Move|`set<int> s2(std::move(s1));`|O(1)|
|7|Copy w/ allocator|`set<int> s2(s1, alloc);`|O(n)|
|8|Move w/ allocator|`set<int> s2(std::move(s1), alloc);`|O(1) or O(n) if allocators differ|
|9|Initializer list|`set<int> s = {3,1,2};`|O(n log n)|
|10|Initializer list + comparator|`set<int, greater<int>> s({3,1,2}, cmp);`|O(n log n)|

### Detailed Examples

**1. Default constructor**

```cpp
std::set<int> s;
```

**2. Constructor with custom comparator**

```cpp
std::set<int, std::greater<int>> s;
s.insert({1, 2, 3});
// order: 3 2 1
```

**4. Range constructor**

```cpp
int arr[] = {4, 2, 5, 2};
std::set<int> s(arr, arr + 4);
// s = {2, 4, 5}
```

**5. Copy constructor**

```cpp
std::set<int> a = {1,2,3};
std::set<int> b(a);   // deep copy
```

**6. Move constructor**

```cpp
std::set<int> a = {1,2,3};
std::set<int> b(std::move(a));  // a is now empty, O(1) transfer
```

**9. Initializer list constructor**

```cpp
std::set<std::string> s = {"c", "a", "b"};
// s = {"a", "b", "c"}
```

**Return type note:** Constructors don't "return" anything (they construct the object), but conceptually the object produced is always a valid, fully-formed `std::set<Key, Compare, Allocator>`.

**Edge case:** Constructing from a range with duplicates is safe — duplicates are simply discarded, no exception thrown.

---

## 4. All Iterator Functions

`std::set` iterators are **bidirectional** (not random-access). Since C++11, iterators for `set` are `const` in effect for the _value_ (you can't modify `*it` because that could break sort order), even though the iterator type itself is technically named `iterator` (as of C++11, `set::iterator` is defined to behave like `const_iterator` for the value).

|Function|Syntax|Returns|Complexity|
|---|---|---|---|
|`begin()`|`s.begin()`|iterator to smallest element|O(1)|
|`end()`|`s.end()`|iterator past the largest element|O(1)|
|`rbegin()`|`s.rbegin()`|reverse_iterator to largest element|O(1)|
|`rend()`|`s.rend()`|reverse_iterator before smallest element|O(1)|
|`cbegin()`|`s.cbegin()`|const_iterator to smallest|O(1)|
|`cend()`|`s.cend()`|const_iterator past largest|O(1)|
|`crbegin()`|`s.crbegin()`|const_reverse_iterator to largest|O(1)|
|`crend()`|`s.crend()`|const_reverse_iterator before smallest|O(1)|

### Examples

```cpp
std::set<int> s = {30, 10, 20};   // stored internally as {10, 20, 30}

// begin() / end()
for (auto it = s.begin(); it != s.end(); ++it)
    std::cout << *it << " ";
// Output: 10 20 30

// rbegin() / rend()
for (auto it = s.rbegin(); it != s.rend(); ++it)
    std::cout << *it << " ";
// Output: 30 20 10

// cbegin() / cend() — guarantees read-only, useful with `auto` to avoid accidental mutation
for (auto it = s.cbegin(); it != s.cend(); ++it)
    std::cout << *it << " ";
// Output: 10 20 30

// crbegin() / crend()
for (auto it = s.crbegin(); it != s.crend(); ++it)
    std::cout << *it << " ";
// Output: 30 20 10
```

**Notes:**

- `*it = 5;` → **compile error**. `set` iterators dereference to `const Key&`.
- `++it` and `--it` are O(log n) _amortized_ O(1) in practice (moving to in-order successor/predecessor in a tree), because across a full traversal the total cost sums to O(n).
- `s.begin()` on an empty set equals `s.end()`.
- Erasing the element an iterator points to invalidates _only that iterator_; all others remain valid (this is the "stable iterator" guarantee mentioned in Section 1).

**Real-life example:** printing a leaderboard both ascending and descending (e.g., "Top scorers" vs "Needs improvement" views) without maintaining two separate sorted containers — just use `rbegin()/rend()` for the reverse view.

```cpp
std::set<int> scores = {88, 95, 72, 60};
std::cout << "Ascending: ";
for (int s : scores) std::cout << s << " ";
std::cout << "\nDescending: ";
for (auto it = scores.rbegin(); it != scores.rend(); ++it) std::cout << *it << " ";
// Ascending: 60 72 88 95
// Descending: 95 88 72 60
```

---

## 5. All Capacity Functions

|Function|Syntax|Return Type|Complexity|
|---|---|---|---|
|`size()`|`s.size()`|`size_type` (unsigned integer)|O(1)|
|`empty()`|`s.empty()`|`bool`|O(1)|
|`max_size()`|`s.max_size()`|`size_type`|O(1)|

### `size()`

```cpp
std::set<int> s = {1, 2, 3};
std::cout << s.size();   // Output: 3
```

### `empty()`

```cpp
std::set<int> s;
std::cout << std::boolalpha << s.empty();  // Output: true
s.insert(1);
std::cout << std::boolalpha << s.empty();  // Output: false
```

**Best practice:** Prefer `s.empty()` over `s.size() == 0` — `empty()` is guaranteed O(1) and more expressive; `size()` is also O(1) for `set` specifically, but `empty()` is the idiomatic choice across all containers (some containers, though not `set`, have O(n) `size()`).

### `max_size()`

```cpp
std::set<int> s;
std::cout << s.max_size();
// Output: some huge implementation-defined number, e.g. 461168601842738790 (theoretical max based on address space / node size)
```

**Note:** `max_size()` is a theoretical ceiling based on `Allocator` limits and `sizeof(node)`, not a practical/actual memory limit. It will always be far larger than any real-world set you'll build; it exists mainly to check against overflow in generic code, and is rarely used in application code.

**Real-life example:** checking whether a "seen items" set is empty before starting a batch-processing loop:

```cpp
std::set<std::string> processedFiles;
if (processedFiles.empty()) {
    std::cout << "No files processed yet, starting fresh.\n";
}
```

---

## 6. All Modifier Functions

### 6.1 `insert()`

`insert()` has **6 overloads**. The most-used two:

**(a) Single value insert — returns `pair<iterator, bool>`**

```cpp
std::set<int> s;
auto result = s.insert(10);
std::cout << *result.first << " " << result.second;
// Output: 10 1   (1 = true, insertion happened)

auto result2 = s.insert(10);   // duplicate
std::cout << *result2.first << " " << result2.second;
// Output: 10 0   (0 = false, no insertion; iterator points to EXISTING element)
```

- **Parameters:** `const Key& value` or `Key&& value` (move overload)
- **Return type:** `std::pair<iterator, bool>` — iterator to the element (new or existing), bool = whether insertion actually happened
- **Complexity:** O(log n)

**(b) Insert with position hint — returns `iterator`**

```cpp
std::set<int> s = {1, 5, 10};
auto it = s.insert(s.find(5), 6);   // hint: insert near 5
// s = {1, 5, 6, 10}
```

- **Complexity:** O(1) amortized if the hint is correct (i.e., the new element goes right before the hint), otherwise O(log n).
- **Edge case:** A wrong hint doesn't cause incorrect behavior — the set still ends up correctly sorted — it just loses the O(1) speed benefit and falls back to O(log n).

**(c) Range insert**

```cpp
std::vector<int> v = {7, 2, 9};
std::set<int> s;
s.insert(v.begin(), v.end());
// s = {2, 7, 9}
```

- **Complexity:** O(k log(n+k)) for inserting k elements into a set of size n.

**(d) Initializer-list insert**

```cpp
std::set<int> s;
s.insert({4, 1, 4, 2});
// s = {1, 2, 4}
```

Real-life example: inserting unique product SKUs scanned at checkout — duplicate scans of the same barcode are automatically ignored.

```cpp
std::set<std::string> scannedSKUs;
scannedSKUs.insert("SKU123");
scannedSKUs.insert("SKU123");  // accidental double-scan, safely ignored
std::cout << scannedSKUs.size();  // Output: 1
```

### 6.2 `emplace()`

Constructs the element **in-place**, avoiding a temporary copy/move for complex types.

```cpp
std::set<std::pair<int,int>> s;
s.emplace(1, 2);          // constructs pair(1,2) directly inside the node
// vs. s.insert(std::make_pair(1, 2));  <- extra temporary object
```

- **Parameters:** `Args&&... args` — forwarded to the `Key`'s constructor
- **Return type:** `std::pair<iterator, bool>` (same as `insert`)
- **Complexity:** O(log n)

**Important subtlety:** Even with `emplace`, if the key already exists, the newly-constructed temporary is destroyed (the "in-place" construction happens against a temporary node that is discarded if it turns out to be a duplicate) — so `emplace` doesn't fully avoid construction cost when a duplicate is found, only the _extra copy on top of construction_.

```cpp
std::set<std::string> s;
s.emplace("hello");
s.emplace("hello");   // constructs a second "hello" temporarily, then discards it (dup)
std::cout << s.size();  // Output: 1
```

### 6.3 `emplace_hint()`

Same as `emplace`, but with a position hint (like `insert`'s hinted overload).

```cpp
std::set<int> s = {1, 5, 10};
auto it = s.emplace_hint(s.find(5), 6);
// s = {1, 5, 6, 10}
```

- **Return type:** `iterator` (not a pair — no bool, since hint-based emplace is "fire and forget")
- **Complexity:** O(1) amortized with correct hint, else O(log n)

### 6.4 `erase()`

**Three overloads:**

**(a) By key**

```cpp
std::set<int> s = {1, 2, 3};
size_t n = s.erase(2);   // n = 1 (number of elements removed: 0 or 1 for set)
std::cout << n;          // Output: 1
s.erase(99);              // key not found
std::cout << s.erase(99); // Output: 0
```

- **Return type:** `size_type` — count of elements removed (always 0 or 1 for `set`, since keys are unique)
- **Complexity:** O(log n)

**(b) By iterator**

```cpp
std::set<int> s = {1, 2, 3};
auto it = s.find(2);
auto next = s.erase(it);   // returns iterator to element AFTER the erased one
std::cout << *next;        // Output: 3
```

- **Return type:** `iterator` — iterator to the element following the erased one
- **Complexity:** O(1) amortized

**(c) By range**

```cpp
std::set<int> s = {1, 2, 3, 4, 5};
auto itFrom = s.find(2);
auto itTo = s.find(4);
s.erase(itFrom, itTo);   // erases [2, 4) -> removes 2 and 3
// s = {1, 4, 5}
```

- **Complexity:** O(k + log n), k = number of elements erased

**Edge case:** Calling `s.erase(s.end())` is **undefined behavior** — always check `find()` result against `end()` before erasing by iterator.

```cpp
auto it = s.find(999);
if (it != s.end()) s.erase(it);   // safe pattern
```

Real-life example: removing a user from an "active sessions" set when they log out.

```cpp
std::set<int> activeSessions = {101, 102, 103};
activeSessions.erase(102);  // user 102 logged out
```

### 6.5 `clear()`

```cpp
std::set<int> s = {1, 2, 3};
s.clear();
std::cout << s.size();   // Output: 0
```

- **Return type:** `void`
- **Complexity:** O(n) — every node must be destroyed/deallocated

### 6.6 `swap()`

```cpp
std::set<int> a = {1, 2, 3};
std::set<int> b = {9, 8};
a.swap(b);
// a = {8, 9}, b = {1, 2, 3}
```

- **Return type:** `void`
- **Complexity:** O(1) — just swaps internal tree-root pointers, no element-by-element work
- **Note:** All iterators/references remain valid but now "belong" to the swapped-into container.

Also available as a free function: `std::swap(a, b);` (equivalent, calls the member `swap`).

### 6.7 `extract()` — C++17

Removes an element **without destroying it**, returning a movable "node handle" (`node_type`). Useful for changing a key's value (impossible via a normal iterator) or transferring a node between containers without reallocation.

```cpp
std::set<int> s = {1, 2, 3};
auto node = s.extract(2);      // node now holds the value "2", removed from s
std::cout << s.size();          // Output: 2
node.value() = 20;               // legal! node handles allow mutation
s.insert(std::move(node));      // re-insert with new value
// s = {1, 3, 20}
```

- **Overloads:** `extract(const Key&)` and `extract(const_iterator)`
- **Return type:** `node_type` (empty/"null" node handle if key not found — check with `node.empty()`)
- **Complexity:** O(log n)
- **Key benefit:** No allocation/deallocation — the actual node memory is reused, making this **the only standard-sanctioned way to modify a set element's value in place**.

### 6.8 `merge()` — C++17

Transfers all elements from one `set` into another **without copying/reallocating nodes**. Elements whose keys already exist in the destination are left untouched in the source.

```cpp
std::set<int> a = {1, 2, 3};
std::set<int> b = {3, 4, 5};
a.merge(b);
// a = {1, 2, 3, 4, 5}
// b = {3}   <- "3" stayed in b because a already had "3"
```

- **Parameter:** another `set` (or `multiset`) with a compatible comparator, by lvalue or rvalue reference
- **Return type:** `void`
- **Complexity:** O(n log(size(a) + size(b))) roughly — implementation-defined but node-transfer avoids reallocation cost per element.
- **Real-life example:** merging two departments' unique employee-ID sets during a company merger, keeping whichever record already exists in the primary set on conflict.

---

## 7. All Lookup Functions

|Function|Since|Return Type|Complexity|
|---|---|---|---|
|`find()`|C++98|`iterator`|O(log n)|
|`count()`|C++98|`size_type` (0 or 1)|O(log n)|
|`contains()`|C++20|`bool`|O(log n)|
|`lower_bound()`|C++98|`iterator`|O(log n)|
|`upper_bound()`|C++98|`iterator`|O(log n)|
|`equal_range()`|C++98|`pair<iterator, iterator>`|O(log n)|

### 7.1 `find()`

```cpp
std::set<int> s = {10, 20, 30};
auto it = s.find(20);
if (it != s.end()) std::cout << "Found: " << *it;   // Output: Found: 20

auto it2 = s.find(99);
std::cout << (it2 == s.end());   // Output: 1 (true, not found)
```

**Edge case:** Always compare against `s.end()`; dereferencing a not-found iterator is UB.

### 7.2 `count()`

```cpp
std::set<int> s = {10, 20, 30};
std::cout << s.count(20);   // Output: 1
std::cout << s.count(99);   // Output: 0
```

**Note:** For `std::set` (unlike `std::multiset`), `count()` only ever returns `0` or `1`, since keys are unique. Prefer `find() != end()` or `contains()` for clarity/performance when you only need existence, not count.

### 7.3 `contains()` — C++20

```cpp
std::set<int> s = {10, 20, 30};
if (s.contains(20)) std::cout << "Yes, 20 is present";
// Output: Yes, 20 is present
```

**Note:** Cleanest, most readable way to check membership since C++20. Before C++20, the idiomatic pattern was `s.find(x) != s.end()` or `s.count(x) > 0`.

### 7.4 `lower_bound()`

Returns an iterator to the **first element not less than** (i.e., `>=`) the given key.

```cpp
std::set<int> s = {10, 20, 30, 40};
auto it = s.lower_bound(25);
std::cout << *it;   // Output: 30  (first element >= 25)

auto it2 = s.lower_bound(20);
std::cout << *it2;  // Output: 20  (20 itself, since 20 >= 20)
```

### 7.5 `upper_bound()`

Returns an iterator to the **first element strictly greater than** the given key.

```cpp
std::set<int> s = {10, 20, 30, 40};
auto it = s.upper_bound(20);
std::cout << *it;   // Output: 30  (first element > 20)
```

### 7.6 `equal_range()`

Returns `{lower_bound(key), upper_bound(key)}` as a pair — the range of elements matching `key` (for `set`, this range has at most 1 element, since keys are unique).

```cpp
std::set<int> s = {10, 20, 30};
auto range = s.equal_range(20);
for (auto it = range.first; it != range.second; ++it)
    std::cout << *it << " ";
// Output: 20
```

### Real-Life Combined Example: Range Query

Find all order IDs between 1000 and 2000 (inclusive) in a set of order IDs:

```cpp
std::set<int> orderIDs = {500, 1000, 1500, 1999, 2000, 2500};
auto from = orderIDs.lower_bound(1000);  // first >= 1000
auto to = orderIDs.upper_bound(2000);    // first > 2000

for (auto it = from; it != to; ++it) std::cout << *it << " ";
// Output: 1000 1500 1999 2000
```

**Edge case:** If `key` is larger than every element, `lower_bound`/`upper_bound` both return `s.end()`. If `key` is smaller than every element, both return `s.begin()`.

---

## 8. Observers

### 8.1 `key_comp()`

Returns a copy of the container's comparison function (the `Compare` template parameter instance).

```cpp
std::set<int, std::greater<int>> s = {1, 2, 3};
auto cmp = s.key_comp();
std::cout << cmp(5, 3);   // Output: 1 (true) — because with greater<int>, 5 "comes before" 3
```

- **Return type:** `Compare` (e.g., `std::less<Key>` by default)
- **Complexity:** O(1)

### 8.2 `value_comp()`

For `std::set`, `value_comp()` behaves identically to `key_comp()` because the "value" and the "key" are the same thing (unlike `std::map`, where key and value differ, and `value_comp` wraps the comparator to compare only the `.first` of each `pair`).

```cpp
std::set<int> s = {1, 2, 3};
auto vcmp = s.value_comp();
std::cout << vcmp(1, 2);   // Output: 1 (true, 1 < 2)
```

- **Return type:** `Compare` (an unspecified type derived from `Compare` for `set`; for `map` it's `std::map::value_compare`)
- **Complexity:** O(1)

**Real-life use:** Passing `s.key_comp()` into an algorithm like `std::is_sorted` or a custom merge routine so it respects the same ordering rules as the set itself, without hardcoding `<` or `>`.

```cpp
std::set<int, std::greater<int>> s = {5, 1, 3};
std::vector<int> v(s.begin(), s.end());
std::cout << std::is_sorted(v.begin(), v.end(), s.key_comp());  // Output: 1 (true)
```

---

## 9. Traversal Methods

### 9.1 Range-based for loop (most idiomatic, C++11+)

```cpp
std::set<int> s = {3, 1, 2};
for (int x : s) std::cout << x << " ";
// Output: 1 2 3
```

### 9.2 Iterator-based loop

```cpp
std::set<int> s = {3, 1, 2};
for (auto it = s.begin(); it != s.end(); ++it)
    std::cout << *it << " ";
// Output: 1 2 3
```

**Use when:** you need the iterator itself (e.g., to erase, or to pass to another algorithm as a position).

### 9.3 Reverse iterator loop

```cpp
std::set<int> s = {3, 1, 2};
for (auto it = s.rbegin(); it != s.rend(); ++it)
    std::cout << *it << " ";
// Output: 3 2 1
```

### 9.4 Const iterator loop

```cpp
const std::set<int> s = {3, 1, 2};
for (auto it = s.cbegin(); it != s.cend(); ++it)
    std::cout << *it << " ";
// Output: 1 2 3
```

**Use when:** documenting intent that the loop body must never mutate the container (self-documenting code, avoids accidental misuse in large loop bodies).

**Real-life example — printing a sorted attendance sheet both ways:**

```cpp
std::set<std::string> attendees = {"Rafi", "Anika", "Mehedi"};
std::cout << "Check-in order (alphabetical): ";
for (const auto &name : attendees) std::cout << name << ", ";

std::cout << "\nCheck-out priority (reverse-alphabetical): ";
for (auto it = attendees.rbegin(); it != attendees.rend(); ++it) std::cout << *it << ", ";
```

---

## 10. Comparison Operators

`std::set` supports **all six** relational operators (lexicographical comparison of contents, element by element, since C++11; since C++20 `<=>` synthesizes them via `operator==` and `operator<=>` on `Key` — but `==`/`<`/etc. are all still available and behave the same from the caller's perspective).

|Operator|Meaning|Complexity|
|---|---|---|
|`==`|Same size, same elements in same order|O(n)|
|`!=`|Not equal|O(n)|
|`<`|Lexicographically less|O(n)|
|`>`|Lexicographically greater|O(n)|
|`<=`|Lexicographically less-or-equal|O(n)|
|`>=`|Lexicographically greater-or-equal|O(n)|

```cpp
std::set<int> a = {1, 2, 3};
std::set<int> b = {1, 2, 3};
std::set<int> c = {1, 2, 4};

std::cout << (a == b) << "\n";  // Output: 1 (true, identical contents)
std::cout << (a != c) << "\n";  // Output: 1 (true)
std::cout << (a < c) << "\n";   // Output: 1 (true — element-wise: 1==1, 2==2, 3<4)
std::cout << (c > a) << "\n";   // Output: 1 (true)
std::cout << (a <= b) << "\n";  // Output: 1 (true)
```

**How lexicographical comparison works:** Compares element-by-element like `std::lexicographical_compare` — the first differing element decides the result; if one set is a prefix of the other, the shorter one is "less".

```cpp
std::set<int> x = {1, 2};
std::set<int> y = {1, 2, 3};
std::cout << (x < y);   // Output: 1 (true) — x is a prefix of y, so x < y
```

**Real-life example:** Checking if two teams have registered the exact same set of unique player jersey numbers (useful for validating that a roster hasn't changed between two data syncs).

```cpp
std::set<int> teamA_jerseys = {7, 10, 23};
std::set<int> teamB_jerseys = {7, 10, 23};
if (teamA_jerseys == teamB_jerseys) std::cout << "Rosters match exactly.";
```

---

## 11. Common STL Algorithms Used with Set

These live in `<algorithm>` and operate on **sorted ranges** — which makes `std::set` a perfect fit since it's always sorted. All five below run in **O(n + m)** where `n`, `m` are the sizes of the two input sets, and all require an `OutputIterator` (commonly `std::inserter` into a result container).

### 11.1 `set_union`

```cpp
#include <algorithm>
#include <iterator>

std::set<int> a = {1, 2, 3, 4};
std::set<int> b = {3, 4, 5, 6};
std::set<int> result;

std::set_union(a.begin(), a.end(), b.begin(), b.end(),
               std::inserter(result, result.begin()));

for (int x : result) std::cout << x << " ";
// Output: 1 2 3 4 5 6
```

Real-life: combining two teams' unique attendee lists for a joint event — everyone who attended either session.

### 11.2 `set_intersection`

```cpp
std::set<int> a = {1, 2, 3, 4};
std::set<int> b = {3, 4, 5, 6};
std::set<int> result;

std::set_intersection(a.begin(), a.end(), b.begin(), b.end(),
                       std::inserter(result, result.begin()));

for (int x : result) std::cout << x << " ";
// Output: 3 4
```

Real-life: finding customers who purchased **both** Product A and Product B (cross-sell analysis).

### 11.3 `set_difference`

```cpp
std::set<int> a = {1, 2, 3, 4};
std::set<int> b = {3, 4, 5, 6};
std::set<int> result;

std::set_difference(a.begin(), a.end(), b.begin(), b.end(),
                     std::inserter(result, result.begin()));

for (int x : result) std::cout << x << " ";
// Output: 1 2   (elements in a but NOT in b)
```

Real-life: finding students enrolled last semester who did **not** re-enroll this semester (churn analysis).

### 11.4 `set_symmetric_difference`

```cpp
std::set<int> a = {1, 2, 3, 4};
std::set<int> b = {3, 4, 5, 6};
std::set<int> result;

std::set_symmetric_difference(a.begin(), a.end(), b.begin(), b.end(),
                               std::inserter(result, result.begin()));

for (int x : result) std::cout << x << " ";
// Output: 1 2 5 6   (elements in exactly one of the two sets, not both)
```

Real-life: finding items that changed between two inventory snapshots — added OR removed, but not present in both.

### 11.5 `includes`

Checks whether one sorted range is a **subset** of another.

```cpp
std::set<int> a = {1, 2, 3, 4, 5};
std::set<int> sub = {2, 4};

bool isSubset = std::includes(a.begin(), a.end(), sub.begin(), sub.end());
std::cout << isSubset;   // Output: 1 (true)
```

Real-life: verifying that a user's granted permission set includes all permissions required for a specific action (`std::includes(userPerms.begin(), userPerms.end(), requiredPerms.begin(), requiredPerms.end())`).

### Summary Table

|Algorithm|Purpose|Complexity|
|---|---|---|
|`set_union`|Elements in A or B|O(n + m)|
|`set_intersection`|Elements in A and B|O(n + m)|
|`set_difference`|Elements in A but not B|O(n + m)|
|`set_symmetric_difference`|Elements in exactly one of A, B|O(n + m)|
|`includes`|Is B fully contained in A?|O(n + m)|

**Important note:** All these algorithms require both input ranges to already be sorted according to the **same comparator**. Since `std::set` is always sorted, it's the ideal input; mixing a `set<int>` (ascending) with a `set<int, greater<int>>` (descending) as the two ranges of the same algorithm call produces **undefined/incorrect results** — always ensure matching comparators, or convert one range first.

---

## 12. Complete Time Complexity Table

|Category|Function|Time Complexity|Space Complexity|
|---|---|---|---|
|Construction|Default|O(1)|O(1)|
|Construction|Copy|O(n)|O(n)|
|Construction|Move|O(1)|O(1)|
|Construction|Range (n elements)|O(n log n)|O(n)|
|Construction|Initializer list|O(n log n)|O(n)|
|Iterators|`begin/end/rbegin/rend/cbegin/cend/crbegin/crend`|O(1) each|O(1)|
|Iterators|`++it` / `--it`|O(log n) worst, O(1) amortized|O(1)|
|Capacity|`size()`|O(1)|O(1)|
|Capacity|`empty()`|O(1)|O(1)|
|Capacity|`max_size()`|O(1)|O(1)|
|Modifiers|`insert(value)`|O(log n)|O(1) extra (new node)|
|Modifiers|`insert(hint, value)`|O(1) amortized (correct hint) / O(log n)|O(1)|
|Modifiers|`insert(first, last)`|O(k log(n+k))|O(k)|
|Modifiers|`emplace(args...)`|O(log n)|O(1)|
|Modifiers|`emplace_hint(hint, args...)`|O(1) amortized / O(log n)|O(1)|
|Modifiers|`erase(key)`|O(log n)|O(1)|
|Modifiers|`erase(iterator)`|O(1) amortized|O(1)|
|Modifiers|`erase(first, last)`|O(k + log n)|O(1)|
|Modifiers|`clear()`|O(n)|O(1)|
|Modifiers|`swap()`|O(1)|O(1)|
|Modifiers|`extract()`|O(log n)|O(1)|
|Modifiers|`merge()`|O(n log(n+m)) approx|O(1) (no new allocation)|
|Lookup|`find()`|O(log n)|O(1)|
|Lookup|`count()`|O(log n)|O(1)|
|Lookup|`contains()`|O(log n)|O(1)|
|Lookup|`lower_bound()` / `upper_bound()`|O(log n)|O(1)|
|Lookup|`equal_range()`|O(log n)|O(1)|
|Observers|`key_comp()` / `value_comp()`|O(1)|O(1)|
|Operators|`==`, `!=`, `<`, `>`, `<=`, `>=`|O(n)|O(1)|
|Algorithms|`set_union/intersection/difference/symmetric_difference`|O(n + m)|O(n + m) for output|
|Algorithms|`includes`|O(n + m)|O(1)|
|Traversal|Full traversal (any method)|O(n)|O(1)|

**Where `n` = size of the set, `k` = number of elements being inserted/erased, `m` = size of a second set in a binary algorithm.**

---

## 13. Interview Notes

- **"Why `set` and not `sorted vector`?"** — If you insert/erase frequently and need to stay sorted, `set` wins (O(log n) vs O(n) for `vector` insert/erase due to shifting). If you build once and only read/search afterward, a sorted `vector` + `std::binary_search`/`std::lower_bound` usually **outperforms** `set` due to cache locality — this is a very common follow-up question.
- **"Why `set` and not `unordered_set`?"** — Use `set` when you need sorted order, range queries (`lower_bound`/`upper_bound`), or predictable iteration order. Use `unordered_set` when you only need O(1) average membership checks and don't care about order.
- **`set::iterator` is effectively `const`** — you cannot do `*it = 5`. This trips up many candidates who assume all containers give mutable iterators.
- **`erase(iterator)` returns the next iterator** — a very common interview gotcha: many candidates write `it++` after `erase(it)`, which is undefined behavior (the erased iterator is already invalidated). Correct pattern:
    
    ```cpp
    for (auto it = s.begin(); it != s.end(); ) {    if (shouldRemove(*it)) it = s.erase(it);    else ++it;}
    ```
    
- **Comparator must define a strict weak ordering.** A comparator like `return a <= b;` is a classic interview bug — it violates irreflexivity (`cmp(a,a)` must be `false`) and typically causes crashes or infinite loops inside the RB-tree balancing logic.
- **`lower_bound` vs `upper_bound` vs `find`** — a favorite whiteboard question: "given a sorted set, find the smallest element ≥ X" → `lower_bound`. "Smallest element > X" → `upper_bound`. "Exact match" → `find`.
- **Time complexity of building a `set` from `n` unsorted elements is O(n log n)**, same order as sorting — a common "gotcha" is assuming it's O(n) like building a hash set.
- **Space overhead** — be ready to explain the ~32-48 bytes/node overhead (3 pointers + color + payload) vs. a `vector<int>`'s 4 bytes/element, especially in memory-constrained interview scenarios (embedded systems, etc.).
- **Stability of iterators/pointers** is a key differentiator often asked: "If I hold a pointer to an element in a `set` and someone erases a _different_ element, is my pointer still valid?" → **Yes** (unlike `vector`).

---

## 14. Common Mistakes

### Mistake 1 — Trying to modify an element via iterator

```cpp
std::set<int> s = {1, 2, 3};
auto it = s.find(2);
*it = 20;   // COMPILE ERROR: assignment of read-only location
```

**Fix:** Erase and reinsert, or use `extract()` (C++17) to mutate then reinsert.

### Mistake 2 — Using `it++` after `erase(it)`

```cpp
for (auto it = s.begin(); it != s.end(); it++) {
    if (*it % 2 == 0) s.erase(it);   // UB: 'it' is invalidated, then incremented
}
```

**Fix:**

```cpp
for (auto it = s.begin(); it != s.end(); ) {
    if (*it % 2 == 0) it = s.erase(it);
    else ++it;
}
```

### Mistake 3 — Assuming `insert()` on a duplicate throws or overwrites

```cpp
std::set<int> s = {5};
s.insert(5);   // no exception, no overwrite — silently returns {existing_it, false}
```

**Fix:** Always check the returned `pair.second` if you need to know whether insertion happened.

### Mistake 4 — Bad comparator (not a strict weak ordering)

```cpp
struct BadCompare {
    bool operator()(int a, int b) const { return a <= b; }  // WRONG: should be strictly <
};
std::set<int, BadCompare> s;   // undefined behavior when populated — may crash or corrupt tree
```

**Fix:** Comparator must return `false` for `cmp(a, a)` and satisfy strict weak ordering; use `<` not `<=`.

### Mistake 5 — Confusing `set` with `map` when you need key-value pairs

```cpp
std::set<std::pair<std::string,int>> s;   // works, but awkward for "lookup by name only"
```

**Fix:** If you need to look up a value by key, use `std::map`. Use `set<pair<...>>` only when you genuinely need to sort/dedupe by the _combination_.

### Mistake 6 — Using `set` where `unordered_set` would be faster

```cpp
// Only need "have I seen this ID before?" — order doesn't matter
std::set<int> seen;   // O(log n) per check
// Better:
std::unordered_set<int> seen;  // O(1) average per check
```

### Mistake 7 — Assuming `size()` is O(n)

Unlike some older/other language container APIs, `std::set::size()` is guaranteed **O(1)** in C++11+ (implementations maintain an internal counter). No need to avoid calling it repeatedly.

### Mistake 8 — Dereferencing `end()`

```cpp
auto it = s.find(999);   // not found -> it == s.end()
std::cout << *it;         // UB: dereferencing end()
```

**Fix:** Always check `it != s.end()` first.

### Mistake 9 — Forgetting `set<vector<int>>` / `set<pair<>>` sort lexicographically, not by size/sum

```cpp
std::set<std::vector<int>> s = {{2}, {1, 100}};
// order is {1,100} then {2}  -- because 1 < 2 lexicographically at index 0
```

**Fix:** If you need to sort by a derived property (like sum or length), supply a **custom comparator**.

### Mistake 10 — Using `set` for frequent index-based access

```cpp
std::set<int> s = {10, 20, 30};
// s[1];  // COMPILE ERROR: no operator[]
```

**Fix:** Use `std::next(s.begin(), i)` (O(i), still not O(1) — reconsider if you truly need random access; maybe `vector` or `std::set` isn't the right container).

---

## 15. Practice Examples

### Example 1 — Remove duplicates from an array while sorting

```cpp
#include <set>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> nums = {5, 3, 5, 1, 3, 7, 1};
    std::set<int> unique(nums.begin(), nums.end());
    for (int n : unique) std::cout << n << " ";
}
// Output: 1 3 5 7
```

### Example 2 — Check if two arrays have any common elements

```cpp
#include <set>
#include <vector>
#include <algorithm>
#include <iterator>
#include <iostream>

int main() {
    std::vector<int> a = {1, 2, 3};
    std::vector<int> b = {4, 5, 3};
    std::set<int> sa(a.begin(), a.end()), sb(b.begin(), b.end());

    std::vector<int> common;
    std::set_intersection(sa.begin(), sa.end(), sb.begin(), sb.end(),
                           std::back_inserter(common));

    std::cout << (common.empty() ? "No common elements" : "Common elements exist");
}
// Output: Common elements exist
```

### Example 3 — Find the Kth smallest element

```cpp
#include <set>
#include <iterator>
#include <iostream>

int main() {
    std::set<int> s = {7, 2, 9, 4, 1};   // sorted internally: 1 2 4 7 9
    int k = 3;
    auto it = std::next(s.begin(), k - 1);
    std::cout << *it;   // Output: 4  (3rd smallest)
}
```

**Note:** `std::next` on a `set` iterator is O(k) (bidirectional, not random-access) — for frequent k-th queries on huge sets, consider an order-statistics tree (policy-based data structure) instead.

### Example 4 — Sliding window: check if any window of size k has all unique elements

```cpp
#include <set>
#include <vector>
#include <iostream>

bool allUniqueInWindow(std::vector<int>& arr, int k) {
    std::set<int> window(arr.begin(), arr.begin() + k);
    return window.size() == static_cast<size_t>(k);
}

int main() {
    std::vector<int> arr = {1, 2, 3, 2, 5};
    std::cout << std::boolalpha << allUniqueInWindow(arr, 4);   // Output: false (2 repeats)
    std::cout << " " << std::boolalpha << allUniqueInWindow(arr, 3);  // Output: true
}
```

### Example 5 — Custom comparator: sort strings by length then alphabetically

```cpp
#include <set>
#include <string>
#include <iostream>

struct ByLengthThenAlpha {
    bool operator()(const std::string& a, const std::string& b) const {
        if (a.size() != b.size()) return a.size() < b.size();
        return a < b;
    }
};

int main() {
    std::set<std::string, ByLengthThenAlpha> words = {"pear", "fig", "kiwi", "apple"};
    for (auto& w : words) std::cout << w << " ";
}
// Output: fig kiwi pear apple
```

### Example 6 — Maintaining a "top N recent unique IP addresses" log (real-life security use case)

```cpp
#include <set>
#include <string>
#include <iostream>

int main() {
    std::set<std::string> uniqueIPs;
    std::vector<std::string> logEntries = {
        "192.168.1.1", "10.0.0.5", "192.168.1.1", "172.16.0.2"
    };

    for (auto& ip : logEntries) uniqueIPs.insert(ip);

    std::cout << "Unique IPs seen: " << uniqueIPs.size() << "\n";
    for (auto& ip : uniqueIPs) std::cout << ip << " ";
}
// Output:
// Unique IPs seen: 3
// 10.0.0.5 172.16.0.2 192.168.1.1
```

### Example 7 — Using `equal_range` to implement a simple range filter

```cpp
#include <set>
#include <iostream>

int main() {
    std::set<int> prices = {100, 250, 300, 450, 600};
    int minP = 200, maxP = 500;

    auto lo = prices.lower_bound(minP);
    auto hi = prices.upper_bound(maxP);

    std::cout << "Products in range: ";
    for (auto it = lo; it != hi; ++it) std::cout << *it << " ";
}
// Output: Products in range: 250 300 450
```

### Example 8 — Extract + modify + reinsert pattern (C++17)

```cpp
#include <set>
#include <iostream>

int main() {
    std::set<int> scores = {50, 60, 70};
    auto node = scores.extract(60);
    node.value() += 15;              // "update" 60 -> 75
    scores.insert(std::move(node));

    for (int s : scores) std::cout << s << " ";
}
// Output: 50 70 75
```

---

## 16. Coding Interview Questions

### Q1. Find the longest consecutive sequence in an unsorted array

**Problem:** Given `{100, 4, 200, 1, 3, 2}`, find the length of the longest run of consecutive integers. Answer: `4` (sequence `1,2,3,4`).

```cpp
#include <set>
#include <vector>
#include <iostream>

int longestConsecutive(std::vector<int>& nums) {
    std::set<int> s(nums.begin(), nums.end());
    int longest = 0;

    for (int num : s) {
        // only start counting from the beginning of a sequence
        if (s.find(num - 1) == s.end()) {
            int length = 1;
            while (s.find(num + length) != s.end()) length++;
            longest = std::max(longest, length);
        }
    }
    return longest;
}

int main() {
    std::vector<int> nums = {100, 4, 200, 1, 3, 2};
    std::cout << longestConsecutive(nums);   // Output: 4
}
```

**Complexity:** O(n log n) due to set construction; each element is visited O(1) amortized times overall (classic amortized analysis: the inner `while` only runs for true sequence starts).

---

### Q2. Two Sum using a set (single pass)

**Problem:** Given an array and a target, determine if two distinct elements sum to target.

```cpp
#include <set>
#include <vector>
#include <iostream>

bool hasTwoSum(std::vector<int>& nums, int target) {
    std::set<int> seen;
    for (int n : nums) {
        if (seen.count(target - n)) return true;
        seen.insert(n);
    }
    return false;
}

int main() {
    std::vector<int> nums = {2, 7, 11, 15};
    std::cout << std::boolalpha << hasTwoSum(nums, 9);   // Output: true (2 + 7)
}
```

**Complexity:** O(n log n) — could be O(n) average with `unordered_set` instead; interviewers often ask you to name this tradeoff.

---

### Q3. Number of distinct islands / unique BFS visited-cell tracking

**Problem:** Track visited `(row, col)` cells during BFS/DFS in a grid without revisiting.

```cpp
#include <set>
#include <utility>
#include <iostream>

int main() {
    std::set<std::pair<int,int>> visited;
    std::vector<std::pair<int,int>> moves = {{0,0}, {0,1}, {0,0}, {1,1}};

    for (auto& cell : moves) {
        if (visited.count(cell)) {
            std::cout << "(" << cell.first << "," << cell.second << ") already visited\n";
        } else {
            visited.insert(cell);
            std::cout << "(" << cell.first << "," << cell.second << ") newly visited\n";
        }
    }
}
// Output:
// (0,0) newly visited
// (0,1) newly visited
// (0,0) already visited
// (1,1) newly visited
```

---

### Q4. Design a data structure supporting insert, delete, and getRandom in O(log n) with sorted order

**Discussion question (no single code answer expected):** Interviewers use this to test whether you know `set` gives O(log n) insert/delete but **no O(1) random access** (need `std::next` which is O(k), or an order-statistics/indexed structure — e.g., a Policy-Based Data Structure `tree` with `order_of_key`/`find_by_order` in GCC's `<ext/pb_ds/tree_policy.hpp>` — for true O(log n) random-by-rank access).

```cpp
// GCC-specific extension (not portable ISO C++, but common in competitive programming)
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;

typedef tree<int, null_type, std::less<int>, rb_tree_tag,
             tree_order_statistics_node_update> ordered_set;

int main() {
    ordered_set s;
    s.insert(5); s.insert(1); s.insert(3);
    std::cout << *s.find_by_order(1);        // Output: 3  (0-indexed 2nd smallest)
    std::cout << s.order_of_key(5);          // Output: 2  (count of elements < 5)
}
```

---

### Q5. Merge two sorted, duplicate-free lists into one duplicate-free sorted list

```cpp
#include <set>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> a = {1, 3, 5};
    std::vector<int> b = {3, 4, 5, 6};

    std::set<int> merged(a.begin(), a.end());
    merged.insert(b.begin(), b.end());

    for (int x : merged) std::cout << x << " ";
}
// Output: 1 3 4 5 6
```

---

### Q6. Implement a simple autocomplete "prefix search" using set + lower_bound

**Problem:** Given a dictionary and a prefix, return all words starting with that prefix.

```cpp
#include <set>
#include <string>
#include <iostream>

int main() {
    std::set<std::string> dict = {"cat", "car", "cart", "dog", "carbon"};
    std::string prefix = "car";

    auto it = dict.lower_bound(prefix);
    std::cout << "Suggestions: ";
    while (it != dict.end() && it->compare(0, prefix.size(), prefix) == 0) {
        std::cout << *it << " ";
        ++it;
    }
}
// Output: Suggestions: car carbon cart
```

**Note:** This works because `set<string>` is sorted lexicographically, so all words sharing a prefix are **contiguous** in iteration order — `lower_bound(prefix)` jumps straight to the first candidate in O(log n).

---

### Q7. Find if a subarray sums to a given value using running-sum set (variant)

**Problem:** Determine if there exists a contiguous subarray with sum exactly `k` (allowing negatives), using a `set` of prefix sums.

```cpp
#include <set>
#include <vector>
#include <iostream>

bool subarraySumExists(std::vector<int>& nums, int k) {
    std::set<int> prefixSums;
    prefixSums.insert(0);
    int sum = 0;
    for (int n : nums) {
        sum += n;
        if (prefixSums.count(sum - k)) return true;
        prefixSums.insert(sum);
    }
    return false;
}

int main() {
    std::vector<int> nums = {1, -1, 5, -2, 3};
    std::cout << std::boolalpha << subarraySumExists(nums, 3);   // Output: true ({1,-1,5,-2} sums to 3)
}
```

---

### Q8. Explain: Why can't you use `std::sort` directly on a `std::set`?

**Expected answer:** `std::sort` requires **random-access iterators** (it needs to jump around, e.g., for quicksort partitioning/introsort). `std::set` only provides **bidirectional iterators**. Attempting `std::sort(s.begin(), s.end())` is a **compile error**. Besides, it's conceptually redundant — a `set` is _already_ sorted at all times by construction; you'd instead copy it into a `vector` if you needed random-access sorted algorithms.

```cpp
std::set<int> s = {3, 1, 2};
// std::sort(s.begin(), s.end());   // COMPILE ERROR: no operator+ for set::iterator
```

---

---

_End of document — std::set Complete Reference Guide._