# Working with Nullable Values

By default, only reference types (like objects and strings) can accept `null` as a valid value. However, C# provides **Nullable Value Types**, which allow value types (like `int`, `double`, `bool`) to represent a `null` state.

## Nullable Value Types
To define a nullable value type, append a question mark (`?`) to the type name.

```csharp
double? pi = 3.14;
char? letter = 'a';
int? m2 = 10;
bool? flag = null;

// An array of nullable integers
int?[] arr = new int?[10];
```

Internally, a nullable value type is an instance of the `System.Nullable<T>` structure. `Nullable<T>` and `T?` are interchangeable forms.

## Handling Nullable Values
The `System.Nullable<T>` structure provides properties to safely check for and access values:

- **HasValue**: Returns `true` if the variable contains a value; `false` if it is `null`.
- **Value**: Gets the underlying value if `HasValue` is `true`. Accessing `Value` when `HasValue` is `false` throws an `InvalidOperationException`.

### Example: Checking for Values
```csharp
int? b = 10;

if (b.HasValue)
{
    Console.WriteLine($"b is {b.Value}");
}
else
{
    Console.WriteLine("b does not have a value");
}
```

### The Null-Coalescing Operator (??)
If you want to assign a nullable value to a non-nullable variable, you can provide a fallback value using the `??` operator.

```csharp
int? a = 28;
int b = a ?? -1;
Console.WriteLine($"b is {b}"); // output: b is 28

int? c = null;
int d = c ?? -1;
Console.WriteLine($"d is {d}"); // output: d is -1
```
