# Summary

| Feature                             | Assignment (`=`) | `clone`                                                |
| ----------------------------------- | ---------------- | ------------------------------------------------------ |
| Creates a new object                | ❌ No             | ✅ Yes                                                  |
| Variables point to the same object  | ✅ Yes            | ❌ No                                                   |
| Changes affect the original         | ✅ Yes            | ❌ No (except shared nested objects in a shallow clone) |
| Copies nested objects automatically | ❌ Not applicable | ❌ No (shallow copy by default)                         |
| Can customize behavior              | ❌ No             | ✅ Yes, using `__clone()`                               |
### Object Copy (Assignment)

**Definition:**  
Object copy (using the assignment operator `=`) is the process of copying the **object reference (handle)** from one variable to another. No new object is created; both variables refer to the same object in memory.

```
$user1 = new User();
$user2 = $user1;
```

- ✅ No new object is created.
- ✅ Both variables point to the same object.
- ✅ Changes through one variable are visible through the other.

---

### Object Cloning

**Definition:**  
Object cloning is the process of creating a **new object** by copying the properties of an existing object using the `clone` keyword. The new object has its own identity in memory.

```
$user1 = new User();
$user2 = clone $user1;
```

- ✅ A new object is created.
- ✅ The original and cloned objects are independent.
- ✅ By default, cloning is **shallow** (nested objects are still shared unless you implement `__clone()`).

---

## Quick Comparison

|Object Copy (Assignment)|Object Cloning|
|---|---|
|Copies the object reference|Creates a new object|
|Uses `=`|Uses `clone`|
|No new object is created|A new object is created|
|Both variables refer to the same object|Each variable refers to a different object|
|Changes affect both variables|Changes affect only the modified object|

**Remember:**

- `=` → **Copies the reference (handle), not the object itself.**
- `clone` → **Copies the object by creating a new instance.**