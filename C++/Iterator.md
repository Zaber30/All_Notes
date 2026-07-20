# C++ Iterator

# What is an Iterator?

An **iterator** is an object that **points to an element inside a container** (such as `vector`, `set`, `map`, or `string`).

It is similar to a **pointer**, but it works with STL containers.

Think of an iterator as a tool that allows you to **traverse (iterate through)** the elements of a container one by one.

---

# Why Do We Need Iterators?

Suppose we have a vector.

```cpp
vector<int> v = {10,20,30,40,50};
```

Without an iterator, we can access elements using indexing.

```cpp
cout << v[0];
cout << v[1];
cout << v[2];
```

This works only because `vector` supports indexing.

However, containers like `set` and `map` **do not support indexing**.

```cpp
set<int> s = {10,20,30};

// Invalid
cout << s[0];
```

Therefore, STL provides **iterators** that work for almost every container.

---

# Declaration

```cpp
vector<int>::iterator it;
```

or

```cpp
auto it = v.begin();
```

Using `auto` is recommended because the iterator type can be very long.

Example

```cpp
map<int,string>::iterator it;
```

Instead of writing that, we write

```cpp
auto it = mp.begin();
```

---

# begin()

Returns an iterator pointing to the **first element**.

```cpp
vector<int> v = {10,20,30};

auto it = v.begin();

cout << *it;
```

Output

```
10
```

Visualization

```
10 20 30
^
it
```

---

# end()

Returns an iterator pointing **one position after the last element**.

```cpp
auto it = v.end();
```

Visualization

```
10 20 30
        ^
      end()
```

You **cannot** dereference `end()`.

```cpp
cout << *v.end();   // ❌ Undefined Behavior
```

---

# Dereference Operator (*)

The `*` operator returns the value pointed to by the iterator.

```cpp
vector<int> v = {10,20,30};

auto it = v.begin();

cout << *it;
```

Output

```
10
```

---

# Moving an Iterator

Move Forward

```cpp
++it;
```

Move Backward (if supported)

```cpp
--it;
```

Example

```cpp
vector<int> v = {10,20,30};

auto it = v.begin();

++it;

cout << *it;
```

Output

```
20
```

---

# Traversing a Vector

```cpp
vector<int> v = {10,20,30,40};

for(auto it = v.begin(); it != v.end(); ++it)
{
    cout << *it << " ";
}
```

Output

```
10 20 30 40
```

---

# Traversing a Set

```cpp
set<int> s = {10,20,30};

for(auto it = s.begin(); it != s.end(); ++it)
{
    cout << *it << " ";
}
```

Output

```
10 20 30
```

---

# Traversing a Map

```cpp
map<int,string> mp =
{
    {1,"Alice"},
    {2,"Bob"}
};

for(auto it = mp.begin(); it != mp.end(); ++it)
{
    cout << it->first << " ";
    cout << it->second << endl;
}
```

Output

```
1 Alice
2 Bob
```

Notice

```cpp
it->first
```

returns the key.

```cpp
it->second
```

returns the value.

---

# Reverse Iterator

Starts from the last element.

```cpp
auto it = v.rbegin();
```

Example

```cpp
vector<int> v = {10,20,30};

for(auto it = v.rbegin(); it != v.rend(); ++it)
{
    cout << *it << " ";
}
```

Output

```
30 20 10
```

---

# Const Iterator

A const iterator cannot modify elements.

```cpp
vector<int> v = {10,20,30};

for(auto it = v.cbegin(); it != v.cend(); ++it)
{
    cout << *it;
}
```

---

# Iterator Categories

## 1. Random Access Iterator

Supports

```cpp
it + 5;
it - 2;
it[3];
```

Containers

- vector
- deque
- array
- string

---

## 2. Bidirectional Iterator

Supports

```cpp
++it;
--it;
```

Containers

- set
- multiset
- map
- multimap
- list

---

## 3. Forward Iterator

Supports only

```cpp
++it;
```

Containers

- forward_list
- unordered_set
- unordered_map
- unordered_multiset
- unordered_multimap

---

# Iterator Operations

| Operation | Description | Complexity |
|------------|-------------|------------|
| `begin()` | First element | O(1) |
| `end()` | One past last element | O(1) |
| `rbegin()` | Last element | O(1) |
| `rend()` | Before first element (reverse) | O(1) |
| `cbegin()` | Const begin | O(1) |
| `cend()` | Const end | O(1) |
| `++it` | Move forward | O(1) |
| `--it` | Move backward (if supported) | O(1) |
| `*it` | Access value | O(1) |
| `it->member` | Access object member | O(1) |

---

# Difference Between Pointer and Iterator

| Pointer | Iterator |
|----------|----------|
| Works with arrays | Works with STL containers |
| `int* p` | `vector<int>::iterator it` |
| Supports pointer arithmetic | Depends on iterator category |
| Raw memory address | Container abstraction |

---

# Common STL Algorithms That Return Iterators

```cpp
find()
```

```cpp
auto it = find(v.begin(), v.end(), 30);
```

---

```cpp
lower_bound()
```

```cpp
auto it = lower_bound(v.begin(), v.end(), 30);
```

---

```cpp
upper_bound()
```

```cpp
auto it = upper_bound(v.begin(), v.end(), 30);
```

---

```cpp
unique()
```

```cpp
auto it = unique(v.begin(), v.end());
```

Returns the iterator to the **new logical end**.

---

```cpp
remove()
```

```cpp
auto it = remove(v.begin(), v.end(), 10);
```

Returns the iterator to the **new logical end**.

---

# Common Mistakes

### Wrong

```cpp
cout << *v.end();
```

`end()` points **after** the last element.

---

### Correct

```cpp
cout << *(v.end() - 1);
```

Only works for containers with **random access iterators** (e.g., `vector`, `deque`, `array`, `string`).

For a `set` or `map`, use:

```cpp
auto it = prev(s.end());

cout << *it;
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| Dereference (`*it`) | O(1) |
| Increment (`++it`) | O(1) |
| Decrement (`--it`) | O(1) |
| `begin()` | O(1) |
| `end()` | O(1) |
| `rbegin()` | O(1) |
| `rend()` | O(1) |
| Iterator comparison (`it == other`) | O(1) |

---

# Interview Tips

- An **iterator** is like a generalized pointer for STL containers.
- `begin()` points to the first element.
- `end()` points **one past the last element**.
- `*it` accesses the current element.
- `it->member` is used when the element is an object or a `pair` (such as in a `map`).
- Not every iterator supports `+` and `-`; this depends on its **iterator category**.
- Most STL algorithms (`find`, `lower_bound`, `upper_bound`, `unique`, `remove`) **return iterators**, not indexes.