C# doesn’t have “named” class kinds in quite the same way as, say, Java (with interfaces vs. classes), but you can think of classes in C# as coming in several flavors based on modifiers, usage or how they’re declared:

---

## 1. Concrete (Regular) Class

```csharp
public class Person { … }
```

– You can instantiate it; it can have fields, methods, properties, etc.

---

## 2. Abstract Class

```csharp
public abstract class Animal  
{  
    public abstract void Speak();  
}
```

– Cannot be instantiated directly; provides base functionality/contracts.

---

## 3. Sealed Class

```csharp
public sealed class MathUtilities { … }
```

– Cannot be inherited from. Often used to prevent subclassing.

---

## 4. Static Class

```csharp
public static class Configuration  
{  
    public static string Read(string key) => …;  
}
```

– Only contains `static` members; you can’t instantiate it or inherit from it.

---

## 5. Partial Class

```csharp
// File1.cs
public partial class Order { void Save() { … } }
// File2.cs
public partial class Order { void Validate() { … } }
```

– Lets you split a single class’s definition across multiple files (often used by tools–e.g. WinForms, EF).

---

## 6. Nested (Inner) Class

```csharp
public class Outer  
{  
    public class Inner  
    {  
        // only accessible as Outer.Inner  
    }  
}
```

– A class defined inside another; great for logical grouping or implementing helpers.

---

## 7. Generic Class

```csharp
public class Repository<T>  
{  
    public void Add(T item) { … }  
}
```

– Parameterized by one or more type arguments at compile time.

---

## 8. Record Class (C# 9+)

```csharp
public record PersonRecord(string FirstName, string LastName);
```

– Immutable by default, value‑based equality, built‑in `ToString`, `with` expressions, etc.

---

## 9. Anonymous Types (Compiler‑Generated)

```csharp
var anon = new { Name = "Alice", Age = 30 };
```

– You don’t explicitly declare a class, but the compiler generates one at compile time.

---

### Accessibility Modifiers

Independently of the above, every class can also have an access modifier, which affects *where* it’s visible:

| Modifier                         | Visibility                                |
| -------------------------------- | ----------------------------------------- |
| `public`                         | Anywhere                                  |
| `internal`                       | Only within same assembly                 |
| `private`                        | (Nested classes only) only within parent  |
| `protected`/`protected internal` | Only to subclasses (and/or same assembly) |

---

### Putting it all together

You can even combine these:

```csharp
internal static partial class Foo<T> { … }
```

Here `Foo<T>` is a static, generic, partial class only visible inside its own assembly.

---

**In summary**, the main “types” of classes in C# are:

1. **Concrete**
2. **Abstract**
3. **Sealed**
4. **Static**
5. **Partial**
6. **Nested**
7. **Generic**
8. **Record**
9. **Anonymous (compiler‑generated)**

…and each of those can carry the usual C# access modifiers (`public`, `internal`, etc.).

---

**Here are some common C# interview questions along with concise answers to help you prepare:

---

### 1. What’s the difference between **`abstract`** classes and **interfaces** in C#?

* **Abstract class**

  * Can contain both abstract methods (no body) and concrete methods (with implementation).
  * Can define fields, constructors, and access modifiers.
  * A class can inherit only one abstract base.

* **Interface**

  * (Pre‑C# 8) Can contain only method/property/event signatures (no bodies). Since C# 8 default implementations are allowed.
  * Cannot contain fields or constructors.
  * A class or struct can implement multiple interfaces.

---

### 2. Explain **`async`/`await`**. How does it differ from traditional threading?

* Mark a method with `async` and return `Task`/`Task<T>` so callers can await it.
* `await` yields control back to the caller until the awaited `Task` completes—**without** blocking the thread.
* Under the hood, the compiler rewrites it as a state machine.
* **Difference**: Threads actually block/wait, whereas async methods free up the thread to do other work (better scalability in I/O‑bound scenarios).

---

### 3. What are **delegates**, and how do they differ from **events**?

* **Delegate**

  * A type‑safe function pointer: `public delegate int Transformer(int x);`
  * Can be invoked directly.

* **Event**

  * A special delegate field with publisher/subscriber semantics:

    ```csharp
    public event EventHandler Clicked;
    ```
  * Only the declaring class can invoke (raise) it; subscribers can only add/remove handlers.

---

### 4. How do **boxing** and **unboxing** work?

* **Boxing**

  * Converting a value type (e.g. `int`) into `object` or an interface it implements.
  * Wraps the value in a heap‑allocated object.

* **Unboxing**

  * Converting back from `object` to a value type.
  * Requires an explicit cast and will throw if the runtime type doesn’t match.

---

### 5. Explain **generics** in C#. Why use them?

* Generics let you write type‑safe, reusable code:

  ```csharp
  public class Repository<T> { … }
  ```
* Benefits:

  * **Type safety** (no need to cast).
  * **Performance** (avoids boxing and reflection).
  * **Reusability** (one implementation works for many types).

---

### 6. What’s the difference between **`IEnumerable<T>`** and **`IQueryable<T>`**?

* **`IEnumerable<T>`**

  * In‑memory, forward‑only iteration. Methods (e.g. `.Where`, `.Select`) operate in .NET after data is loaded.

* **`IQueryable<T>`**

  * Extends `IEnumerable<T>` with an expression tree.
  * Query provider (e.g. Entity Framework) can translate operations into SQL (deferred execution on the database side).

---

### 7. What is **dependency injection** (DI) and how does .NET Core support it?

* **DI**

  * Inversion of control pattern: dependencies are “injected” rather than created by the class itself.
  * Promotes testability, loose coupling, and easier maintenance.

* **.NET Core**

  * Built‑in container via `IServiceCollection` in `Startup.cs`:

    ```csharp
    services.AddTransient<IMyService, MyService>();
    services.AddScoped<IOther, OtherImpl>();
    services.AddSingleton<ILogger, Logger>();
    ```

---

### 8. What’s the **difference** between **`StringBuilder`** and **`string`** concatenation?

* **`string`**

  * Immutable: each concatenation creates a new string and copies data—bad for many repeated concatenations.

* **`StringBuilder`**

  * Mutable buffer: efficient when building up large or dynamic text via multiple `.Append()` calls.

---

### 9. How do you handle **exceptions** in C#? Any best practices?

* Use `try { … } catch (SpecificException ex) { … } finally { … }`.
* **Best practices**:

  * Catch the most specific exceptions you can handle.
  * Don’t swallow exceptions silently—log or rethrow.
  * Use `using` or C# 8’s `using var` for `IDisposable` instead of manual `finally` cleanup.

---

### 10. What is the **GC** (Garbage Collector) and how does it work in .NET?

* A background service that **automatically reclaims** memory of objects no longer referenced.
* Generational: Gen 0 (short‑lived) → Gen 1 → Gen 2 (long‑lived, expensive to collect).
* Triggers when Gen 0 fills, large object heap allocs occur, or on explicit `GC.Collect()` (rarely recommended).

---

Feel free to drill into any of these topics or ask for sample code!
**

---
Sure! Here's a concise yet clear answer for each **.NET Core technical question** you asked, perfect for interviews:

---

### 1. **What is the difference between .NET Core and .NET Framework?**

* **.NET Framework** is Windows-only, mature, and used mainly for desktop and ASP.NET applications.
* **.NET Core** is cross-platform (Windows, Linux, macOS), lightweight, open-source, and designed for modern cloud and web apps.
* .NET Core supports modular architecture and side-by-side versioning, unlike .NET Framework.

---

### 2. **What is dependency injection? How is it implemented in .NET Core?**

* Dependency Injection (DI) is a design pattern to inject dependencies into classes rather than creating them internally, promoting loose coupling and easier testing.
* In .NET Core, DI is built-in. You register services in the `Startup.cs` file’s `ConfigureServices` method using methods like `services.AddSingleton()`, `services.AddScoped()`, or `services.AddTransient()`. These services are then injected into constructors.

---

### 3. **What is middleware in ASP.NET Core?**

* Middleware are components in the HTTP request pipeline that handle requests and responses.
* Each middleware can perform actions before and after the next middleware executes.
* Examples: Authentication, routing, error handling middleware. Middleware is configured in `Startup.cs` via the `Configure` method using `app.Use...()` methods.

---

### 4. **How is routing handled in ASP.NET Core MVC?**

* Routing maps URLs to controller actions.
* ASP.NET Core supports **conventional routing** (defined patterns like `{controller}/{action}/{id?}`) and **attribute routing** (using attributes like `[Route("api/[controller]")]` on controllers or actions).
* Routing is configured in `Startup.cs` inside the `Configure` method.

---

### 5. **What is Entity Framework Core and how does it work?**

* EF Core is an Object-Relational Mapper (ORM) that enables developers to work with databases using .NET objects instead of SQL queries.
* It maps your classes (entities) to database tables and allows querying/updating data using LINQ.
* It supports migrations to evolve the database schema as your models change.

---

### 6. **What is the difference between eager, lazy, and explicit loading in EF Core?**

* **Eager Loading**: Loads related data along with the main entity in a single query using `.Include()`.
* **Lazy Loading**: Loads related data only when you access the navigation property. Requires enabling proxies.
* **Explicit Loading**: Manually loads related data by calling `.Load()` on the navigation property after the main entity is retrieved.

---

### 7. **How do you implement authentication and authorization in ASP.NET Core?**

* **Authentication** verifies user identity (e.g., cookie-based, JWT tokens, OAuth).
* **Authorization** controls access to resources based on roles, claims, or policies.
* Implemented using middleware, `[Authorize]` attribute, and configuring schemes in `Startup.cs`.

---

### 8. **What are the differences between ViewBag, ViewData, and TempData?**

* **ViewData**: A dictionary for passing data from controller to view; data lasts for a single request.
* **ViewBag**: A dynamic wrapper around ViewData, provides simpler syntax.
* **TempData**: Stores data that persists across redirects (multiple requests); usually implemented using session.

---

### 9. **What is model binding and validation in MVC?**

* **Model binding** automatically maps HTTP request data (form fields, query strings) to action method parameters or model properties.
* **Validation** uses data annotations (e.g., `[Required]`, `[Range]`) on models to enforce rules. The framework validates incoming data and sends validation errors to views.

---

### 10. **How do you handle exceptions globally in ASP.NET Core?**

* Use **middleware** like `app.UseExceptionHandler()` in `Startup.cs` to catch unhandled exceptions globally and show custom error pages or JSON responses.
* For API projects, you can use exception filters or custom middleware for structured error handling and logging.

---

If you want, I can provide **code snippets** or **more detailed explanations** on any question. Just ask!

