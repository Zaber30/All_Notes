# C++ `priority_queue` - Complete Notes

# What is `priority_queue`?

A `priority_queue` is an STL container adapter where the element with the **highest priority** is always at the top.

By default:

> **Largest element has the highest priority.**

It is implemented using a **Heap (Binary Heap)**.

---

# Header File

```cpp
#include <queue>
```

---

# Declaration (Max Heap)

```cpp
priority_queue<data_type> pq;
```

Example

```cpp
priority_queue<int> pq;
```

---

# Properties

- Implemented using **Binary Heap**
- By default, it is a **Max Heap**
- Largest element is always at the top
- No iterators
- No indexing
- No random access

---

# Push Element

Insert an element.

```cpp
pq.push(10);
pq.push(30);
pq.push(20);
pq.push(50);
```

Visualization

```
Top
 ↓
50
30
20
10
```

**Note:** Internally the heap structure is different, but `top()` always returns the largest element.

Time Complexity

```
O(log n)
```

---

# Top Element

Returns the highest-priority element.

```cpp
cout << pq.top();
```

Output

```
50
```

Time Complexity

```
O(1)
```

---

# Pop Element

Removes the top element.

```cpp
pq.pop();
```

Before

```
Top
 ↓
50
30
20
10
```

After

```
Top
 ↓
30
20
10
```

Time Complexity

```
O(log n)
```

---

# Size

Returns the number of elements.

```cpp
cout << pq.size();
```

Time Complexity

```
O(1)
```

---

# Empty

Checks whether the priority queue is empty.

```cpp
if(pq.empty())
{
    cout << "Empty";
}
```

Time Complexity

```
O(1)
```

---

# Swap

Swap two priority queues.

```cpp
priority_queue<int> pq1, pq2;

pq1.swap(pq2);
```

Time Complexity

```
O(1)
```

---

# Complete Example (Max Heap)

```cpp
#include <iostream>
#include <queue>

using namespace std;

int main()
{
    priority_queue<int> pq;

    pq.push(10);
    pq.push(50);
    pq.push(20);
    pq.push(30);

    while(!pq.empty())
    {
        cout << pq.top() << " ";
        pq.pop();
    }
}
```

Output

```
50 30 20 10
```

---

# Min Heap

To make the **smallest element** have the highest priority:

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

Example

```cpp
priority_queue<int, vector<int>, greater<int>> pq;

pq.push(10);
pq.push(50);
pq.push(20);
pq.push(30);

while(!pq.empty())
{
    cout << pq.top() << " ";
    pq.pop();
}
```

Output

```
10 20 30 50
```

---

# Explanation of Min Heap Declaration

```cpp
priority_queue<
    int,              // Data type
    vector<int>,      // Underlying container
    greater<int>      // Comparator
> pq;
```

- `int` → Type of elements
- `vector<int>` → Stores the heap
- `greater<int>` → Makes it a Min Heap

---

# Traversing a Priority Queue

A `priority_queue` does **not** support iterators.

❌ Invalid

```cpp
for(auto it = pq.begin(); it != pq.end(); ++it)
```

To print all elements:

```cpp
while(!pq.empty())
{
    cout << pq.top() << " ";
    pq.pop();
}
```

Output (Max Heap)

```
50 30 20 10
```

**Note:** This empties the priority queue.

---

# Traverse Without Modifying

```cpp
priority_queue<int> temp = pq;

while(!temp.empty())
{
    cout << temp.top() << " ";
    temp.pop();
}
```

Original priority queue remains unchanged.

---

# Common Operations

| Function | Description | Complexity |
|----------|-------------|------------|
| `push(x)` | Insert element | O(log n) |
| `pop()` | Remove top element | O(log n) |
| `top()` | Access highest-priority element | O(1) |
| `size()` | Number of elements | O(1) |
| `empty()` | Check if empty | O(1) |
| `swap()` | Swap two priority queues | O(1) |

---

# Max Heap Example

```cpp
priority_queue<int> pq;

pq.push(40);
pq.push(10);
pq.push(30);
pq.push(20);
```

```
top() = 40

After pop()

top() = 30
```

---

# Min Heap Example

```cpp
priority_queue<int, vector<int>, greater<int>> pq;

pq.push(40);
pq.push(10);
pq.push(30);
pq.push(20);
```

```
top() = 10

After pop()

top() = 20
```

---

# Applications

- Dijkstra's Algorithm
- Prim's Algorithm
- Huffman Coding
- Job Scheduling
- Event Simulation
- K Largest / K Smallest Elements
- Merge K Sorted Arrays
- Task Scheduling

---

# Difference Between Queue and Priority Queue

| Feature | `queue` | `priority_queue` |
|---------|---------|------------------|
| Order | FIFO | Highest priority first |
| Access | `front()` | `top()` |
| Removal | Oldest element | Highest-priority element |
| Internal Structure | `deque` | Binary Heap |
| `push()` | O(1) | O(log n) |
| `pop()` | O(1) | O(log n) |

---

# Difference Between Max Heap and Min Heap

| Feature | Max Heap | Min Heap |
|---------|----------|----------|
| Top Element | Largest | Smallest |
| Declaration | `priority_queue<int>` | `priority_queue<int, vector<int>, greater<int>>` |

---

# Important Points

- Default `priority_queue` is a **Max Heap**.
- Uses a **Binary Heap** internally.
- `top()` returns the highest-priority element.
- `pop()` removes the highest-priority element.
- No iterators.
- No indexing.
- No `find()`, `lower_bound()`, or `upper_bound()`.

---

# Interview Tips

- `priority_queue` stores elements according to **priority**, not insertion order.
- Default is **Max Heap**.
- Use `greater<int>` to create a **Min Heap**.
- `push()` and `pop()` are **O(log n)**.
- `top()` is **O(1)**.
- No iterators or random access.
- Implemented using a **Binary Heap**.