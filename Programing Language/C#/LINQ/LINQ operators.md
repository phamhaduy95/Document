**1.Overview**

In this section, we provide an overview of the standard query operators.
They fall into three categories:

- Sequence in, sequence out (sequence→sequence).

- Sequence in, single element or scalar value out.

- Nothing in, sequence out (generation methods).

Note: All the knowledge about LINQ operators mentioned in this article
are applied to local collection only. The other LINQ 's application for
entity framework core will be focused in a different article.

**2. Sequence to Sequence**

these includes several functional subsets

**2.1 Filtering**

Returns a subset of the original elements.**\**
![](media/image1.png){width="6.811809930008749in"
height="4.4524201662292215in"}

**2.1.1 Where**

Where returns the elements from the input sequence that satisfy the
given predicate.

List\<int\> numbers = new List\<int\> { 1, 3, 5, 6, 3, 2, 3, 4 };

// we filter any element whose value is greater than 3

var query = numbers.Where(v =\> v \> 3);

foreach(var number in query)

{

Console.WriteLine(number); // 5,6,4

}

Where is one of fundamental operators usable in query expression. It is
used similarly with where statement in SQL.

var query1 = from i in numbers

where i \> 3

select i;

Lambda expression used in Where operator allows second input argument as
the index of element in array-like collection.

An exception is thrown if you use indexed filtering in EF Core.

**2.1.2 Take, TakeLast, Skip, and SkipLast**

Take emits the first n elements and discards the rest.

Skip discards the first n elements and emits the rest.

TakeLast and SkipLast methods take or skip the last n elements.

**2.1.3 TakeWhile and SkipWhile**

TakeWhile enumerates the input sequence, emitting each item until the
given predicate is false. It then ignores the remaining elements.

SkipWhile enumerates the input sequence, ignoring each item until the
given predicate is false. It then emits the remaining elements.

**2.1.4 Distinct and DistinctBy**

Distinct returns the input sequence, stripped of duplicates

DistinctBy method was introduced in .NET 6 and lets you specify a key
selector to be applied before performing equality comparison.

**2.2 Projecting**

IEnumerable\<TSource\>→IEnumerable\<TResult\>\
Transforms each element with a lambda function. The result sequence may
have its element transformed to completely new type.

![](media/image2.png){width="6.9548359580052495in"
height="2.1657753718285213in"}

**2.2.1 Select**

One frequent application of Select is to perform some transformation on
the input sequence. Doing so won't change the type of element of the
output sequence.

/\*\* select is used to do some transformation on the existing
sequence\*/

var numbers = new List\<int\> { 1, 2, 3 ,8};

// we just add 10 to each element of input element

var query = numbers.Select(x =\> x + 10);

foreach (var e in query) {

Console.WriteLine(e);

}

Select statements are also used to project into anonymous types or
concreted types.

/\*\* select operators can be used to project an existing sequence into
a sequence of new object\*/

class Student{

public string name;

public Student(string name)

{

this.name = name;

}

public Student () { }

public override string ToString() {

return \$\"student {name}\";

}

}

var names = new List\<string\> { \"Hung\", \"Cao\", \"Tung\", \"Van\" };

var Students = from n in names

select new Student {

name = n

};

foreach (Student s in Students){

Console.WriteLine(s);

}

A projection with no transformation is sometimes used with query syntax
to satisfy the requirement that the query end in a select or group
clause.

/\*\* select with no transformation is often used in query expression as
the query termination \*/

var nameQuery = from n in names

where n.Length \> 0

orderby n.Length descending

select n; // select with no transformation is often used in query
expression as the query termination

2.2.2 **SelectMany**

SelectMany concatenates subsequences into a single flat output sequence

The SelectMany accepts the other sequence that concentrates them into
one sequence.

var names = new List\<string\> {\"Hung\", \"Cao\", \"Nhan\", \"Trung\"};

var characters = names.Select(x =\> x.ToLower())

.SelectMany(x =\> x.ToCharArray())

.Select(x =\> x.ToString())

.Distinct();

foreach (var e in characters) {

Console.Write(\$\"{e} \") ;//u n g c a o t r

}

SelectMany can be used to flat the nested collections.

var listOfIntArray = new List\<int \[\]\>();

listOfIntArray.Add(new int\[\] {1,3, 4, 5, 6 });

listOfIntArray.Add(new int\[\] { 2, 3, 4, 5, 7 });

listOfIntArray.Add(new int\[\] { 3, 4, 5, 5 });

var flattedArray = listOfIntArray.SelectMany(x =\> x.ToArray());

foreach (var e in flattedArray)

{

Console.Write(e);

}

SelectMany is supported in query syntax and is invoked by having an
additional generator---in other words, an extra from clause in the
query.

var fullNames = new List\<string\>() { \"Anne Williams\", \"John Fred
Smith\", \"Sue Green\" };

IEnumerable\<string\> query = from fullName in fullNames

> from name in fullName.Split() // Translates to
>
> SelectMany

select name;

Joining query is considerably faster than using SelectMany
