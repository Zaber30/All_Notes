# How `std::unique()` Works (Step by Step)

## What is `unique()`?

`std::unique()` is an STL algorithm that **removes consecutive duplicate elements** by moving the unique elements to the front of the container.

> **Important:** `unique()` **does not change the container's size.**
>
> It only rearranges the elements and returns an iterator to the **new logical end**.

Header File

```cpp
#include <algorithm>
```

Syntax

```cpp
auto newEnd = unique(first, last);
```

---

# Return Value

`unique()` returns an iterator pointing to **one position after the last unique element**.

```
[first, newEnd)
```

is the valid range.

---

# Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
    vector<int> v = {1,1,2,2,3,3,4};

    auto it = unique(v.begin(), v.end());

    for(int x : v)
        cout << x << " ";
}
```

Output (Actual vector memory)

```
1 2 3 4 3 3 4
```

Notice

The vector size is still **7**.

Only the first **4** elements are valid.

---

# Step-by-Step Working

Initial vector

```
Index : 0 1 2 3 4 5 6

Value : 1 1 2 2 3 3 4
```

---

## Step 1

Keep the first element.

```
1 1 2 2 3 3 4
^

Unique Part

1
```

---

## Step 2

Current element

```
1
```

Previous unique element

```
1
```

They are equal.

Ignore it.

```
1 1 2 2 3 3 4
  ^
```

Unique Part

```
1
```

---

## Step 3

Current element

```
2
```

Previous unique element

```
1
```

Different.

Move it to the next available position.

```
Before

1 1 2 2 3 3 4

After

1 2 2 2 3 3 4
```

Unique Part

```
1 2
```

---

## Step 4

Current element

```
2
```

Previous unique

```
2
```

Duplicate.

Ignore it.

```
1 2 2 2 3 3 4
      ^
```

---

## Step 5

Current element

```
3
```

Different from previous unique.

Move it forward.

```
Before

1 2 2 2 3 3 4

After

1 2 3 2 3 3 4
```

Unique Part

```
1 2 3
```

---

## Step 6

Current element

```
3
```

Duplicate.

Ignore it.

---

## Step 7

Current element

```
4
```

Different.

Move it forward.

```
Before

1 2 3 2 3 3 4

After

1 2 3 4 3 3 4
```

Unique Part

```
1 2 3 4
```

---

# Final Result

```
Index : 0 1 2 3 4 5 6

Value : 1 2 3 4 3 3 4
```

The iterator returned by `unique()` points here.

```
1 2 3 4 | 3 3 4
        ^
       newEnd
```

The valid range is

```
[v.begin(), newEnd)
```

Only

```
1 2 3 4
```

should be considered.

---

# Removing the Remaining Elements

Use `erase()`.

```cpp
v.erase(unique(v.begin(), v.end()), v.end());
```

Now the vector becomes

```
1 2 3 4
```

---

# Why Doesn't `unique()` Remove Them Automatically?

Because `unique()` is a generic STL algorithm.

It works with many containers and iterator ranges.

Algorithms **cannot change the size of a container**.

Only the container itself (like `vector`) can remove elements.

That's why we call

```cpp
erase()
```

after

```cpp
unique()
```

---

# Example with Non-Consecutive Duplicates

```cpp
vector<int> v = {1,2,1,3,2};

unique(v.begin(), v.end());
```

Output

```
1 2 1 3 2
```

Nothing changes.

Reason

```
unique()

↓

Only removes consecutive duplicates.
```

---

# Remove All Duplicates

Sort first.

```cpp
sort(v.begin(), v.end());

v.erase(unique(v.begin(), v.end()), v.end());
```

Before sorting

```
3 1 2 1 4 2
```

After sorting

```
1 1 2 2 3 4
```

After `unique()`

```
1 2 3 4
```

---

# Internal Logic (Pseudo Code)

```cpp
result = first

for(current = first + 1; current != last; current++)
{
    if(*current != *result)
    {
        result++

        *result = *current
    }
}

return result + 1
```

---

# Time Complexity

| Operation | Complexity |
|-----------|------------|
| `unique()` | **O(n)** |
| `erase()` | **O(n)** |
| `sort()` | **O(n log n)** |
| `sort() + unique()` | **O(n log n)** |

---

# Important Points

- `unique()` only removes **consecutive** duplicates.
- It does **not** reduce the container's size.
- It returns an iterator to **one position after the last unique element**.
- The standard pattern is:

```cpp
v.erase(unique(v.begin(), v.end()), v.end());
```

- To remove **all** duplicates from an unsorted vector:

```cpp
sort(v.begin(), v.end());

v.erase(unique(v.begin(), v.end()), v.end());
```

---

# Interview Tip

Remember this sequence:

```
Sort
    ↓
unique()
    ↓
erase()
```

This is the standard STL idiom for removing **all duplicate elements** from a `std::vector`.