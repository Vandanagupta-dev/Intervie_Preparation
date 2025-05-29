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

