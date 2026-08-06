# C# Fields vs Properties

## What is a Field?

A **field** is a variable that belongs to a class. It is the actual storage location for data.

```csharp
class Person
{
    private string name;
    private int age;
}
```

### Characteristics

- Stores the actual value.
- Usually declared `private`.
- Cannot have `get` or `set`.
- Accessed directly inside the class.

Example:

```csharp
class Person
{
    public string Name;
}

Person p = new Person();

p.Name = "John";
Console.WriteLine(p.Name);
```

Here, `"John"` is stored **directly** in the field.

---

# What is a Property?

A **property** provides controlled access to data. It acts like a bridge between outside code and the field.

```csharp
class Person
{
    private string name;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }
}
```

### Characteristics

- Does **not usually** store data itself.
- Controls how data is read and written.
- Can contain validation or other logic.
- Uses `get` and `set`.

---

# Difference Between Field and Property

| Field | Property |
|--------|----------|
| Stores data | Controls access to data |
| Variable | Class member with `get`/`set` |
| Cannot contain logic | Can contain logic |
| No `get`/`set` | Uses `get` and/or `set` |
| Usually `private` | Usually `public` |

---

# How `get` Works

```csharp
private string name;

public string Name
{
    get
    {
        return name;
    }

    set
    {
        name = value;
    }
}
```

When you write

```csharp
Console.WriteLine(person.Name);
```

C# executes

```csharp
return name;
```

---

# How `set` Works

When you write

```csharp
person.Name = "John";
```

C# executes

```csharp
name = value;
```

where

```text
value = "John"
```

So it becomes

```csharp
name = "John";
```

---

# Auto-Implemented Property

```csharp
public string Name { get; set; }
```

The compiler automatically creates a hidden field.

It is equivalent to

```csharp
private string _name;

public string Name
{
    get
    {
        return _name;
    }

    set
    {
        _name = value;
    }
}
```

The hidden field cannot be accessed directly.

---

# Why Use Properties?

Suppose you want age to never be negative.

```csharp
private int age;

public int Age
{
    get
    {
        return age;
    }

    set
    {
        if (value >= 0)
        {
            age = value;
        }
    }
}
```

Usage

```csharp
person.Age = 20;
```

Stored

```text
age = 20
```

Now

```csharp
person.Age = -5;
```

Validation fails.

Result

```text
age remains 20
```

The invalid value is **not stored**.

---

# Property as a Gatekeeper

```text
Assign Value
      │
      ▼
 Property (set)
      │
Is value valid?
   /        \
 Yes         No
 │            │
 ▼            ▼
Store      Reject
```

The property decides whether the value should be stored.

---

# Do Properties Need a Field?

## Auto Property

```csharp
public int Age { get; set; }
```

❌ No.

The compiler creates a hidden field automatically.

---

## Property with Custom Logic

```csharp
private int age;

public int Age
{
    get
    {
        return age;
    }

    set
    {
        if (value >= 0)
            age = value;
    }
}
```

✅ Yes.

You must create your own field because the compiler's hidden field is not available inside custom `get`/`set` blocks.

---

# Field vs Auto Property

### Field

```csharp
public string Name;
```

```text
person.Name = "John"

↓

Stored directly
```

---

### Auto Property

```csharp
public string Name { get; set; }
```

```text
person.Name = "John"

↓

Calls set

↓

Stores in hidden field
```

---

# Read-Only Property

```csharp
public bool IsAdult => Age >= 18;
```

Equivalent to

```csharp
public bool IsAdult
{
    get
    {
        return Age >= 18;
    }
}
```

This property does **not** store a value.

Every time it is accessed, it calculates

```csharp
Age >= 18
```

---

# When to Use Fields

Use fields for **internal storage**.

```csharp
private string name;
private int age;
```

---

# When to Use Properties

Expose data using properties.

```csharp
public string Name { get; set; }

public int Age { get; set; }
```

If validation or logic is required

```csharp
private int age;

public int Age
{
    get
    {
        return age;
    }

    set
    {
        if (value >= 0)
            age = value;
    }
}
```

---

# Best Practices

✔ Keep fields `private`.

✔ Expose data using `public` properties.

✔ Use auto-properties when no custom logic is needed.

✔ Use a private field + property when validation or custom logic is required.

✔ Never expose mutable data as public fields.

---

# Quick Revision

### Field

- Stores data
- Variable
- No `get`/`set`
- Usually private

### Property

- Controls access to data
- Uses `get`/`set`
- Can validate
- Usually public

### Auto Property

```csharp
public int Age { get; set; }
```

- Hidden field created automatically.
- Use when no custom logic is needed.

### Property with Validation

```csharp
private int age;

public int Age
{
    get => age;
    set
    {
        if (value >= 0)
            age = value;
    }
}
```

- Uses your own field.
- Property decides whether to store the value.

---

# Easy Memory Trick

```
Field = Box 📦
(Property has the data)

Property = Door 🚪
(Controls who can enter or leave the box)

Object = House 🏠
(Contains both the box and the door)
```

```
Outside Code
      │
      ▼
+--------------+
|  Property    |  ← Validation / Logic
+--------------+
       │
       ▼
+--------------+
|    Field     |  ← Actual Storage
+--------------+
```