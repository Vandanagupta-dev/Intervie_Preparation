Creating a calculator using **polymorphism** involves defining a base class or interface for operations and then creating multiple derived classes to implement specific arithmetic operations like addition, subtraction, multiplication, and division.

### 🧠 What is Polymorphism?

Polymorphism allows objects of different classes to be treated through a common interface — usually via method overriding in inheritance.

---

## ✅ Example in C# using Polymorphism

```csharp
using System;
using System.Collections.Generic;

// Step 1: Define base class
abstract class Operation
{
    public abstract double Calculate(double a, double b);
}

// Step 2: Create derived classes for each operation
class Addition : Operation
{
    public override double Calculate(double a, double b) => a + b;
}

class Subtraction : Operation
{
    public override double Calculate(double a, double b) => a - b;
}

class Multiplication : Operation
{
    public override double Calculate(double a, double b) => a * b;
}

class Division : Operation
{
    public override double Calculate(double a, double b)
    {
        if (b == 0)
            throw new DivideByZeroException("Cannot divide by zero.");
        return a / b;
    }
}

// Step 3: Use polymorphism in the calculator
class Calculator
{
    private readonly Dictionary<string, Operation> _operations;

    public Calculator()
    {
        _operations = new Dictionary<string, Operation>
        {
            { "+", new Addition() },
            { "-", new Subtraction() },
            { "*", new Multiplication() },
            { "/", new Division() }
        };
    }

    public double Compute(string op, double a, double b)
    {
        if (_operations.TryGetValue(op, out var operation))
        {
            return operation.Calculate(a, b);
        }

        throw new InvalidOperationException("Unsupported operation");
    }
}

// Step 4: Example usage
class Program
{
    static void Main()
    {
        var calculator = new Calculator();

        Console.Write("Enter first number: ");
        double a = Convert.ToDouble(Console.ReadLine());

        Console.Write("Enter operation (+, -, *, /): ");
        string op = Console.ReadLine();

        Console.Write("Enter second number: ");
        double b = Convert.ToDouble(Console.ReadLine());

        try
        {
            double result = calculator.Compute(op, a, b);
            Console.WriteLine($"Result: {result}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```

---

### 🧾 Output Sample

```
Enter first number: 10
Enter operation (+, -, *, /): *
Enter second number: 5
Result: 50
```

---

### 💡 Key Concepts:

* **Polymorphism** is used here via an abstract base class `Operation`.
* Different arithmetic operations are implemented in separate derived classes.
* The `Calculator` class uses a dictionary to map string inputs to appropriate operations, demonstrating runtime polymorphism.

Let me know if you want this implemented in another language like Java, Python, or C++.



Absolutely! For a **2+ year C# interview**, you are likely to be asked **logic-based programming questions**. I’ll give you **common logic interview problems**, explanations, and **C# implementations** for each. These include **Prime numbers, string occurrence, palindrome, Fibonacci, factorial, etc.**, which are frequently asked in interviews.

---

## **1. Prime Number Check**

**Q:** How do you check if a number is prime in C#?

```csharp
using System;

class Program
{
    static bool IsPrime(int n)
    {
        if (n <= 1) return false;
        if (n == 2) return true;

        for (int i = 2; i <= Math.Sqrt(n); i++)
        {
            if (n % i == 0) return false;
        }
        return true;
    }

    static void Main()
    {
        int num = 29;
        Console.WriteLine(IsPrime(num) ? "Prime" : "Not Prime");
    }
}
```

**Logic:** A number is prime if it has only two divisors: 1 and itself. Check divisibility up to √n for efficiency.

---

## **2. Count Occurrences of a Character in String**

**Q:** Count how many times a character appears in a string.

```csharp
using System;

class Program
{
    static void Main()
    {
        string str = "interview";
        char ch = 'i';
        int count = 0;

        foreach (char c in str)
        {
            if (c == ch) count++;
        }

        Console.WriteLine($"'{ch}' occurs {count} times");
    }
}
```

**Logic:** Iterate through the string and increment a counter when character matches.

---

## **3. Reverse a String**

```csharp
using System;

class Program
{
    static void Main()
    {
        string str = "hello";
        char[] arr = str.ToCharArray();
        Array.Reverse(arr);
        string reversed = new string(arr);
        Console.WriteLine(reversed);
    }
}
```

**Logic:** Convert string to char array, reverse it, and create a new string.

---

## **4. Fibonacci Series**

**Q:** Print first N Fibonacci numbers.

```csharp
using System;

class Program
{
    static void Main()
    {
        int n = 10;
        int a = 0, b = 1;
        Console.Write($"{a} {b} ");
        for (int i = 2; i < n; i++)
        {
            int c = a + b;
            Console.Write(c + " ");
            a = b;
            b = c;
        }
    }
}
```

**Logic:** Each number is the sum of previous two numbers.

---

## **5. Factorial of a Number**

```csharp
using System;

class Program
{
    static int Factorial(int n)
    {
        if (n <= 1) return 1;
        return n * Factorial(n - 1);
    }

    static void Main()
    {
        int num = 5;
        Console.WriteLine(Factorial(num));
    }
}
```

**Logic:** Multiply all integers from 1 to N recursively.

---

## **6. Palindrome Check (String)**

```csharp
using System;

class Program
{
    static bool IsPalindrome(string str)
    {
        int i = 0, j = str.Length - 1;
        while (i < j)
        {
            if (str[i] != str[j]) return false;
            i++;
            j--;
        }
        return true;
    }

    static void Main()
    {
        string str = "level";
        Console.WriteLine(IsPalindrome(str) ? "Palindrome" : "Not Palindrome");
    }
}
```

**Logic:** Compare characters from start and end, move towards the center.

---

## **7. Count Word Occurrences in a Sentence**

```csharp
using System;

class Program
{
    static void Main()
    {
        string sentence = "C# is great and C# is powerful";
        string wordToFind = "C#";
        int count = 0;

        string[] words = sentence.Split(' ');
        foreach (string w in words)
        {
            if (w.Equals(wordToFind)) count++;
        }

        Console.WriteLine($"'{wordToFind}' occurs {count} times");
    }
}
```

**Logic:** Split sentence into words and count matching words.

---

## **8. Find Largest Number in Array**

```csharp
using System;

class Program
{
    static void Main()
    {
        int[] arr = { 5, 12, 3, 45, 7 };
        int max = arr[0];
        foreach (int n in arr)
        {
            if (n > max) max = n;
        }
        Console.WriteLine($"Largest number is {max}");
    }
}
```

**Logic:** Iterate through array, keep track of the largest number.

---

## **Tips for C# Logic Interviews**

1. Always **optimize loops** (e.g., √n for prime numbers).
2. Mention **recursive vs iterative approaches**.
3. Explain **time complexity** for each solution (important for high-paying interviews).
4. Be ready for **combined questions**: e.g., check if a Fibonacci number is prime.

---

If you want, I can make a **full 20–30 common C# interview logic problems sheet**, all ready with **code + explanation + time complexity**, so you can **practice for high-paying interviews** efficiently.

Do you want me to prepare that?

using System;

class PrimeProgram
{
    public static void Main(string[] args)
    {
        Console.WriteLine("Enter the number: ");
        int number = Convert.ToInt32(Console.ReadLine());
        bool isPrime = true;

        if (number <= 1)
        {
            isPrime = false;
        }
        else
        {
            for (int i = 2; i <= Math.Sqrt(number); i++)
            {
                if (number % i == 0) 
                {
                    isPrime = false;
                    break;
                }
            }
        }

        if (isPrime)
        {
            Console.WriteLine($"{number} is Prime");
        }
        else
        {
            Console.WriteLine($"{number} is Not Prime");
        }
    }
}

