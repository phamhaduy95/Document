# Table of Contents

[1 Table of Contents [1](#_Toc128167149)](#_Toc128167149)

[2 Delegate introduction
[1](#delegate-introduction)](#delegate-introduction)

[3 Built-in predicate [3](#built-in-predicate)](#built-in-predicate)

**\**

# Delegate introduction

C# don't have function as the first-class citizen as many popular
dynamic languages such as Python and Javascript. However, we are still
able to pass a function-like object as an argument. C# offers two
approaches to do this. The first is to utilize Interface to encapsulate
the function.

For instance, we have IComparer\<T\> interface which consists of
CompareTo method that allows you to define how we compare two objects.

Give example:

class DescendingComparer : IComparer\<int\>

{

public int Compare(int x, int y)

{

// Compare y and x in reverse order

return x -- y;

}

}

var list = new List\<int\>() { 1, 3, 2, 5, 4 };

var comparer = new DescendingComparer();

list.Sort(comparer);

Beside the interface, we can use delegate to describe the common
template for a group of method. You can also treat it as a valid type
presenting group of methods. For example:

public delegate T Function\<T\>(T arg);

Note: delegate is often made generic so that it can be reused for many
types at once.

Then you can declare the delegate object which only accepts any function
whose function signature is matched with one defined in delegate type.

int getSquareOf(int number)

{

return number \* number;

}

Function\<int\> functionGroup = getSquareOf;

delegate object can use add and subtract operation to subscribe and
unsubscribe method which share the same method template defined in
delegate declaration.

int getAbsoluteValue(int number)

{

return Math.Abs(number);

}

functionGroup += getAbsoluteValue;

functionGroup -= getAbsoluteValue;

Since delegate object can hold many functions at once. You can trigger
all registered method by calling the delegate object directly as calling
regular method or using invoke method. When this happens, C# will base
on the order of subscription of each method to determine its turn to be
executed.

functionGroup(23);

functionGroup.Invoke(2);

delegate object can be null by removing all methods inside it, to safely
call delegate, provide any null safety machenic.

functionGroup?.Invoke(23);

if (functionGroup != null) functionGroup(23);

Assigning a meaningless lambda without any side effect is aslo valiable
way to avoid null value error as delegate object is always guaranteed
not null.

Use delegate to make method to be passable into other method.

void printResultFromCallback\<T\>(Function\<T\> callback, T arg)

{

Console.WriteLine(callback?.invoke(arg));

}

getResultFromCallback\<int\>(getSquareOf, 8);

# Variant generic delegate 

# Built-in predicate

C# provides us some inbuilt delegate to work with:

- Predicate\<T\>: the predicate accepts one single argument whose type
  is T and return a boolean value. The base signature for Predicate is
  (T arg) =\> bool.

- Comparison\<T\>: compare two values that are same type and return
  integer as the result of the comparison.

- Action\<in T\>, Action\<in T1, in T2\>, ...: Action delegate family
  accepts one or more arguments but return nothing.

- Result\<int T, out R\>, Result\<in T1, in T2, out R\>, ...: Result
  delegate family is similar with Action in term of arguments but
  returns one single value that has type R.

# Lambda

Lambda is a special expression to define a function. A Lambda function
does not have name, this, it is sometimes called anonymous function. A
Lambda function can be defined in two ways. The first one is known as
anonymous method which involves the delegate keyword.

public static int GetIndexOfFirstNonEmptyBin(int\[\] bins) {

return Array.FindIndex(

bins,

delegate (int value) {return value \> 0;}

);

}

Alternatively, you can use arrow expression that does not require
delegate keyword.

Comparison\<int\> compare = (a, b) =\> a - b;
