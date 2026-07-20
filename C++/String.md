# C++ STL `string` Notes

# What is a String?

A **string** is a sequence of characters.

In C++, `std::string` is a class provided by the Standard Template Library (STL) that makes working with text much easier than using C-style character arrays.

Header File

```cpp
#include <string>
```

---

# Declaration

```cpp
string s;
```

Initialize

```cpp
string s = "Hello";
```

---

# Characteristics

- Dynamic size
- Stores characters
- Supports indexing
- Supports iterators
- Rich set of built-in functions
- Memory managed automatically

---

# Types of String

## Empty String

```cpp
string s;
```

Output

```
""
```

---

## Initialize with Text

```cpp
string s = "Hello";
```

---

## Copy String

```cpp
string s1 = "Hello";

string s2(s1);
```

or

```cpp
string s2 = s1;
```

---

## String with Repeated Characters

```cpp
string s(5,'A');
```

Output

```
AAAAA
```

---

## Character Array to String

```cpp
char str[] = "Hello";

string s(str);
```

---

## Substring Constructor

```cpp
string s = "Programming";

string sub(s,3,5);
```

Output

```
gramm
```

---

# Accessing Characters

## []

```cpp
string s="Hello";

cout<<s[1];
```

Output

```
e
```

Time Complexity

```
O(1)
```

---

## at()

```cpp
cout<<s.at(2);
```

Output

```
l
```

Difference

- `[]` does not check bounds.
- `at()` checks bounds.

---

## front()

```cpp
cout<<s.front();
```

Output

```
H
```

---

## back()

```cpp
cout<<s.back();
```

Output

```
o
```

---

# Capacity Functions

## size()

```cpp
cout<<s.size();
```

Output

```
5
```

Complexity

```
O(1)
```

---

## length()

Same as

```cpp
size()
```

Example

```cpp
cout<<s.length();
```

---

## empty()

```cpp
if(s.empty())
    cout<<"Empty";
```

---

## capacity()

```cpp
cout<<s.capacity();
```

Returns allocated memory.

---

## reserve()

```cpp
s.reserve(100);
```

Reserves memory.

---

## shrink_to_fit()

```cpp
s.shrink_to_fit();
```

Reduces unused memory.

---

# Modifying Functions

## append()

```cpp
string s="Hello";

s.append(" World");
```

Output

```
Hello World
```

---

## operator +=

```cpp
s += " World";
```

---

## push_back()

```cpp
s.push_back('!');
```

Output

```
Hello!
```

---

## pop_back()

```cpp
s.pop_back();
```

Removes last character.

---

## insert()

```cpp
string s="Helo";

s.insert(3,"l");
```

Output

```
Hello
```

---

## erase()

Remove by position

```cpp
string s="Hello";

s.erase(1,2);
```

Output

```
Hlo
```

---

## replace()

```cpp
string s="Hello";

s.replace(1,2,"abc");
```

Output

```
Habclo
```

---

## clear()

```cpp
s.clear();
```

Result

```
Empty String
```

---

## assign()

```cpp
string s;

s.assign("Programming");
```

---

## swap()

```cpp
string a="Hello";

string b="World";

a.swap(b);
```

Output

```
a = World

b = Hello
```

---

# Searching Functions

## find()

Returns first occurrence.

```cpp
string s="Programming";

cout<<s.find("gram");
```

Output

```
3
```

If not found

```
string::npos
```

---

## rfind()

Search from right.

```cpp
cout<<s.rfind("m");
```

---

## find_first_of()

```cpp
string s="abcdef";

cout<<s.find_first_of("de");
```

Output

```
3
```

---

## find_last_of()

```cpp
cout<<s.find_last_of("a");
```

---

## find_first_not_of()

```cpp
string s="111223";

cout<<s.find_first_not_of("1");
```

Output

```
3
```

---

## find_last_not_of()

Returns last character not matching.

---

# Compare Functions

## compare()

```cpp
string a="abc";

string b="abc";

cout<<a.compare(b);
```

Output

```
0
```

Return Values

```
0

Equal

<0

First string smaller

>0

First string greater
```

---

# Substring

## substr()

```cpp
string s="Programming";

cout<<s.substr(3,5);
```

Output

```
gramm
```

---

# Conversion Functions

## stoi()

String to Integer

```cpp
string s="123";

int x=stoi(s);
```

---

## stol()

String to long

```cpp
long x=stol(s);
```

---

## stoll()

String to long long

```cpp
long long x=stoll(s);
```

---

## stof()

```cpp
float x=stof("3.14");
```

---

## stod()

```cpp
double x=stod("3.14");
```

---

## to_string()

```cpp
int x=100;

string s=to_string(x);
```

Output

```
"100"
```

---

# Iterators

## begin()

```cpp
auto it=s.begin();
```

---

## end()

```cpp
auto it=s.end();
```

---

## rbegin()

```cpp
auto it=s.rbegin();
```

---

## rend()

```cpp
auto it=s.rend();
```

---

# Traversing String

## Using Index

```cpp
for(int i=0;i<s.size();i++)
{
    cout<<s[i];
}
```

---

## Range-Based Loop

```cpp
for(char c:s)
{
    cout<<c;
}
```

---

## Iterator

```cpp
for(auto it=s.begin();it!=s.end();it++)
{
    cout<<*it;
}
```

---

## Reverse Iterator

```cpp
for(auto it=s.rbegin();it!=s.rend();it++)
{
    cout<<*it;
}
```

---

# Useful Algorithms

Header

```cpp
#include <algorithm>
```

---

## sort()

```cpp
sort(s.begin(),s.end());
```

Example

```
dcab

↓

abcd
```

Complexity

```
O(n log n)
```

---

## reverse()

```cpp
reverse(s.begin(),s.end());
```

---

## count()

```cpp
count(s.begin(),s.end(),'a');
```

---

## find()

```cpp
find(s.begin(),s.end(),'a');
```

---

## transform()

Convert to uppercase

```cpp
transform(s.begin(),s.end(),s.begin(),::toupper);
```
 Read every character from `s`, stop at the end, write the result back into `s`, and convert each character to lowercase.
 
Convert to lowercase

```cpp
transform(s.begin(),s.end(),s.begin(),::tolower);
```

---

## unique()

Remove consecutive duplicates

```cpp
sort(s.begin(),s.end());

s.erase(unique(s.begin(),s.end()),s.end());
```

---

# Operators

## Concatenation

```cpp
string a="Hello";

string b="World";

cout<<a+b;
```

Output

```
HelloWorld
```

---

## Equality

```cpp
if(a==b)
```

---

## Less Than

```cpp
if(a<b)
```

Lexicographical comparison.

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| [] | O(1) |
| at() | O(1) |
| front() | O(1) |
| back() | O(1) |
| size() | O(1) |
| length() | O(1) |
| empty() | O(1) |
| append() | O(n) |
| push_back() | O(1) amortized |
| pop_back() | O(1) |
| insert() | O(n) |
| erase() | O(n) |
| replace() | O(n) |
| substr() | O(k) |
| find() | O(n) |
| compare() | O(n) |
| clear() | O(n) |
| swap() | O(1) |
| sort() | O(n log n) |
| reverse() | O(n) |
| count() | O(n) |
| transform() | O(n) |

---

# Common Interview Questions

## Difference between size() and length()

```
No Difference

Both return the number of characters.
```

---

## Difference between [] and at()

| [] | at() |
|------|------|
| No bounds checking | Bounds checking |
| Faster | Slightly slower |
| Undefined behavior if index is invalid | Throws `std::out_of_range` |

---

## Difference between find() and std::find()

| string::find() | std::find() |
|----------------|-------------|
| Searches for substring or character | Searches using iterators |
| Returns index (`size_t`) | Returns iterator |

---

# Frequently Used Functions

- size()
- length()
- empty()
- clear()
- append()
- push_back()
- pop_back()
- insert()
- erase()
- replace()
- substr()
- find()
- compare()
- at()
- front()
- back()
- begin()
- end()
- rbegin()
- rend()

---

# Interview Tips

- `std::string` is a dynamic character container.
- Supports random access (`[]`, `at()`).
- `find()` returns the **index** of the first match or `string::npos` if not found.
- `substr(pos, len)` extracts part of a string.
- Use `transform()` for uppercase/lowercase conversion.
- Use `stoi()`, `stol()`, `stod()`, etc., to convert strings to numeric types.
- Use `to_string()` to convert numbers to strings.
- Strings support iterators, making them compatible with STL algorithms like `sort()`, `reverse()`, and `count()`.
# Time Complexity of C++ `std::string` Operations

| Function / Operation | Time Complexity | Notes |
|----------------------|-----------------|-------|
| `[]` | **O(1)** | Access character by index |
| `at()` | **O(1)** | Bounds checking included |
| `front()` | **O(1)** | First character |
| `back()` | **O(1)** | Last character |
| `size()` | **O(1)** | Number of characters |
| `length()` | **O(1)** | Same as `size()` |
| `empty()` | **O(1)** | Checks whether string is empty |
| `capacity()` | **O(1)** | Allocated storage |
| `max_size()` | **O(1)** | Maximum possible size |
| `data()` | **O(1)** | Pointer to character array |
| `c_str()` | **O(1)** | Pointer to null-terminated string |
| `begin()` | **O(1)** | Iterator to first character |
| `end()` | **O(1)** | Iterator after last character |
| `rbegin()` | **O(1)** | Reverse iterator |
| `rend()` | **O(1)** | Reverse end iterator |
| `push_back()` | **O(1)** amortized | May reallocate |
| `pop_back()` | **O(1)** | Removes last character |
| `append()` | **O(n)** | Appends another string |
| `operator+=` | **O(n)** | Concatenation |
| `operator+` | **O(n + m)** | Creates a new string |
| `insert()` | **O(n)** | Shifts characters |
| `erase()` | **O(n)** | Shifts remaining characters |
| `replace()` | **O(n)** | Replace part of string |
| `clear()` | **O(n)** | Removes all characters |
| `assign()` | **O(n)** | Replaces entire content |
| `swap()` | **O(1)** | Swaps internal buffers |
| `resize()` | **O(n)** | May add/remove characters |
| `reserve()` | **O(n)** | If reallocation occurs |
| `shrink_to_fit()` | **O(n)** | May reallocate |
| `copy()` | **O(k)** | Copies `k` characters |
| `substr()` | **O(k)** | Copies substring of length `k` |
| `compare()` | **O(min(n,m))** | Compares until mismatch/end |
| `find(char)` | **O(n)** | Linear search |
| `find(string)` | **O(n × m)** worst case | Searches substring |
| `rfind()` | **O(n)** | Reverse search |
| `find_first_of()` | **O(n × m)** | First matching character |
| `find_last_of()` | **O(n × m)** | Last matching character |
| `find_first_not_of()` | **O(n × m)** | First non-matching character |
| `find_last_not_of()` | **O(n × m)** | Last non-matching character |
| `stoi()` | **O(n)** | String → Integer |
| `stol()` | **O(n)** | String → Long |
| `stoll()` | **O(n)** | String → Long Long |
| `stof()` | **O(n)** | String → Float |
| `stod()` | **O(n)** | String → Double |
| `to_string()` | **O(n)** | Number → String |

---

# Common STL Algorithms Used with String

Header

```cpp
#include <algorithm>
```

| Algorithm | Time Complexity |
|------------|-----------------|
| `sort()` | **O(n log n)** |
| `reverse()` | **O(n)** |
| `find()` | **O(n)** |
| `count()` | **O(n)** |
| `binary_search()` *(sorted string)* | **O(log n)** |
| `lower_bound()` *(sorted string)* | **O(log n)** |
| `upper_bound()` *(sorted string)* | **O(log n)** |
| `min_element()` | **O(n)** |
| `max_element()` | **O(n)** |
| `unique()` | **O(n)** |
| `transform()` | **O(n)** |
| `rotate()` | **O(n)** |
| `next_permutation()` | **O(n)** |
| `prev_permutation()` | **O(n)** |

---

# Traversing a String

| Method | Complexity |
|---------|------------|
| Index (`s[i]`) | **O(n)** |
| Range-based loop | **O(n)** |
| Iterator | **O(n)** |
| Reverse iterator | **O(n)** |

---

# Memory Complexity

| Operation | Space Complexity |
|------------|------------------|
| `substr()` | **O(k)** |
| `operator+` | **O(n+m)** |
| `append()` | **O(1)** extra (unless reallocation) |
| `find()` | **O(1)** |
| `compare()` | **O(1)** |
| `sort()` | **O(log n)** (stack space) |
| `reverse()` | **O(1)** |

---

# Interview Summary

| Operation         | Complexity              |
| ----------------- | ----------------------- |
| Access Character  | **O(1)**                |
| Insert Character  | **O(n)**                |
| Delete Character  | **O(n)**                |
| Append Character  | **O(1)** amortized      |
| Search Character  | **O(n)**                |
| Search Substring  | **O(n × m)** worst case |
| Compare Strings   | **O(min(n,m))**         |
| Extract Substring | **O(k)**                |
| Sort String       | **O(n log n)**          |
| Reverse String    | **O(n)**                |