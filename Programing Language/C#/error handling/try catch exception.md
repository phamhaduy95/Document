# Exception Handling in C#

In C#, the `try-catch` statement is the primary tool for handling runtime errors (exceptions).

## The try-catch Block
A `try-catch` statement consists of a `try` block containing code that might throw an exception, followed by one or more `catch` blocks to handle those exceptions.

```csharp
try 
{
    // Code that may throw an exception
    byte b = byte.Parse(args[0]);
    Console.WriteLine(b);
}
catch (IndexOutOfRangeException) 
{
    Console.WriteLine("Please provide at least one argument.");
}
catch (FormatException) 
{
    Console.WriteLine("That's not a valid number!");
}
catch (OverflowException) 
{
    Console.WriteLine("The number is too large for a byte.");
}
```

> [!TIP]
> Always list the most **specific** exceptions first, followed by more general ones. The base `System.Exception` should always be the last catch block if included.

---

## The finally Clause
The `finally` block contains code that **always** executes, regardless of whether an exception was thrown or caught. It is primarily used for cleaning up resources (like file handles or database connections).

```csharp
void ReadFile() 
{
    StreamReader reader = null;
    try 
    {
        reader = File.OpenText("file.txt");
        Console.WriteLine(reader.ReadToEnd());
    }
    finally 
    {
        // Ensure the reader is closed even if an error occurs
        reader?.Dispose();
    }
}
```

---

## The using Statement
Since resource cleanup is a common task, C# provides the `using` statement as a shorthand for `try-finally`. Any object that implements the `IDisposable` interface can be used with `using`.

```csharp
using (StreamReader reader = File.OpenText("file.txt")) 
{
    Console.WriteLine(reader.ReadToEnd());
} 
// The reader is automatically disposed of here
```

In modern C#, you can also use a **using declaration** for even cleaner code:

```csharp
using var reader = File.OpenText("file.txt");
Console.WriteLine(reader.ReadToEnd());
// Disposed at the end of the current scope
```
