| Feature            | `set`                             | `unordered_set`          |
| ------------------ | --------------------------------- | ------------------------ |
| Internal Structure | Red-Black Tree                    | Hash Table               |
| Order              | **Sorted** (ascending by default) | **No order**             |
| Duplicate Elements | ❌ Not allowed                     | ❌ Not allowed            |
| Insertion          | O(log n)                          | O(1) average, O(n) worst |
| Deletion           | O(log n)                          | O(1) average, O(n) worst |
| Search (`find`)    | O(log n)                          | O(1) average, O(n) worst |
| `lower_bound()`    | ✅ Available                       | ❌ Not available          |
| `upper_bound()`    | ✅ Available                       | ❌ Not available          |
| Iterate Elements   | Sorted order                      | Random/unspecified order |
| Uses Hash Function | ❌ No                              | ✅ Yes                    |
## Memory Trick

- **`set` → Sorted → Tree → O(log n)**
- **`unordered_set` → Unsorted → Hash Table → O(1) average**