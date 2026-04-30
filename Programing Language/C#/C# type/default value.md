# Default Values in C#

The majority of built-in types in .NET are automatically assigned a default value when no specific value is provided. You can obtain the default value of any type using the `default(T)` operator (or the `default` literal in newer C# versions).

```csharp
Console.WriteLine(default(int)); // 0
```

## Primitive Types

| Type | Default Value |
| :--- | :--- |
| `bool` | `false` |
| `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong` | `0` |
| `float`, `double`, `decimal` | `0` |
| `char` | `\0` (null character) |

```csharp
Console.WriteLine(default(bool));    // False
Console.WriteLine(default(float));   // 0
Console.WriteLine(default(decimal)); // 0
```

## Reference Types
All reference types (including `string`) have a default value of `null`.

```csharp
string str = default(string);
Console.WriteLine(str == null); // True

internal class Student
{
    public string Id { get; set; }
    public string Name { get; set; }
    public uint Age { get; set; }
    public string ClassName { get; set; }

    public Student()
    {
        Id = Guid.NewGuid().ToString();
    }
}

var student = default(Student);
Console.WriteLine(student == null); // True
```

## Special Structs
Commonly used structs like `DateTime` and `Guid` have specific default values:

- **DateTime**: `1/1/0001 12:00:00 AM`
- **Guid**: `00000000-0000-0000-0000-000000000000`

```csharp
Console.WriteLine(default(DateTime)); // 1/1/0001 12:00:00 AM
Console.WriteLine(default(Guid));     // 00000000-0000-0000-0000-000000000000
```

For custom `struct` types, the default value is an instance with all its fields set to their respective default values.
