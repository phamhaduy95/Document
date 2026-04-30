# Generics in C#

Generics allow you to write classes and methods with type parameters, enabling code reuse, type safety, and performance (by avoiding boxing).

---

## 1. Generic Classes
A generic class is defined using the `<T>` notation after the class name. `T` is a placeholder for the actual type that will be provided when the class is instantiated.

```csharp
public class Stack<T>
{
    private int _position;
    private T[] _data = new T[100];

    public void Push(T obj) => _data[_position++] = obj;
    public T Pop() => _data[--_position];
}
```

### Usage
To use the class, replace the type parameter with a specific type.
```csharp
Stack<int> stackOfInt = new Stack<int>();
stackOfInt.Push(10);
```

---

## 2. Generic Methods
Methods can also be generic, independent of whether the class they belong to is generic.

```csharp
void Swap<T>(ref T a, ref T b)
{
    T temp = a;
    a = b;
    b = temp;
}
```

### Usage
```csharp
int x = 20, y = 10;
Swap<int>(ref x, ref y);
```

---

## 3. Generic Constraints
Constraints allow you to restrict the types that can be used as type arguments. Use the `where` keyword to specify these requirements.

| Constraint | Description |
| :--- | :--- |
| `where T : class` | `T` must be a reference type. |
| `where T : struct` | `T` must be a value type (excluding `Nullable`). |
| `where T : new()` | `T` must have a public parameterless constructor. |
| `where T : BaseClass` | `T` must derive from or be `BaseClass`. |
| `where T : IInterface` | `T` must implement `IInterface`. |
| `where T : notnull` | `T` must be a non-nullable type. |

### Example with Multiple Constraints
```csharp
class GenericClass<T, U> 
    where T : SomeClass, IInterface1
    where U : new()
{
    // Implementation
}
```

---

## 4. Default Values
In generic code, you cannot assign `null` or `0` to a variable of type `T` because `T` could be either a value or a reference type. Instead, use the `default(T)` operator to get the appropriate zero-like value.

```csharp
static void ShowDefault<T>()
{
    // Prints 0 for int, null for string, false for bool, etc.
    Console.WriteLine(default(T));
}
```
