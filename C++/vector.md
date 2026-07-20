# C++ STL Vector

## What is a Vector?

A **vector** is a dynamic array provided by the C++ Standard Template Library (STL). Unlike a normal array, a vector can automatically increase or decrease its size during runtime.

Header File:

```cpp
#include <vector>
using namespace std;
```

Declaration:

```cpp
vector<int> v;
```

---

# Why Use Vector Instead of Array?

| Array | Vector |
|-------|--------|
| Fixed size | Dynamic size |
| Size cannot change | Size can grow or shrink |
| No built-in functions | Many built-in functions |
| Less flexible | More flexible |
| Manual memory management | Automatic memory management |

Example:

```cpp
int arr[5];
```

```cpp
vector<int> v;
```

The vector automatically allocates more memory when needed.

---

# Internal Working of Vector

A vector stores elements in **contiguous memory**, just like an array.

Example:

```
Index : 0   1   2   3
Value : 10  20  30  40
```

Memory Layout

```
+----+----+----+----+
|10  |20  |30  |40  |
+----+----+----+----+
```

Because memory is contiguous:

- Random access is very fast.
- Cache performance is excellent.
- Access by index is O(1).

---

# Types of Vector

## 1. Empty Vector

```cpp
vector<int> v;
```

Creates an empty vector.

Size = 0

Capacity = 0

---

## 2. Vector with Fixed Size

```cpp
vector<int> v(5);
```

Output

```
0 0 0 0 0
```

Five integers are created and initialized with 0.

---

## 3. Vector with Size and Initial Value

```cpp
vector<int> v(5, 10);
```

Output

```
10 10 10 10 10
```

Five elements are created.

Every element contains 10.

---

## 4. Vector Using Initializer List

```cpp
vector<int> v = {1,2,3,4,5};
```

Output

```
1 2 3 4 5
```

---

## 5. Copy Vector

```cpp
vector<int> a = {1,2,3};

vector<int> b(a);
```

or

```cpp
vector<int> b = a;
```

Output

```
1 2 3
```

---

## 6. Two-Dimensional Vector

```cpp
vector<vector<int>> matrix;
```

Example

```cpp
vector<vector<int>> matrix =
{
    {1,2,3},
    {4,5,6},
    {7,8,9}
};
```

Output

```
1 2 3
4 5 6
7 8 9
```

---

## 7. Three-Dimensional Vector

```cpp
vector<vector<vector<int>>> cube;
```

---

## 8. Vector of Pair

```cpp
vector<pair<int,int>> vp;
```

Example

```cpp
vp.push_back({1,100});
vp.push_back({2,200});
```

Output

```
(1,100)
(2,200)
```

---

## 9. Vector of String

```cpp
vector<string> names;
```

Example

```cpp
names.push_back("Alice");
names.push_back("Bob");
```

---

# Important Properties

## size()

Returns the number of elements.

```cpp
vector<int> v={1,2,3};

cout<<v.size();
```

Output

```
3
```

Time Complexity

```
O(1)
```

---

## capacity()

Returns allocated memory.

```cpp
vector<int> v;

v.push_back(10);

cout<<v.capacity();
```

Example

```
Size = 1
Capacity = 1
```

After more insertions

```
Size = 5
Capacity = 8
```

Capacity is always greater than or equal to size.

Time Complexity

```
O(1)
```

---

## max_size()

Returns the maximum possible size.

```cpp
cout<<v.max_size();
```

Time Complexity

```
O(1)
```

---

## empty()

Checks whether vector is empty.

```cpp
if(v.empty())
    cout<<"Empty";
```

Output

```
true
```

Time Complexity

```
O(1)
```

---

# Access Functions

## operator[]

```cpp
vector<int> v={10,20,30};

cout<<v[1];
```

Output

```
20
```

Time Complexity

```
O(1)
```

---

## at()

```cpp
cout<<v.at(2);
```

Output

```
30
```

Difference

- at() checks bounds.
- [] does not.

Time Complexity

```
O(1)
```

---

## front()

Returns first element.

```cpp
cout<<v.front();
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

## back()

Returns last element.

```cpp
cout<<v.back();
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

## data()

Returns pointer to first element.

```cpp
int *ptr = v.data();
```

Time Complexity

```
O(1)
```

---

# Iterator Functions

## begin()

Returns iterator pointing to first element.

```cpp
auto it=v.begin();
```

Time Complexity

```
O(1)
```

---

## end()

Returns iterator after last element.

```cpp
auto it=v.end();
```

Time Complexity

```
O(1)
```

---

## rbegin()

Returns reverse iterator.

```cpp
auto it=v.rbegin();
```

---

## rend()

Returns reverse end iterator.

```cpp
auto it=v.rend();
```

---

# Modifier Functions

## push_back()

Adds element at end.

```cpp
vector<int> v;

v.push_back(10);
v.push_back(20);
```

Output

```
10 20
```

Time Complexity

```
Average : O(1)

Worst : O(n)
```

---

## pop_back()

Removes last element.

```cpp
v.pop_back();
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

## insert()

Insert single element.

```cpp
vector<int> v={1,2,4};

v.insert(v.begin()+2,3);
```

Output

```
1 2 3 4
```

Insert multiple copies

```cpp
v.insert(v.begin(),3,100);
```

Output

```
100 100 100 1 2 3 4
```

Time Complexity

```
O(n)
```

---

## erase()

Remove one element.

```cpp
v.erase(v.begin()+2);
```

Output

```
1 2 4
```

Remove range

```cpp
v.erase(v.begin()+1,v.begin()+4);
```

Time Complexity

```
O(n)
```

---

## clear()

Deletes all elements.

```cpp
v.clear();
```

Output

```
Size = 0
```

Capacity remains unchanged.

Time Complexity

```
O(n)
```

---

## resize()

Increase size.

```cpp
vector<int> v={1,2,3};

v.resize(5);
```

Output

```
1 2 3 0 0
```

Decrease size

```cpp
v.resize(2);
```

Output

```
1 2
```

Time Complexity

```
O(n)
```

---

## reserve()

Reserve memory.

```cpp
v.reserve(100);
```

Capacity becomes

```
100
```

Time Complexity

```
O(n) if reallocation happens

Otherwise O(1)
```

---

## shrink_to_fit()

Reduce unused capacity.

```cpp
v.shrink_to_fit();
```

Time Complexity

```
O(n)
```

---

## assign()

Replace all elements.

```cpp
vector<int> v;

v.assign(5,7);
```

Output

```
7 7 7 7 7
```

Time Complexity

```
O(n)
```

---

## swap()

Swap two vectors.

```cpp
vector<int> a={1,2};

vector<int> b={10,20};

a.swap(b);
```

Output

```
a

10 20

b

1 2
```

Time Complexity

```
O(1)
```

---

# Traversing a Vector

## Using Index

```cpp
for(int i=0;i<v.size();i++)
{
    cout<<v[i]<<" ";
}
```

---

## Using Range-Based Loop

```cpp
for(int x:v)
{
    cout<<x<<" ";
}
```

---

## Using Iterator

```cpp
for(auto it=v.begin();it!=v.end();it++)
{
    cout<<*it<<" ";
}
```

---

## Using Reverse Iterator

```cpp
for(auto it=v.rbegin();it!=v.rend();it++)
{
    cout<<*it<<" ";
}
```

---

# Common Algorithms Used with Vector

Include

```cpp
#include <algorithm>
```

---

## sort()

```cpp
sort(v.begin(),v.end());
```

Time Complexity

```
O(n log n)
```

---

## reverse()

```cpp
reverse(v.begin(),v.end());
```

Time Complexity

```
O(n)
```

---

## find()

```cpp
auto it=find(v.begin(),v.end(),10);
```

Time Complexity

```
O(n)
```

---

## count()

```cpp
count(v.begin(),v.end(),5);
```

Time Complexity

```
O(n)
```

---

## binary_search()

Vector must be sorted.

```cpp
binary_search(v.begin(),v.end(),20);
```

Time Complexity

```
O(log n)
```

---

## lower_bound()

Returns first occurrence position.

```cpp
lower_bound(v.begin(),v.end(),10);
```

Time Complexity

```
O(log n)
```

---

## upper_bound()

Returns position after last occurrence.

```cpp
upper_bound(v.begin(),v.end(),10);
```

Time Complexity

```
O(log n)
```

---

## min_element()

```cpp
min_element(v.begin(),v.end());
```

Time Complexity

```
O(n)
```

---

## max_element()

```cpp
max_element(v.begin(),v.end());
```

Time Complexity

```
O(n)
```

---

## accumulate()

Header

```cpp
#include <numeric>
```

Example

```cpp
accumulate(v.begin(),v.end(),0);
```

Time Complexity

```
O(n)
```

---

# Complete Time Complexity Table

| Operation | Complexity |
|------------|------------|
| Access using [] | O(1) |
| Access using at() | O(1) |
| front() | O(1) |
| back() | O(1) |
| size() | O(1) |
| capacity() | O(1) |
| empty() | O(1) |
| push_back() | O(1) amortized |
| pop_back() | O(1) |
| insert() at end | O(1) amortized |
| insert() at beginning | O(n) |
| insert() in middle | O(n) |
| erase() at end | O(1) |
| erase() at beginning | O(n) |
| erase() in middle | O(n) |
| clear() | O(n) |
| resize() | O(n) |
| reserve() | O(n) (if reallocation occurs) |
| assign() | O(n) |
| swap() | O(1) |
| find() | O(n) |
| count() | O(n) |
| sort() | O(n log n) |
| reverse() | O(n) |
| binary_search() | O(log n) |
| lower_bound() | O(log n) |
| upper_bound() | O(log n) |
| min_element() | O(n) |
| max_element() | O(n) |
| accumulate() | O(n) |

---

# Most Frequently Used Vector Functions in Interviews

- push_back()
- pop_back()
- size()
- capacity()
- empty()
- clear()
- front()
- back()
- begin()
- end()
- rbegin()
- rend()
- insert()
- erase()
- resize()
- reserve()
- assign()
- swap()
- operator[]
- at()

---

# Most Frequently Used Algorithms with Vector

- sort()
- reverse()
- find()
- count()
- binary_search()
- lower_bound()
- upper_bound()
- min_element()
- max_element()
- accumulate()

---

# Interview Tips

- Vector stores elements in contiguous memory.
- Random access is O(1).
- Inserting or deleting from the beginning or middle is O(n).
- Adding at the end is O(1) on average (amortized).
- `capacity()` is the allocated storage.
- `size()` is the number of stored elements.
- Use `reserve()` when you know the number of elements in advance to reduce reallocations.
- Prefer `at()` when you need bounds checking and `operator[]` when performance is critical.