C# and .NET provide a rich set of collection types for storing and managing groups of objects. Collections are primarily divided into:

- **Non-generic** (System.Collections) — Store object, less type-safe, older style (avoid in new code).
- **Generic** (System.Collections.Generic) — Type-safe, preferred for performance and safety.
- **Concurrent** (System.Collections.Concurrent) — Thread-safe for multi-threaded scenarios.
- **Immutable** (System.Collections.Immutable) — Thread-safe, unchangeable after creation.
- **Specialized** — Like ObservableCollection<T> for data binding.

All collections implement IEnumerable / IEnumerable<T> for iteration.

1. Core Generic Collections (`System.Collections.Generic`)

These are the foundational, general-purpose collections utilized in everyday C# programming. They are **not thread-safe** for concurrent write operations.

- **`List<T>`**: Resizable array providing fast indexed access by integer position.
- **`Dictionary<TKey, TValue>`**: Highly optimized hash table mapping unique keys to values.
- **`HashSet<T>`**: High-performance mathematical set that strictly enforces unique elements.
- **`SortedList<TKey, TValue>`**: Key-value pairs sorted automatically by key; uses less memory than `SortedDictionary`.
- **`SortedDictionary<TKey, TValue>`**: Key-value pairs sorted by key; offers faster insertion and removal for large datasets.
- **`SortedSet<T>`**: Distinct, sorted collection of elements maintained in a binary search tree structure.
- **`Queue<T>`**: First-In, First-Out (FIFO) sequential structure.
- **`Stack<T>`**: Last-In, First-Out (LIFO) sequential structure.
- **`LinkedList<T>`**: Doubly-linked list enabling fast insertion or removal anywhere in the sequence. [[1](https://www.telerik.com/blogs/aspnet-core-basics-data-structures-part-1), [2](https://medium.com/@cnkumar28/collections-in-c-from-basic-to-advanced-7a646d2895be), [3](https://www.telerik.com/blogs/aspnet-core-basics-data-structures-part-1), [4](https://www.telerik.com/blogs/aspnet-core-basics-data-structures-part-1)]

### No, there are no other primary, standalone data structure classes in the core `System.Collections.Generic` namespace beside the 9 you listed.

However, Microsoft includes **three specialized helper types** inside that exact same core namespace to assist with read-only views, unique keys, and structural lookups.

The 3 Core Helper Structures

1. `KeyValuePair<TKey, TValue>` [[1](https://www.acte.in/dictionary-collection-in-c-tutorial)]

This is a lightweight structure (struct) rather than a full collection class. It represents a single key-value pair. Whenever you iterate through a `Dictionary<TKey, TValue>` using a `foreach` loop, this is the exact data type returned on each step.

2. `ReadOnlyDictionary<TKey, TValue>`

This acts as a completely immutable, read-only wrapper around a standard mutable `Dictionary`. If you want to allow external code to read your dictionary cache but want to strictly block them from adding or deleting items, you wrap it in this structure. [[1](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentdictionary-2?view=net-10.0), [2](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9/libraries)]

3. `PriorityQueue<TElement, TPriority>`

Added in modern .NET (.NET 6), this is a unique collection found directly in `System.Collections.Generic`. Unlike a standard `Queue<T>` which processes items strictly by time of arrival (FIFO), a `PriorityQueue` removes items based on a designated **priority value** you assign to them (lowest priority values are dequeued first). [[1](https://www.geeksforgeeks.org/c-sharp/c-priorityqueue/), [2](https://medium.com/@iamprovidence/too-many-collections-in-c-a6a23b636119), [3](https://www.scribd.com/document/560091325/Unit4-Collections-Notes)]

---


2. Thread-Safe Concurrent Collections (`System.Collections.Concurrent`)

Designed for **multi-threaded scenarios**, these collections use granular locking and lock-free algorithms to maximize performance when accessed simultaneously by multiple threads.

- **`ConcurrentDictionary<TKey, TValue>`**: Thread-safe implementation of key-value lookups with atomic update operations.
- **`ConcurrentQueue<T>`**: Thread-safe FIFO queue supporting concurrent producers and consumers.
- **`ConcurrentStack<T>`**: Thread-safe LIFO stack for concurrent multi-threaded execution.
- **`ConcurrentBag<T>`**: Unordered, thread-safe collection optimized for scenarios where the same thread both produces and consumes data.
- **`BlockingCollection<T>`**: Thread-safe wrapper that blocks consumer threads if empty, or producer threads if a capacity limit is reached.

---

3. Immutable Collections (`System.Collections.Immutable`)

These collections are **completely unmodifiable** once created. Any mutation operation (like adding an element) returns a brand new collection instance while sharing underlying memory structures for efficiency. They are inherently thread-safe. [[1](https://medium.com/@cnkumar28/collections-in-c-from-basic-to-advanced-7a646d2895be)]

- **`ImmutableList<T>`**: Array-like sequence that cannot be changed after creation.
- **`ImmutableDictionary<TKey, TValue>`**: Immutable key-value lookup map.
- **`ImmutableHashSet<T>`**: Fixed, immutable mathematical set of unique elements.
- **`ImmutableSortedDictionary<TKey, TValue>`**: Immutable key-value store kept in sorted order.
- **`ImmutableSortedSet<T>`**: Immutable set of sorted elements.
- **`ImmutableQueue<T>`**: Immutable FIFO collection structure.
- **`ImmutableStack<T>`**: Immutable LIFO collection structure.
- **`ImmutableArray<T>`**: Ultra-low overhead, immutable wrapper strictly wrapping a traditional C# array.

---

4. Specialized & UI-Binding Collections

These collections support specialized behaviors, such as notifying user interfaces when data changes or acting as read-only views.

- **`ReadOnlyCollection<T>`** (`System.Collections.ObjectModel`): Direct read-only wrapper around an existing mutable `List<T>`.
- **`ObservableCollection<T>`** (`System.Collections.ObjectModel`): Emits notification events whenever items are added, removed, or refreshed; vital for data binding in WPF, MAUI, and Avalonia UI applications.
- **`KeyedCollection<TKey, TItem>`** (`System.Collections.ObjectModel`): Hybrid collection that acts as an indexable list while allowing fast key lookups extracted directly from the items themselves.

### What does **"thread-safe"** mean?

**Thread-safe** means that a piece of code, class, or data structure can be safely used by **multiple threads at the same time** without causing problems like:

- Crashing
- Data corruption
- Wrong results
- Race conditions

### Why is it important?

In modern applications, programs often run multiple threads simultaneously (for example: UI thread + background workers, web servers handling many requests, parallel processing, etc.).

If two or more threads try to modify the **same collection** (or any shared data) at the same time without proper protection, unexpected bugs can occur.