# Difference Between `queue` and `priority_queue`

| Feature | `queue` | `priority_queue` |
|---------|---------|------------------|
| Principle | FIFO (First In, First Out) | Highest Priority First |
| Internal Structure | `deque` (default) | Binary Heap |
| Order of Removal | First inserted element | Highest-priority element |
| Access Function | `front()` | `top()` |
| Insertion Function | `push()` | `push()` |
| Removal Function | `pop()` | `pop()` |
| `pop()` Removes | Front element | Highest-priority element |
| Default Order | Insertion order | Largest element first (Max Heap) |
| Iterator Support | ❌ No | ❌ No |
| Indexing | ❌ No | ❌ No |
| `push()` Complexity | O(1) | O(log n) |
| `pop()` Complexity | O(1) | O(log n) |
| Top/Front Access | O(1) | O(1) |

---

# `queue`

A queue follows the **FIFO (First In, First Out)** rule.

The first element inserted is the first one removed.

Example

```cpp
queue<int> q;

q.push(10);
q.push(30);
q.push(20);
```

Visualization

```
Front                Back
 ↓                    ↓
10  →  30  →  20
```

Operations

```cpp
cout << q.front();   // 10

q.pop();             // Removes 10

cout << q.front();   // 30
```

Output

```
10
30
```

---

# `priority_queue`

A priority queue removes elements based on **priority**, not insertion order.

By default, the **largest element** has the highest priority.

Example

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(30);
pq.push(20);
```

Visualization

```
Top
 ↓
30
20
10
```

Operations

```cpp
cout << pq.top();   // 30

pq.pop();           // Removes 30

cout << pq.top();   // 20
```

Output

```
30
20
```

---

# Example Comparison

Insert elements in this order:

```cpp
10
30
20
```

## Queue

Insertion

```
10 → 30 → 20
```

Removal

```
10
30
20
```

---

## Priority Queue (Max Heap)

Insertion

```
10
30
20
```

Removal

```
30
20
10
```

Notice that the **largest value is removed first**, regardless of when it was inserted.

---

# Min Heap

To make the **smallest** element come first:

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

Now

```cpp
pq.push(10);
pq.push(30);
pq.push(20);
```

Removal

```
10
20
30
```

---

# Time Complexity

| Operation | `queue` | `priority_queue` |
|-----------|---------|------------------|
| `push()` | O(1) | O(log n) |
| `pop()` | O(1) | O(log n) |
| `front()` / `top()` | O(1) | O(1) |
| `size()` | O(1) | O(1) |
| `empty()` | O(1) | O(1) |

---

# Applications

## `queue`

- Breadth-First Search (BFS)
- CPU Scheduling
- Printer Queue
- Task Scheduling
- Order Processing

---

## `priority_queue`

- Dijkstra's Algorithm
- Prim's Algorithm
- Huffman Coding
- K Largest / K Smallest Elements
- Job Scheduling
- Event Simulation

---

# Memory Trick

## Queue

```
FIFO

First In
↓

First Out
```

Example

```
10 → 20 → 30

Remove

10
```

---

## Priority Queue

```
Highest Priority

Largest
↓

Removed First
```

Example

```
10 30 20

Remove

30
```

---

# Interview Summary

- `queue` follows **FIFO (First In, First Out)**.
- `priority_queue` removes the **highest-priority element** first.
- Default `priority_queue` is a **Max Heap**.
- Use `greater<int>` to create a **Min Heap**.
- `queue` operations are generally **O(1)**.
- `priority_queue` uses a **Binary Heap**, so `push()` and `pop()` are **O(log n)**.
- Neither container supports iterators or indexing.