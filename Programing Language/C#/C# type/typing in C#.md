# Tuples 

You can combine multiple data into one variable using tuples. To define
tuples,

# Class

# Referenced vs value type

Value type variables directly contain their values. There\'s no separate
heap allocation or garbage collection overhead for value-type variables.

# Record

It is not uncommon to have the object whose application is to contain
data only. This type of object is called plain old data (POD). Although,
you can still use class that only consist public field to generate POD,
record type is more preferable choice to serve this purpose. The example
below shows the syntax for Record definition.

internal record StudentData(int Id, string Name = \"\", int Age=
default);

The reason for record preference over regular class type is its
immutability assurance and convenience for cloning data to another
record object using with statement.

internal record StudentData(int Id, string Name = \"\", int Age=
default);

internal static class RecordExample

{

public static void main()

{

var student1 = new StudentData(1,\"Hung\",20);

student1.Name = \"han\"; // Error cannot mutate Name property of
student1 record;

// use with statement to copy data from student1 to student2

var student2 = student1 with {};

Console.WriteLine(object.ReferenceEquals(student1, student2)); // false
mean student1 and student2 are different objects.

Console.WriteLine(student2); // StudentData { Id = 1, Name = Hung, Age =
20 }

// you can alter any property \'s value by assigning new value inside
the brackets.

var student3 = student1 with

{

Id = 2,

};

Console.WriteLine(student3); //StudentData { Id = 2, Name = Hung, Age =
20 }

}

}

# Struct

A struct can have most of the same features as a class; it can contain
methods, fields, properties, constructors and the same accessibility
keywords. However, the produced objects created from struct are treated
as value-type and not referenced typed just like class does.

public struct Point: IEquatable\<Point\>

{

readonly int x;

readonly int y;

public Point(int x, int y)

{

this.x = x;

this.y = y;

}

public override bool Equals(\[NotNullWhen(true)\] object? obj)

{

return obj is Point && this.Equals((Point)obj);

}

public bool Equals(Point other)

{

return x == other.x && y == other.y;

}

// we have to define both == and != operator inside the struct type.

public static bool operator == (Point a, Point b)

{

return a.Equals(b);

}

public static bool operator !=(Point a, Point b)

{

return !a.Equals(b);

}

// GetHashCode method is required if we want to override the default
Equals method.

public override int GetHashCode()

{

return HashCode.Combine(x, y);

}

public static void Example()

{

var point1 = new Point(1, 2);

var point2 = new Point(1, 2);

var point3 = new Point(3, 4);

Console.WriteLine(point1 == point2); // false

Console.WriteLine(point1 == point3); // true

Console.WriteLine(point1.GetHashCode() == point2.GetHashCode());

}

}

Like class, a struct need to implement IEquatable interface so that any
equality checking operation can be performed. Yet, since C# does not
automatically support == and != for a struct, C# requires us to define
both independently.

Note: Four main characteristics of OOP can be applied to both struct and
record type as well. So, consider applying these to make your struct and
record more flexible and maintainable.

# Enum

enum type is a value type defined by a set of named constants. To define
an enum type, use the enum keyword and specify the names of enum
members:

public enum Season

{

Spring,

Summer,

Autumn,

Winter

}

By default, int value that start from 0 is automatically assigned to
each name constant inside an enum. However, you can assign arbitrary int
values to them.

public enum Season

{

spring = 100,

summber = 10,

fall = 1000,

winter = 2

}

enum is often used to define a set of flags which is then consumed in
switch statement.

public static void PlanActivityForEachSeason(Season season)

{

switch(season)

{

case Season.spring:

\... // do something

break;

case Season.summer:

\...

break;

case Season.winter:

\...

break;

case Season.fall:

\...

break;

}

}

# Anonymous type

Anonymous type is a special object type that don't require name of class
or struct to be initiated. For example:

var student = new {

Name = "John",

Class = "12A1",

Age = 17,

}

Although, anonymous type can be used to hold several pieces of data at
once, it 's rarely used for this purpose and Tuples is preferred
instead, since Tuples are valued-type which are not managed by GC, thus
less overhead and more CPU efficiency. Regardless, anonymous type is
often seen in LINQ expression especially with projection operation like
Select, SelectMany, ...
