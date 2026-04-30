Sometimes, you may end up writing long and complex LINQ query. One of
straightforward solutions to reduce the complexity is to break the big
complicated query into smaller intermediate ones. There are some
strategies for achieving this.

**Progressive Query Building**

The approach is mostly used for fluent syntax. As fluent syntax support
chaining multiple operators together, and we can break the chain of
LINQS operator into several middle query.

> var names = new List\<string\> { \"Hung\", \"Cao\", \"Van\", \"Tung\"
> ,\"Chau\",\"An\"};
>
> /\*\* we use multiple small intermediate queries instead of one single
> query\*/
>
> var filtered = names.Where(n =\> n.Contains(\"H\"));
>
> var ordered = filtered.OrderBy(n =\> n.Length);
>
> var projected = ordered.Select(n=\> n.ToUpper());
>
> var limited = projected.Take(2);
>
> foreach(var name in limited) {
>
> Console.WriteLine(name);
>
> }

Moreover, the progressive query building method allows us to write
conditional query.

if (includeFilter) query = query.Where(\...)

**The into keyword**

This is only appliable in query expression. It enables us to extend the
query after select or group operation which are both the query
termination operators.

var resultQuery = from n in names // first query scope

where n.Length \> 0 // first query scope

select n // first query scope

into n1 // second query scope

orderby n1.Length // second query scope

select n1.ToLower(); // second query scope

n1 represents the element of the sequence resulted from the first query.

When move to next query scope, any variable from previous scope (for
example n from the first query) is not accessible.

var query = from n1 in names

select n1.ToUpper()

into n2 // Only n2 is visible from here on.

> where n1.Contains(\"x\") // Illegal: n1 is not in scope.

select n2;

**Wrapping query**

A query built progressively can be formulated into a single statement by
wrapping one query around another. For example

**var tempQuery = tempQueryExpr**

**var finalQuery = from \... in tempQuery \...**

can be reformulated as:

**var finalQuery = from \... in (tempQueryExpr)**
