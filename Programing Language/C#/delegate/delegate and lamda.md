# Delegates and Lambdas in C#

In C#, functions are not first-class citizens in the same way they are in languages like JavaScript. However, you can still pass functionality as an argument using **Interfaces** or **Delegates**.

## 1. Using Interfaces
One way to pass a function is to encapsulate it within an interface. For example, `IComparer<T>` defines a `Compare` method.

```csharp
class DescendingComparer : IComparer<int>
{
    public int Compare(int x, int y) => y - x;
}

var list = new List<int> { 1, 3, 2, 5, 4 };
list.Sort(new DescendingComparer());
```

---

## 2. Using Delegates
A **Delegate** is a type that defines a method signature. It allows you to treat a group of methods as a single object.

```csharp
public delegate T Transformer<T>(T arg);

int Square(int n) => n * n;

Transformer<int> compute = Square;
Console.WriteLine(compute(5)); // 25
```

### Multicast Delegates
Delegates can hold references to multiple methods using the `+` and `-` operators. When a multicast delegate is invoked, methods are called in the order they were added.

```csharp
void MethodA() => Console.WriteLine("A");
void MethodB() => Console.WriteLine("B");

Action del = MethodA;
del += MethodB; // Subscribing another method

del(); // Executes both MethodA and MethodB in order
```

### Null Safety
Always check if a delegate is null before invoking it, or use the null-conditional operator to avoid a `NullReferenceException`.

```csharp
del?.Invoke();
```

---

## 3. Built-in Delegates
.NET provides several pre-defined delegate types for common scenarios:

- **Action**: Accepts arguments but returns `void`.
  - `Action<T1, T2>`
- **Func**: Accepts arguments and returns a value of a specified type.
  - `Func<int, string>` (Takes an `int`, returns a `string`)
- **Predicate<T>**: Accepts one argument and returns a `bool`.
  - `Predicate<int> isPositive = x => x > 0;`
- **Comparison<T>**: Compares two objects of the same type and returns an `int`.

---

## 4. Lambdas and Anonymous Methods
Lambdas provide a concise way to write anonymous functions.

### Anonymous Methods (Older Syntax)
```csharp
Func<int, bool> isPositive = delegate(int x) { return x > 0; };
```

### Lambda Expressions (Modern Syntax)
```csharp
Func<int, bool> isPositive = x => x > 0;
Comparison<int> compare = (a, b) => a - b;
```
