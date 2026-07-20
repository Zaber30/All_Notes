# C++ `stack` - Complete Notes

# What is `stack`?

A `stack` is an STL container adapter that follows the **LIFO (Last In, First Out)** principle.

This means:

> The **last element inserted** is the **first element removed**.

Example

```
Push: 10
Push: 20
Push: 30

Stack

Top
 ↓
30
20
10

Pop

30 removed first
```

---

# Header File

```cpp
#include <stack>
```

---

# Declaration

```cpp
stack<data_type> st;
```

Example

```cpp
stack<int> st;
```

---

# Properties

- Follows **LIFO**
- Insert/Delete only from the **top**
- No random access
- No iterators
- Cannot use indexing
- Default underlying container is `deque`

---

# Push Element

Adds an element to the top.

```cpp
st.push(10);
st.push(20);
st.push(30);
```

Visualization

```
Top
 ↓
30
20
10
```

Time Complexity

```
O(1)
```

---

# Pop Element

Removes the top element.

```cpp
st.pop();
```

Before

```
Top
 ↓
30
20
10
```

After

```
Top
 ↓
20
10
```

**Note:** `pop()` does **not** return the removed value.

Time Complexity

```
O(1)
```

---

# Access Top Element

Returns the top element.

```cpp
cout << st.top();
```

Output

```
30
```

Time Complexity

```
O(1)
```

---

# Size

Returns the number of elements.

```cpp
cout << st.size();
```

Time Complexity

```
O(1)
```

---

# Empty

Checks whether the stack is empty.

```cpp
if(st.empty())
{
    cout << "Empty";
}
```

Returns

```
true
```

or

```
false
```

Time Complexity

```
O(1)
```

---

# Swap

Swap two stacks.

```cpp
stack<int> s1, s2;

s1.swap(s2);
```

Time Complexity

```
O(1)
```

---

# Complete Example

```cpp
#include <iostream>
#include <stack>

using namespace std;

int main()
{
    stack<int> st;

    st.push(10);
    st.push(20);
    st.push(30);

    cout << st.top() << endl;

    st.pop();

    cout << st.top() << endl;
}
```

Output

```
30
20
```

---

# Traversing a Stack

A stack **does not support iterators**, so you cannot use:

```cpp
for(auto it = st.begin(); it != st.end(); ++it)
```

❌ Compilation Error

To print all elements, repeatedly access the top and pop it.

```cpp
while(!st.empty())
{
    cout << st.top() << " ";
    st.pop();
}
```

Output

```
30 20 10
```

**Note:** This empties the stack.

---

# Traversing Without Changing the Original Stack

Make a copy first.

```cpp
stack<int> temp = st;

while(!temp.empty())
{
    cout << temp.top() << " ";
    temp.pop();
}
```

Original stack remains unchanged.

---

# Common Operations

| Function | Description | Complexity |
|----------|-------------|------------|
| `push(x)` | Insert element | O(1) |
| `pop()` | Remove top element | O(1) |
| `top()` | Access top element | O(1) |
| `size()` | Number of elements | O(1) |
| `empty()` | Check if empty | O(1) |
| `swap()` | Swap two stacks | O(1) |

---

# Stack Visualization

Push

```cpp
st.push(10);
st.push(20);
st.push(30);
```

```
Top
 ↓
30
20
10
```

After

```cpp
st.pop();
```

```
Top
 ↓
20
10
```

---

# Important Points

- `push()` inserts at the top.
- `pop()` removes the top element.
- `top()` returns the top element.
- `pop()` returns **nothing** (`void`).
- No indexing (`st[0]` ❌).
- No iterators (`begin()`, `end()` ❌).
- No `find()`, `sort()`, `lower_bound()`, or `upper_bound()`.

---

# Applications of Stack

- Function call stack
- Undo/Redo operations
- Browser history
- Parentheses matching
- Expression evaluation
- Infix to Postfix conversion
- DFS (Depth-First Search)
- Backtracking

---

# Difference Between `stack` and `vector`

| Feature | `stack` | `vector` |
|---------|---------|----------|
| Access | Top only | Any index |
| Iterator | ❌ No | ✅ Yes |
| Indexing | ❌ No | ✅ Yes |
| Insert | Top only | Anywhere |
| Delete | Top only | Anywhere |
| LIFO | ✅ Yes | ❌ No |

---

# Interview Tips

- `stack` follows **LIFO (Last In, First Out)**.
- Only the **top element** can be accessed.
- `top()` returns the top element.
- `pop()` removes the top element but returns nothing.
- `push()`, `pop()`, and `top()` are all **O(1)**.
- `stack` does **not** support iterators or random access.
- To traverse a stack, repeatedly use `top()` and `pop()`.