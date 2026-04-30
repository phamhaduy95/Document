# Typing in C#

In C#, types are divided into two main categories: **Value Types** and **Reference Types**. Understanding the difference between them is fundamental to memory management and performance in .NET.

---

## Reference vs Value Types

- **Value Types**: Variables directly contain their data. They are usually stored on the stack (though they can be part of an object on the heap). There is no separate heap allocation or garbage collection overhead for simple value-type variables.
- **Reference Types**: Variables store a reference (a pointer) to the actual data, which is stored on the managed heap. Multiple variables can point to the same object.

---

## Reference Types

### Classes
Classes are the primary building blocks of object-oriented programming in C#. They support inheritance, polymorphism, and encapsulate data and behavior.

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }

    public void Study() 
    {
        Console.WriteLine($"{Name} is studying.");
    }
}
```

### Records
It is common to have objects whose sole purpose is to contain data. These are often called **Plain Old Data (POD)** objects. While you can use a standard class with public fields, the `record` type is a more concise and preferable choice.

The reason for preferring records over regular classes is their built-in **immutability assurance** and the convenience of cloning data using the `with` statement.

```csharp
internal record StudentData(int Id, string Name = "", int Age = default);

internal static class RecordExample
{
    public static void Main()
    {
        var student1 = new StudentData(1, "Hung", 20);

        // Error: cannot mutate Name property of student1 record if defined as positional
        // student1.Name = "han"; 

        // Use 'with' statement to copy data from student1 to student2 (Non-destructive mutation)
        var student2 = student1 with { };

        Console.WriteLine(object.ReferenceEquals(student1, student2)); // false (different objects)
        Console.WriteLine(student2); // StudentData { Id = 1, Name = Hung, Age = 20 }

        // You can alter properties by assigning new values inside the brackets
        var student3 = student1 with
        {
            Id = 2,
        };

        Console.WriteLine(student3); // StudentData { Id = 2, Name = Hung, Age = 20 }
    }
}
```

### Anonymous Types
Anonymous types provide a convenient way to encapsulate a set of read-only properties into a single object without having to explicitly define a type first.

```csharp
var student = new {
    Name = "John",
    Class = "12A1",
    Age = 17
};
```

While anonymous types can hold data, **Tuples** are often preferred for simple data grouping because they are value types (less GC pressure). Anonymous types are most commonly used in **LINQ** projection operations (e.g., `.Select(x => new { x.Name, x.Id })`).

---

## Value Types

### Structs
A `struct` can have many of the same features as a class (methods, fields, properties, constructors). However, instances of a struct are **value types**.

```csharp
public struct Point : IEquatable<Point>
{
    private readonly int x;
    private readonly int y;

    public Point(int x, int y)
    {
        this.x = x;
        this.y = y;
    }

    public override bool Equals(object? obj)
    {
        return obj is Point other && this.Equals(other);
    }

    public bool Equals(Point other)
    {
        return x == other.x && y == other.y;
    }

    // We must define both == and != operators for structs if we want to use them
    public static bool operator ==(Point a, Point b) => a.Equals(b);
    public static bool operator !=(Point a, Point b) => !a.Equals(b);

    public override int GetHashCode() => HashCode.Combine(x, y);

    public static void Example()
    {
        var point1 = new Point(1, 2);
        var point2 = new Point(1, 2);
        var point3 = new Point(3, 4);

        Console.WriteLine(point1 == point2); // true (Value equality)
        Console.WriteLine(point1 == point3); // false
        Console.WriteLine(point1.GetHashCode() == point2.GetHashCode()); // true
    }
}
```

> **Note:** Structs do not automatically support `==` and `!=`. You must implement them manually. Also, consider the four characteristics of OOP (Encapsulation, Inheritance, etc.); while structs have limits (e.g., no inheritance from other structs/classes), they are very flexible for small data structures.

### Enums
An `enum` is a value type defined by a set of named constants.

```csharp
public enum Season
{
    Spring,
    Summer,
    Autumn,
    Winter
}
```

By default, the underlying values start at 0. You can also assign custom values:

```csharp
public enum SeasonCustom
{
    Spring = 100,
    Summer = 10,
    Fall = 1000,
    Winter = 2
}
```

Enums are frequently used in `switch` statements to control flow based on specific states.

### Tuples
Tuples allow you to group multiple data elements in a lightweight data structure. Unlike anonymous types, they are value types.

```csharp
// Defining a tuple
(string Name, int Age) person = ("Alice", 30);
Console.WriteLine($"{person.Name} is {person.Age} years old.");

// Returning multiple values from a method
(int sum, int count) GetStats(int[] numbers) 
{
    return (numbers.Sum(), numbers.Length);
}
```
