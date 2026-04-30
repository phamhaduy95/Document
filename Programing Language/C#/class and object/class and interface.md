# Classes and Interfaces in C#

## 1. Read-only Fields
You can make a class field constant after initialization by applying the `readonly` modifier.

- **Value Types**: The value cannot be changed after the constructor finishes.
- **Reference Types**: The reference itself is constant (the variable cannot point to a different object), but the properties of the referred object can still be modified.

```csharp
public class MyClass
{
    public readonly int Id;
    public readonly List<string> Tags = new List<string>();

    public MyClass(int id)
    {
        Id = id; // Allowed in constructor
    }

    public void Update()
    {
        // Id = 20; // Error: Readonly field
        Tags.Add("New Tag"); // Allowed: reference is constant, but object is mutable
    }
}
```

---

## 2. Method Overriding
To re-implement a method from a base class in a subclass, C# uses the `virtual` and `override` keywords.

1.  **Virtual**: Mark the method in the base class as `virtual`.
2.  **Override**: Use the `override` keyword in the subclass to provide a new implementation.

```csharp
public class Animal
{
    public virtual void Speak() => Console.WriteLine("Animal sound");
}

public class Dog : Animal
{
    public override void Speak() => Console.WriteLine("Woof!");
}
```

---

## 3. Access Modifiers
Access modifiers specify the visibility of class members.

| Modifier | Accessibility |
| :--- | :--- |
| **private** | Accessible only within the same class. (Default for members). |
| **protected** | Accessible within the same class or in derived classes. |
| **internal** | Accessible only within the same assembly (project). |
| **public** | Accessible from anywhere. |
| **protected internal** | Accessible within the same assembly OR from a derived class in another assembly. |
| **private protected** | Accessible within the same class or derived classes within the same assembly. |

---

## 4. Interfaces
An interface defines a contract that classes must implement. It can contain methods, properties, events, or indexers. Starting with C# 8.0, interfaces can also include default implementations for members.

```csharp
public interface ILogger
{
    void Log(string message);
}

public class FileLogger : ILogger
{
    public void Log(string message) => File.WriteAllText("log.txt", message);
}
```
