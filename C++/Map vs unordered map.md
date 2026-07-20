# Difference Between `map` and `unordered_map`

| Feature | `map` | `unordered_map` |
|---------|-------|-----------------|
| Internal Structure | Red-Black Tree | Hash Table |
| Order | Keys are **sorted** | No ordering |
| Duplicate Keys | ❌ Not allowed | ❌ Not allowed |
| Search (`find`) | O(log n) | O(1) average, O(n) worst |
| Insert | O(log n) | O(1) average, O(n) worst |
| Delete | O(log n) | O(1) average, O(n) worst |
| `lower_bound()` | ✅ Available | ❌ Not available |
| `upper_bound()` | ✅ Available | ❌ Not available |
| Iteration Order | Sorted by key | Unspecified (random) |
| Uses Hash Function | ❌ No | ✅ Yes |

---

# `map`

Stores key-value pairs in **sorted order by key**.

```cpp
map<int, string> mp;

mp[3] = "C";
mp[1] = "A";
mp[2] = "B";

for(auto x : mp)
{
    cout << x.first << " " << x.second << endl;
}
```

Output

```
1 A
2 B
3 C
```

---

# `unordered_map`

Stores key-value pairs using a **hash table**.

```cpp
unordered_map<int, string> ump;

ump[3] = "C";
ump[1] = "A";
ump[2] = "B";

for(auto x : ump)
{
    cout << x.first << " " << x.second << endl;
}
```

Possible Output

```
2 B
3 C
1 A
```

or

```
3 C
1 A
2 B
```

The order is **not guaranteed**.

---

# Search Example

## `map`

```cpp
map<int,string> mp;

auto it = mp.find(2);
```

Complexity

```
O(log n)
```

---

## `unordered_map`

```cpp
unordered_map<int,string> ump;

auto it = ump.find(2);
```

Complexity

```
O(1) average
```

---

# `lower_bound()` Example

## `map`

```cpp
map<int,string> mp;

auto it = mp.lower_bound(5);
```

✅ Works

---

## `unordered_map`

```cpp
unordered_map<int,string> ump;

ump.lower_bound(5);
```

❌ Compilation Error

`unordered_map` does **not** support `lower_bound()` or `upper_bound()` because the keys are not sorted.

---

# When to Use `map`

Use `map` when:

- You need keys in **sorted order**.
- You need `lower_bound()` or `upper_bound()`.
- You need ordered traversal.
- You need the smallest or largest key by iteration.

---

# When to Use `unordered_map`

Use `unordered_map` when:

- You only need fast lookup, insertion, and deletion.
- The order of keys doesn't matter.
- Performance is more important than ordering.

---

# Example

```cpp
map<int,string> mp;

mp[5] = "E";
mp[2] = "B";
mp[4] = "D";
```

Iteration

```
2 B
4 D
5 E
```

---

```cpp
unordered_map<int,string> ump;

ump[5] = "E";
ump[2] = "B";
ump[4] = "D";
```

Iteration (example)

```
4 D
5 E
2 B
```

---

# Time Complexity

| Operation | `map` | `unordered_map` |
|-----------|--------|-----------------|
| Insert | O(log n) | O(1) average |
| Search | O(log n) | O(1) average |
| Delete | O(log n) | O(1) average |
| `lower_bound()` | O(log n) | ❌ Not Available |
| `upper_bound()` | O(log n) | ❌ Not Available |

---

# Memory Trick

```
map
↓

Sorted
↓

Tree
↓

O(log n)
```

```
unordered_map
↓

Unsorted
↓

Hash Table
↓

O(1) average
```

---

# Interview Summary

| If you need... | Use |
|----------------|-----|
| Sorted keys | `map` |
| Fast lookup | `unordered_map` |
| `lower_bound()` / `upper_bound()` | `map` |
| Ordered iteration | `map` |
| Best average search performance | `unordered_map` |