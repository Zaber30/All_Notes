# C++ `queue` - Complete Notes

# What is `queue`?

A `queue` is an STL container adapter that follows the **FIFO (First In, First Out)** principle.

This means:

> The **first element inserted** is the **first element removed**.

Example

```
Push: 10
Push: 20
Push: 30

Front               Rear
 ↓                   ↓
10  →  20  →  30

Pop

10 removed first
```

---

# Header File

```cpp
#include <queue>
```

---

# Declaration

```cpp
queue<data_type> q;
```

Example

```cpp
queue<int> q;
```

---

# Properties

- Follows **FIFO (First In, First Out)**
- Insert at the **rear (back)**
- Remove from the **front**
- No random access
- No iterators
- No indexing
- Default underlying container is `deque`

---

# Push Element

Adds an element to the **rear**.

```cpp
q.push(10);
q.push(20);
q.push(30);
```

Visualization

```
Front          Rear
 ↓              ↓
10  → 20  → 30
```

Time Complexity

```
O(1)
```

---

# Pop Element

Removes the **front** element.

```cpp
q.pop();
```

Before

```
Front          Rear
 ↓              ↓
10  → 20  → 30
```

After

```
Front      Rear
 ↓          ↓
20  → 30
```

**Note:** `pop()` returns **nothing** (`void`).

Time Complexity

```
O(1)
```

---

# Access Front Element

Returns the first element.

```cpp
cout << q.front();
```

Output

```
10
```

Time Complexity

```
O(1)
```

---

# Access Rear Element

Returns the last element.

```cpp
cout << q.back();
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
cout << q.size();
```

Time Complexity

```
O(1)
```

---

# Empty

Checks whether the queue is empty.

```cpp
if(q.empty())
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

Swap two queues.

```cpp
queue<int> q1, q2;

q1.swap(q2);
```

Time Complexity

```
O(1)
```

---

# Complete Example

```cpp
#include <iostream>
#include <queue>

using namespace std;

int main()
{
    queue<int> q;

    q.push(10);
    q.push(20);
    q.push(30);

    cout << q.front() << endl;
    cout << q.back() << endl;

    q.pop();

    cout << q.front();
}
```

Output

```
10
30
20
```

---

# Traversing a Queue

A queue **does not support iterators**, so this is invalid:

```cpp
for(auto it = q.begin(); it != q.end(); ++it)
```

❌ Compilation Error

To print all elements, repeatedly access the front and pop it.

```cpp
while(!q.empty())
{
    cout << q.front() << " ";
    q.pop();
}
```

Output

```
10 20 30
```

**Note:** This empties the queue.

---

# Traversing Without Changing the Original Queue

Make a copy first.

```cpp
queue<int> temp = q;

while(!temp.empty())
{
    cout << temp.front() << " ";
    temp.pop();
}
```

Original queue remains unchanged.

---

# Common Operations

| Function | Description | Complexity |
|----------|-------------|------------|
| `push(x)` | Insert at rear | O(1) |
| `pop()` | Remove front | O(1) |
| `front()` | Access first element | O(1) |
| `back()` | Access last element | O(1) |
| `size()` | Number of elements | O(1) |
| `empty()` | Check if empty | O(1) |
| `swap()` | Swap two queues | O(1) |

---

# Queue Visualization

Push

```cpp
q.push(10);
q.push(20);
q.push(30);
```

```
Front          Rear
 ↓              ↓
10  → 20  → 30
```

After

```cpp
q.pop();
```

```
Front      Rear
 ↓          ↓
20  → 30
```

---

# Important Points

- `push()` inserts at the **rear**.
- `pop()` removes the **front** element.
- `front()` returns the first element.
- `back()` returns the last element.
- `pop()` returns **nothing** (`void`).
- No indexing (`q[0]` ❌).
- No iterators (`begin()`, `end()` ❌).
- No `find()`, `sort()`, `lower_bound()`, or `upper_bound()`.

---

# Applications of Queue

- Breadth-First Search (BFS)
- CPU scheduling
- Printer queue
- Task scheduling
- Message queues
- Order processing systems
- Simulation problems

---

# Difference Between `queue` and `stack`

| Feature | `queue` | `stack` |
|---------|---------|---------|
| Principle | FIFO | LIFO |
| Insert | Rear | Top |
| Remove | Front | Top |
| Access | `front()` and `back()` | `top()` |
| Iterator | ❌ No | ❌ No |
| Indexing | ❌ No | ❌ No |

---

# Interview Tips

- `queue` follows **FIFO (First In, First Out)**.
- Insert using `push()`.
- Remove using `pop()`.
- Access first element using `front()`.
- Access last element using `back()`.
- `push()`, `pop()`, `front()`, and `back()` are all **O(1)**.
- `queue` does **not** support iterators or random access.
- To traverse a queue, repeatedly use `front()` and `pop()`.