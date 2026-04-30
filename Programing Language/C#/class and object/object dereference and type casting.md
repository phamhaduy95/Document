Dereferencing a variable means to access one of its members using the .
(dot) When you dereference a variable whose value is null, the runtime
throws
a [System.NullReferenceException](https://learn.microsoft.com/en-us/dotnet/api/system.nullreferenceexception).

However, nullable reference types and nullable value types are
implemented differently: nullable value types are implemented
using [System.Nullable\<T\>](https://learn.microsoft.com/en-us/dotnet/api/system.nullable-1),
and nullable reference types are implemented by attributes read by the
compiler. For example, string? and string are both represented by the
same
type: [System.String](https://learn.microsoft.com/en-us/dotnet/api/system.string).
However, int? and int are represented
by System.Nullable\<System.Int32\> and [System.Int32](https://learn.microsoft.com/en-us/dotnet/api/system.int32),
respectively.

Type Casting.

Implicit conversions: No special syntax is required because the
conversion always succeeds and no data will be lost. Examples include
conversions from smaller to larger integral types, and conversions from
derived classes to base classes.

c

Explicit conversions (casts): Explicit conversions require a cast
expression. Casting is required when information might be lost in the
conversion, or when the conversion might not succeed for other reasons.
Typical examples include numeric conversion to a type that has less
precision or a smaller range, and conversion of a base-class instance to
a derived class.
