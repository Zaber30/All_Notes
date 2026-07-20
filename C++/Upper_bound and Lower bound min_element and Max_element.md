# C++ `lower_bound()` and `upper_bound()`

## Important

> Works **only on sorted data**.

---

# `lower_bound()`

## Definition

Returns an **iterator** to the **first element that is greater than or equal to (`>=`) the given value**.

### Syntax

```cpp
auto it = lower_bound(v.begin(), v.end(), value);
```

### Example

```cpp
vector<int> v = {10,20,20,20,30,40};

auto it = lower_bound(v.begin(), v.end(), 20);

cout << *it;
```

Output

```
20
```

Visualization

```
10 20 20 20 30 40
   ^
lower_bound
```

---

# `upper_bound()`

## Definition

Returns an **iterator** to the **first element that is greater than (`>`) the given value**.

### Syntax

```cpp
auto it = upper_bound(v.begin(), v.end(), value);
```

### Example

```cpp
vector<int> v = {10,20,20,20,30,40};

auto it = upper_bound(v.begin(), v.end(), 20);

cout << *it;
```

Output

```
30
```

Visualization

```
10 20 20 20 30 40
            ^
       upper_bound
```

---

# Example with Value Not Present

```cpp
vector<int> v = {10,20,20,20,30,40};

cout << *lower_bound(v.begin(), v.end(), 25) << endl;
cout << *upper_bound(v.begin(), v.end(), 25);
```

Output

```
30
30
```

Explanation

- First element **>= 25** → `30`
- First element **> 25** → `30`

---

# Return Type

Both functions return an **iterator**.

If no valid element exists, they return:

```cpp
v.end();
```

Example

```cpp
auto it = lower_bound(v.begin(), v.end(), 50);

if(it == v.end())
{
    cout << "Not Found";
}
```

---

# Find Index

```cpp
vector<int> v = {10,20,20,20,30,40};

auto it = lower_bound(v.begin(), v.end(), 30);

cout << it - v.begin();
```

Output

```
4
```

---

# Count Frequency of an Element

```cpp
vector<int> v = {10,20,20,20,30,40};

int count = upper_bound(v.begin(), v.end(), 20)
          - lower_bound(v.begin(), v.end(), 20);

cout << count;
```

Output

```
3
```

---

# Time Complexity

| Function | Complexity |
|----------|------------|
| `lower_bound()` | **O(log n)** |
| `upper_bound()` | **O(log n)** |

---

# Easy Memory Trick

```
lower_bound(x)
        >= x

upper_bound(x)
        > x
```

---

# Summary

| Function | Returns |
|----------|----------|
| `lower_bound(x)` | First element **>= x** |
| `upper_bound(x)` | First element **> x** |
| Return Type | Iterator |
| Not Found | `container.end()` |
| Works On | Sorted containers/ranges |
| Time Complexity | **O(log n)** |