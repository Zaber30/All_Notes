# C++ `map` - Complete Notes

# What is `map`?

A `map` is an STL associative container that stores data in **key-value pairs**.

- Each **key** is **unique**.
- Values can be duplicated.
- Keys are automatically stored in **sorted (ascending)** order.

Internally, `map` is implemented using a **Red-Black Tree**.

---

# Header File

```cpp
#include <map>
```

---

# Declaration

```cpp
map<key_type, value_type> mp;
```

Example

```cpp
map<int, string> mp;
```

---

# Example

```cpp
map<int, string> mp;

mp[1] = "Alice";
mp[2] = "Bob";
mp[3] = "Charlie";
```

Visualization

```
Key     Value
----------------
1   ->  Alice
2   ->  Bob
3   ->  Charlie
```

---

# Properties

- Stores **key-value pairs**
- Keys are **unique**
- Automatically **sorted by key**
- Implemented using **Red-Black Tree**
- Search, Insert, Delete = **O(log n)**

---

# Insert Elements

## Method 1

```cpp
mp[1] = "Alice";
```

---

## Method 2

```cpp
mp.insert({2, "Bob"});
```

---

## Method 3

```cpp
mp.emplace(3, "Charlie");
```

---

# Access Elements

## Using []

```cpp
cout << mp[1];
```

Output

```
Alice
```

---

## Using at()

```cpp
cout << mp.at(2);
```

Output

```
Bob
```

Difference

```cpp
mp[10];
```

Creates a new key if it doesn't exist.

```cpp
mp.at(10);
```

Throws an exception if the key doesn't exist.

---

# Iterate Through Map

```cpp
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
3 Charlie
```

---

Using Range-based Loop

```cpp
for(auto x : mp)
{
    cout << x.first << " ";
    cout << x.second << endl;
}
```

---

# Important Member Functions

---

## begin()

Returns iterator to first element.

```cpp
auto it = mp.begin();
```

Complexity

```
O(1)
```

---

## end()

Returns iterator to one position after the last element.

```cpp
auto it = mp.end();
```

Complexity

```
O(1)
```

---

## size()

Returns number of key-value pairs.

```cpp
cout << mp.size();
```

Complexity

```
O(1)
```

---

## empty()

Checks whether map is empty.

```cpp
if(mp.empty())
```

Complexity

```
O(1)
```

---

## clear()

Removes all elements.

```cpp
mp.clear();
```

Complexity

```
O(n)
```

---

## insert()

```cpp
mp.insert({4,"David"});
```

Complexity

```
O(log n)
```

---

## emplace()

Constructs element directly.

```cpp
mp.emplace(5,"Eva");
```

Complexity

```
O(log n)
```

---

## erase(key)

Remove using key.

```cpp
mp.erase(2);
```

Complexity

```
O(log n)
```

---

## erase(iterator)

```cpp
mp.erase(mp.begin());
```

Complexity

```
O(log n)
```

---

## erase(first,last)

Remove a range.

```cpp
mp.erase(mp.begin(), mp.end());
```

Complexity

```
O(n)
```

---

## find()

Returns iterator to the key.

```cpp
auto it = mp.find(2);
```

If found

```cpp
cout << it->second;
```

If not found

```cpp
if(it == mp.end())
```

Complexity

```
O(log n)
```

---

## count()

Checks whether a key exists.

```cpp
cout << mp.count(2);
```

Output

```
1
```

or

```
0
```

Because keys are unique.

Complexity

```
O(log n)
```

---

## lower_bound()

Returns iterator to first key **>= key**.

```cpp
auto it = mp.lower_bound(3);
```

Complexity

```
O(log n)
```

---

## upper_bound()

Returns iterator to first key **> key**.

```cpp
auto it = mp.upper_bound(3);
```

Complexity

```
O(log n)
```

---

## swap()

```cpp
mp1.swap(mp2);
```

Complexity

```
O(1)
```

---

# Example of find()

```cpp
map<int,string> mp;

mp[1] = "Alice";
mp[2] = "Bob";

auto it = mp.find(2);

if(it != mp.end())
{
    cout << it->first << endl;
    cout << it->second;
}
```

Output

```
2
Bob
```

---

# Example of lower_bound()

```cpp
map<int,string> mp;

mp[1] = "A";
mp[3] = "B";
mp[5] = "C";

auto it = mp.lower_bound(2);

cout << it->first;
```

Output

```
3
```

---

# Example of upper_bound()

```cpp
map<int,string> mp;

mp[1] = "A";
mp[3] = "B";
mp[5] = "C";

auto it = mp.upper_bound(3);

cout << it->first;
```

Output

```
5
```

---

# Difference Between [] and at()

| Function | Key Exists | Key Doesn't Exist |
|----------|------------|-------------------|
| `mp[key]` | Returns value | Creates new key with default value |
| `mp.at(key)` | Returns value | Throws exception |

---

# Difference Between find() and count()

| Function | Returns |
|----------|----------|
| `find()` | Iterator |
| `count()` | 0 or 1 |

Example

```cpp
if(mp.find(5) != mp.end())
```

or

```cpp
if(mp.count(5))
```

---

# Time Complexity

| Operation | Complexity |
|-----------|------------|
| `insert()` | O(log n) |
| `emplace()` | O(log n) |
| `erase(key)` | O(log n) |
| `erase(iterator)` | O(log n) |
| `find()` | O(log n) |
| `count()` | O(log n) |
| `lower_bound()` | O(log n) |
| `upper_bound()` | O(log n) |
| `operator[]` | O(log n) |
| `at()` | O(log n) |
| `size()` | O(1) |
| `empty()` | O(1) |
| `begin()` | O(1) |
| `end()` | O(1) |
| `clear()` | O(n) |
| `swap()` | O(1) |

---

# Important Points

- Stores **key-value pairs**
- Keys are **always unique**
- Keys are **automatically sorted**
- Internally uses a **Red-Black Tree**
- Most operations take **O(log n)** time
- `find()`, `lower_bound()`, and `upper_bound()` return **iterators**
- Access key using `it->first`
- Access value using `it->second`
- `operator[]` inserts a new key if it does not exist
- `at()` does **not** insert a new key

---

# Interview Tips

- `map` = Sorted + Unique Keys
- Internal structure = Red-Black Tree
- `find()` → Returns iterator to key
- `lower_bound(k)` → First key **>= k**
- `upper_bound(k)` → First key **> k**
- `count(k)` → Returns `0` or `1`
- `it->first` → Key
- `it->second` → Value
- Search, Insert, Delete = **O(log n)**