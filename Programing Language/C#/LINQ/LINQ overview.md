# Table of Contents

[1 Table of Contents [1](#_Toc128174905)](#_Toc128174905)

[2 Introduction [2](#introduction)](#introduction)

[3 Writing LINQ query [2](#_Toc128174907)](#_Toc128174907)

[3.1 The fluent API [2](#the-fluent-api)](#the-fluent-api)

[3.2 Query expression [3](#query-expression)](#query-expression)

[4 lazy execution (deferred execution)
[4](#lazy-execution-deferred-execution)](#lazy-execution-deferred-execution)

[5 Subqueries [5](#subqueries)](#subqueries)

[6 LINQ for EF core [6](#linq-for-ef-core)](#linq-for-ef-core)

**\**

# Introduction

LINQ, or Language Integrated Query, is a set of language and runtime
features for writing structured type-safe queries over local object
collections and remote data sources. To apply LINQ for local object
collection, the collection must implement the IEnumerable\<T\>
interface. Fortunately, most of built-in collection classes fulfill this
requirement by default.

The LINQ can also be used for query data from remote database such as
SQL with the help of Entity Framework. []{#_Toc128174907 .anchor}

# Writing LINQ query 

C# provides us two main ways for writing LINQ: fluent API and query
expression. Both require you to import System.Linq at the top of the
file or make it global so that you can access it everywhere with one
using statement.

## The fluent API

The fluent API consists of sets of various methods that can be chained
to perform turn by turn.

The fluent syntax requires you to provide lambda expression as the
operation which is executed on the input sequence.

Most of Lambda expression used in LINQ operator comprises of two input
arguments. The first one refers to the value of the element inside input
sequence while the second one is the index representing the order at
which the element is.

> string\[\] names = {\"Hung\", \"Cao\", \"Trung\",\"Binh\",\"Dung\"};
>
> //n here is element value in names string array.
>
> var filterNames = names.Where(n =\> n.Length \> 4 );
>
> foreach (var name in filterNames)
>
> {
>
> Console.WriteLine(name);
>
> }

Since many LINQ operators return IEnumerable\<T\>, we can to chain
multiple LINQ operations in one statement.

var n = names.Where(n =\> n.StartsWith(\"H\")).OrderBy(n =\> n).First();

Console.WriteLine(n);

## Query expression

C# provides a syntactic shortcut for writing LINQ queries, called query
expressions. It has many similarities with query operation in SQL.
Although, this approach provides us a really clean and convenient way to
write LINQ query, there are some limitations and strict rules which is
need to take into consideration.

- Query expressions always start with a from clause and end with either
  a select or group clause. the from clause declares a range variable
  (in this case, n), which has similar meaning with foreach statement.

- There are only limited set of LINQ operations which can be used within
  query expression. There are where, group, join, let, select, from, ...

> string\[\] names = { \"Hung\", \"Cao\", \"Trung\", \"Binh\", \"Dung\"
> };
>
> var nameQuery = from n in names
>
> where n.Length \> 3
>
> orderby n.Substring(0)
>
> select n;

foreach (var name in nameQuery)

{

Console.WriteLine(name);

}

Since the final result from query expression is an IEnumerable\<T\>
object. It is possible to append other LINQ operations at the end of it
by using mixed query type. For example, suppose we want to take only 2
longest names in the array, the LINQ query expression can be written
below.

var twoLongestNameQuery = (from n in names

orderby n.Length

select n).TakeLast(2);

As query expression is a just special syntax to write LINQs operation,
the results from it's just another IEnumerable object which can be
extended with more LINQs operation.

# Lazy execution (deferred execution)

An important feature of most query operators is that they execute not
when constructed but when enumerated (in other words, when MoveNext is
called on its enumerator).

> List\<int\> numbers = new List\<int\> {1, 2, 3};
>
> var procceedNumbers = numbers.Select(n =\> n + 20);
>
> numbers\[1\] = 10; // we modify the original number arrays
>
> foreach(var number in procceedNumbers)
>
> {
>
> Console.WriteLine(number); // we modify the original number arrays
>
> }

Like the example above, we generate new IEnumerable collection from the
original List\<int\> collection. the LINQ operation select only takes
effect when the foreach statement is called as the result. Therefore,
even we modify the origin List collection after the LINQ query operation
is called, the result still reflects the change.

The lazy execution is essential for LINQ since it allows the decouple
query execution from query declaration.

Most of the LINQ operations have lazy execution as default behavior.
However, there are some exceptions.

- Any operation which returns single value only. such as First(),
  Count(), Last(), ...

- Operation which transforms the input sequence into any specific type
  of collection such as: ToArray(), ToDictionary().

# Subqueries

A subquery is a query contained within another query's lambda
expression. You can do it in both query expression and fluent syntax.

We have Student class which contains fields to describe a student info:
name, age and score. We want to write a query expression to acquire a
sequence of students whose age is greater or equal to the student with
highest score in class. One viable strategy for the above requirement is
that we can execute two different data queries: One is the subquery
which aims to get the student object with highest score, and the other
is the big query which then use the result from the small query to
filter out.

public class Student {

> public string name;
>
> public int age;
>
> public int score;
>
> public Student (string name, int age, int score) {
>
> this.name = name;
>
> this.age = age;
>
> this.score = score;

}

> public override string ToString () {
>
> return \$\"student {name}, age: {age}, score: {score}\";
>
> }

}

List\<Student\> students = new List\<Student\>();

students.Add(new Student(\"Duy\", 20,8));

students.Add(new Student(\"Van\", 22, 6));

students.Add(new Student(\"Trung\", 19, 7));

students.Add(new Student(\"Nhan\", 20, 10));

students.Add(new Student(\"Cao\", 22, 20));

var query = from n1 in students

> where n1.age ==
>
> (from n2 in students orderby n2.score select n2).Last().age

select n1;

foreach (var student in query) {

Console.WriteLine(student);

}

We did a small query expression inside one bigger query expression to
acquire the student list sorted by the score in descendent order. The
query expression must fulfill any rule for regular C# query expression
and be put in parentless to mark them as the subquery. We also apply
mixed LINQs operation techniques to extend the query expression with
Last() to find out the student with best score.

# LINQ for EF core

EF Core relies on IQueryable\<T\>
