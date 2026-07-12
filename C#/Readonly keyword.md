 Yes, that is exactly right!

The **`readonly`** keyword in C# ensures that a field can only be assigned **once**, and that assignment must happen **either where you declare it or inside a constructor**.

Once the constructor finishes executing, you cannot change the value of that field anywhere else in your code

The 2 Allowed Places for Assignment


```
public class BankAccount
{
    // Option 1: Assigned directly at the time of declaration
    private readonly string _routingNumber = "123456789"; 
    
    private readonly string _accountNumber;

    public BankAccount(string accountNumber)
    {
        // Option 2: Assigned inside the constructor
        _accountNumber = accountNumber; 
    }

    public void UpdateAccount(string newNum)
    {
        // ERROR: This will not compile! 
        // You cannot assign to a readonly field outside a constructor.
        _accountNumber = newNum; 
    }
}
```

Use code with caution.

An Important Exception: Reference Types

There is one tricky rule to remember. If your `readonly` variable is an **object or a list** (a reference type), `readonly` only stops you from replacing the _entire object_ with a new one. It does **not** stop you from changing the data _inside_ that object.



```
public class Group
{
    // The list reference itself is readonly
    public readonly List<string> Members = new List<string>();

    public void Test()
    {
        // Allowed: You are modifying the data INSIDE the list
        Members.Add("Alice"); 

        // ERROR: This will not compile! You cannot assign a brand new list.
        Members = new List<string>(); 
    }
}
```

Use code with caution.

Quick Comparison: `readonly` vs `const`

Since you are mastering C# keywords, it helps to see how it differs from `const` (constant):
- **`const`**: Must be known at **compile-time**. You have to hardcode the value right in the text file (e.g., `const double Pi = 3.14;`).
- **`readonly`**: Can be determined at **runtime**. You can read a value from a database or web request, pass it into the constructor, and lock it down forever.

