Projection:

Select

Joining Two DataSets

SelectMany.

Concat:

Chunk

OrderBy

Count

You should be wary of code such as if (q.Count() \> 0). Calculating the
exact count may require the entire source query (q in this case) to be
evaluated

Although most LINQ operators defer execution, as you've now seen there
are some exceptions. With most LINQ providers, the Contains, Any, and
All operators do not produce a wrapped result. (E.g., in LINQ to
Objects, these return a bool, not an IEnumerable\<bool\>.) This
sometimes means that these operators need to do some slow work. For
example, EF Core's LINQ provider will need to send a query to the
database and wait for the response before being able to return the bool
result.

GroupBy

Filter:

Where

Skip

Take

TakeWhile
