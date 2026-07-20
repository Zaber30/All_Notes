| Function             | Returns Iterator To                              | Containers                  |
| -------------------- | ------------------------------------------------ | --------------------------- |
| `find()`             | Found element or `end()`                         | All containers              |
| `find_if()`          | First element satisfying condition               | All                         |
| `find_if_not()`      | First element not satisfying condition           | All                         |
| `lower_bound()`      | First element ≥ value                            | Sorted containers/ranges    |
| `upper_bound()`      | First element > value                            | Sorted containers/ranges    |
| `equal_range()`      | Pair of iterators (`lower_bound`, `upper_bound`) | Sorted containers           |
| `binary_search()`    | ❌ Returns `bool`                                 | Sorted containers           |
| `unique()`           | New logical end                                  | Sequence containers         |
| `remove()`           | New logical end                                  | Sequence containers         |
| `remove_if()`        | New logical end                                  | Sequence containers         |
| `partition()`        | Partition point                                  | Sequence containers         |
| `stable_partition()` | Partition point                                  | Sequence containers         |
| `min_element()`      | Smallest element                                 | All                         |
| `max_element()`      | Largest element                                  | All                         |
| `minmax_element()`   | Pair of iterators                                | All                         |
| `adjacent_find()`    | First adjacent duplicate                         | All                         |
| `search()`           | Beginning of matched subsequence                 | All                         |
| `search_n()`         | First occurrence of `n` consecutive values       | All                         |
| `next()`             | Next iterator                                    | All                         |
| `prev()`             | Previous iterator                                | Bidirectional/Random Access |
| `begin()`            | First element                                    | All                         |
| `end()`              | One past last element                            | All                         |
| `rbegin()`           | Last element                                     | Reverse                     |
| `rend()`             | Before first element (reverse)                   | Reverse                     |