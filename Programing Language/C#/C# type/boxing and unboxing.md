Boxing is the process of converting a [value
type](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types) to
the type object and then stores it on the managed heap. Unboxing
extracts the value type from the object. Boxing is implicit; unboxing is
explicit.

In the following example, the integer variable i is *boxed* and assigned
to object o.

int i = 123;

// The following line boxes i.

object o = i;

The object o can then be unboxed and assigned to integer variable i:

o = 123;

i = (int)o; // unboxing

Boxing and unboxing both has limitation on performance. Both are
expensive in computation.
