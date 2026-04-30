# Composite LINQ Queries

When writing long and complex LINQ queries, it is often helpful to break them down into smaller, more manageable parts. This improves readability and maintainability.

---

## 1. Progressive Query Building
This approach is common when using **Fluent Syntax**. Since LINQ uses deferred execution, you can break a long chain of operators into several intermediate variables without any performance penalty.

```csharp
var names = new List<string> { "Hung", "Cao", "Van", "Tung", "Chau", "An" };

// Building the query step-by-step
var filtered = names.Where(n => n.Contains("H"));
var ordered = filtered.OrderBy(n => n.Length);
var projected = ordered.Select(n => n.ToUpper());
var limited = projected.Take(2);

foreach (var name in limited) 
{
    Console.WriteLine(name);
}
```

### Conditional Queries
Progressive building also allows for conditional logic within your queries:

```csharp
var query = names.AsQueryable();

if (shouldFilterByLength)
{
    query = query.Where(n => n.Length > 3);
}

var result = query.ToList();
```

---

## 2. The `into` Keyword
The `into` keyword is used in **Query Expressions** to continue a query after a `select` or `group` clause (which normally terminate the query).

```csharp
var resultQuery = from n in names
                  where n.Length > 0
                  select n
                  into n1 // Start a new query scope
                  orderby n1.Length
                  select n1.ToLower();
```

> [!IMPORTANT]
> When you use `into`, the variables from the previous scope (like `n` in the example above) are no longer accessible. Only the new range variable (`n1`) is visible from that point forward.

---

## 3. Query Wrapping
A progressively built query can be "wrapped" into a single statement by placing one query expression inside parentheses within another.

```csharp
// Instead of separate variables:
var tempQuery = from n in names where n.Length > 3 select n;
var finalQuery = from n in tempQuery orderby n select n;

// You can wrap them:
var finalQuery = from n in (from n in names where n.Length > 3 select n)
                 orderby n
                 select n;
```
