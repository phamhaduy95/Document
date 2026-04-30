# Arrays in C#

Arrays are used to store a fixed number of strongly typed objects. In C#, an array is actually an object, and the `System.Array` class is the abstract base type of all array types.

## Characteristics
- Arrays implement `IList` and `IEnumerable`.
- You can use the `foreach` statement to iterate through an array.
- Arrays have a fixed size once initialized.

## Default Value Behavior
When an array is created, its elements are initialized to their default values:

- **Value Types**: Elements are initialized with the 0-bit pattern (e.g., `0` for `int`, `false` for `bool`).
- **Reference Types**: Elements are initialized to `null`.
- **Nullable Value Types**: `HasValue` is set to `false`, and elements are considered `null`.

## Example
```csharp
// Declaration and initialization
int[] numbers = new int[5]; // All elements are 0

string[] names = new string[3]; // All elements are null

// Iteration
foreach (var num in numbers)
{
    Console.WriteLine(num);
}
```
