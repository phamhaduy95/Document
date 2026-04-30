# Table of Contents {#table-of-contents .TOC-Heading}

[1 Nullable value type [2](#nullable-value-type)](#nullable-value-type)

[2 Working with nullable value
[2](#working-with-nullable-value)](#working-with-nullable-value)

**\**

# Nullable value type

By default, only object type or reference type can accept null as a
valid value. However, C# consists of a set of types each of which can
have additional null value beside its underlying value type. This
category of type is call nullable value type. To define a nullable value
type, just insert question mark (?) at the end of the name of value
type. For example.

> double? pi = 3.14;
>
> char? letter = \'a\';
>
> string? name = null;
>
> int m2 = 10;
>
> bool? flag = null;
>
> // An array of a nullable value type
>
> int?\[\] arr = new int?\[10\];

Any nullable value type is an instance of the generic
System.Nullable\<T\> structure. You can refer to a nullable value type
with an underlying type T in any of the following interchangeable forms:
Nullable\<T\> or T?.

# Working with nullable value

System.Nullable\<T\> supertype provides us some useful methods to handle
the possible null for any nullable value type:

- Nullable\<T\>.HasValue indicates whether an instance of a nullable
  value type has a value of its underlying type.

- Nullable\<T\>.Value gets the value of an underlying type if HasValue
  is true. If HasValue is false, the Value property throws an
  InvalidOperationException.

For instance, you want to

> int? b = 10;
>
> if (b.HasValue)
>
> {
>
> Console.WriteLine(\$\"b is {b.Value}\");
>
> }
>
> else
>
> {
>
> Console.WriteLine(\"b does not have a value\");
>
> }

If you want to assign a value of a nullable value type to a non-nullable
value type variable, you might need to specify the value to be assigned
in place of null. Use the null-coalescing operator ?? to do that.

> int? a = 28;
>
> int b = a ?? -1;
>
> Console.WriteLine(\$\"b is {b}\"); // output: b is 28
>
> int? c = null;
>
> int d = c ?? -1;
>
> Console.WriteLine(\$\"d is {d}\"); // output: d is -1
