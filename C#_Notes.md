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

