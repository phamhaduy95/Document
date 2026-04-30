Arrays are most useful for creating and working with a fixed number of
strongly typed objects. For information about arrays.

Default value behaviour of array

- For value types, the array elements are initialized with the default
  value, the 0-bit pattern; the elements will have the value 0.

- All the reference types (including the non-nullable), have the values
  null.

- For nullable value types, HasValue is set to false and the elements
  would be set to null.

In C# Array is actually an Object.
[Array](https://learn.microsoft.com/en-us/dotnet/api/system.array) is
the abstract base type of all array types.
implement [IList](https://learn.microsoft.com/en-us/dotnet/api/system.collections.ilist),
and [IEnumerable](https://learn.microsoft.com/en-us/dotnet/api/system.collections.ienumerable).
You can use
the [foreach](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/iteration-statements#the-foreach-statement) statement
to iterate through an array
