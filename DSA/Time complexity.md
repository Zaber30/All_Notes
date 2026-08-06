Definition: **Time complexity** is a measure of how much longer a program takes to finish as your input data grows bigger.

It does not measure the actual execution time in seconds or milliseconds, because real-world speed depends on hardware, operating systems, and processor loads. Instead, it counts the number of elementary operations or code statements executed by the algorithm.

The Best Way to Picture It

Imagine you are looking for a specific book in a library:

- **O(1) – Constant:** You already know the exact shelf. It always takes **1 step**, whether the library has 10 books or 10,000 books.
- **O(n) – Linear:** You have to look at every single book one by one. If the library grows to **n books**, you must take **n steps**.
- **O(n²) – Quadratic:** You have to compare every single book against every other book in the library. If you have 10 books, it takes 100 steps. If you have 100 books, it takes 10,000 steps!

Common Asymptotic Notations

- **Big O (O):** Represents the **worst-case scenario**, providing an upper bound on execution time.
- **Big Omega (Ω):** Represents the **best-case scenario**, providing a lower bound on execution time.
- **Big Theta (Θ):** Represents the **average-case scenario**, providing a tight bound when best and worst cases scale equally

How to Calculate Time Complexity

1. **Identify the input size (n):** Determine what variable controls the size of the data structure (e.g., array length).
2. **Count operations:** Look for loops, nested loops, and recursive function calls.
3. **Drop constants:** Ignore fixed multipliers. For example, 2n operations simplify directly to O(n).
4. **Keep the dominant term:** If an algorithm takes n² + n operations, drop the lower term (n) and express it as O(n²). 

Standard Time Complexities (Ordered by Efficiency)

|Notation|Name|Growth Behavior|Typical Example|
|---|---|---|---|
|**O(1)**|Constant|Time stays exactly the same regardless of input size.|Accessing an array element by index.|
|**\(O(\log n)\)**|Logarithmic|Time increases fractionally as the input size doubles.|Binary search in a sorted array.|
|**O(n)**|Linear|Time increases in direct proportion to input size.|Linear search through an unsorted array.|
|**\(O(n \log n)\)**|Linearithmic|Time increases slightly faster than a linear rate.|Efficient sorting algorithms like Merge Sort.|
|**O(n²)**|Quadratic|Time grows quadratically, often doubling the input quadruples the time.|Nested loops, like Bubble Sort or Selection Sort.|
|**\(O(2^n)\)**|Exponential|Time doubles with every single additional item added to the input.|Naive recursive calculation of Fibonacci numbers.|
|**O(n!)**|Factorial|Time grows exceptionally fast, becoming unusable for tiny inputs.|Solving the Traveling Salesperson Problem via brute force.|
