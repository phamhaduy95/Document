# Introduction

Generics enable us to write types and meth*ods with* type arguments,
which can be filled in at compile time to produce different versions of
the types or methods that work with particular types.

# Generic class

The syntax for structs, records, and interfaces is much the same: the
type name is followed immediately by a type parameter list

\<T\> notation when is put right after class 's name will allow that
class use T as the generic type. T is just common letter to present a
generic type. You can choose other meaningful words as a generic type.

public class Stack\<T\>

{

int position;

T\[\] data = new T\[100\];

public void Push(T obj) =\> data\[position++\] = obj;

public T Pop() =\> data\[\--position\];

}

To able to initiate the generic class, replace generic type T with more
specific type.

Stack\<int\> StackOfInt = new Stack\<int\> ();

Generic classes are often used to create a collection which is aimed to
work with many types.

# Variant generic

Each different type I supply as an argument to NamedContainer\<T\>
constructs a distinct type. (And for generic types with multiple type
arguments, each distinct combination of type arguments would construct a
distinct type.) This means that NamedContainer\<int\> is a different
type than NamedContainer\<string\>.

Invariant, covariant and contravariant

# Generic method

We can also make method to be generic as well.

void Swap\<T\> (ref T a, ref T b)

{

T temp = a;

a = b;

b = temp;

}

to use generic method, provide it with a specific type then call it as a
normal method.

> int a = 20;
>
> int b = 10;
>
> Swap\<int\> (ref a, ref b);

It's possible to have multiple generic types in one declaration.

> class Dictionary**\<TKey, TValue\>** {\...}

# Generic constraints

C# allows you to state that a type argument must fulfill certain
requirements

By default, you can substitute a type parameter with any type
whatsoever. Constraints can be applied to a type parameter to require
more specific type arguments. These are the possible constraints:

where T: base-class // Base-class constraint

where T: interface // Interface constraint

where T: class // Reference-type constraint

where T: class? // (see \"Nullable Reference Types\" in Chapter 1)

where T: struct // Value-type constraint (excludes Nullable types)

where T: unmanaged // Unmanaged constraint

where T: new() // Parameterless constructor constraint

where U: T // Naked type constraint

where T: notnull // Non-nullable value type, or (from C# 8)

// a non-nullable reference type

In the following example, GenericClass\<T,U\> requires T to derive from
(or be identical to) SomeClass and implement Interface1, and requires U
to provide a parameterless constructor:

class SomeClass {}

interface Interface1 {}

class GenericClass\<T,U\> where T: SomeClass, Interface1

where U: new()

# Default value for generic type

Variables of any type can be initialized to a default value. Sometimes,
it can be useful for generic code to be able to set a variable to this
initial default zero-like value. You cannot assign null into a variable
whose type is specified by a type parameter unless that parameter has
been constrained to be a reference type. And you cannot assign the
literal 0 into any such variable, because there is currently no way to
constrain a type argument to be a numeric type.

**static** **void** ShowDefault\<T\>()

{

Console.WriteLine(**default**(T));

}
