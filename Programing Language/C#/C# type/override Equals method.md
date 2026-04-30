# Overriding Equals and GetHashCode

In C#, if you want to implement custom value-based equality for a class or struct, you must override both the `Equals` method and the `GetHashCode` method.

## The Relationship Between Equals and GetHashCode
A correct implementation must meet these two critical requirements:

1.  **Consistency**: An instance must return the same hash code as long as its value remains unchanged.
2.  **Equality Agreement**: Two instances that are considered equal via the `Equals` method **must** return the same hash code.

> [!WARNING]
> If two objects are equal but have different hash codes, they will not work correctly in collections like `Dictionary<K, V>` or `HashSet<T>`.

## Example Implementation

```csharp
public class Person
{
    public string Ssn { get; set; }
    public string Name { get; set; }

    public override bool Equals(object? obj)
    {
        if (obj is Person other)
        {
            return this.Ssn == other.Ssn;
        }
        return false;
    }

    public override int GetHashCode()
    {
        return Ssn?.GetHashCode() ?? 0;
    }
}
```
