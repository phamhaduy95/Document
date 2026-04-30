# LINQ Overview

**LINQ** (Language Integrated Query) is a set of language and runtime features for writing structured, type-safe queries over local object collections and remote data sources.

To use LINQ on a local collection, the collection must implement the `IEnumerable<T>` interface.

---

## 1. Writing LINQ Queries
There are two main ways to write LINQ queries in C#: **Fluent API** and **Query Expressions**. Both require importing the `System.Linq` namespace.

### Fluent API (Method Syntax)
The Fluent API uses extension methods that can be chained together. It requires lambda expressions to define operations.

```csharp
string[] names = { "Hung", "Cao", "Trung", "Binh", "Dung" };

// Filter names longer than 4 characters
var filteredNames = names.Where(n => n.Length > 4);

// Chaining methods
var firstName = names.Where(n => n.StartsWith("H"))
                     .OrderBy(n => n)
                     .First();
```

### Query Expression (Query Syntax)
Query expressions provide a SQL-like syntax. They are often cleaner for complex queries but have a more limited set of available operators.

```csharp
var nameQuery = from n in names
                where n.Length > 3
                orderby n
                select n;
```

> [!NOTE]
> Query expressions always start with a `from` clause and must end with a `select` or `group` clause.

---

## 2. Deferred Execution (Lazy Loading)
An important feature of LINQ is that most queries do **not** execute when they are defined. Instead, they execute when the results are actually enumerated (e.g., in a `foreach` loop).

```csharp
var numbers = new List<int> { 1, 2, 3 };
var query = numbers.Select(n => n + 20);

numbers[1] = 10; // Modify the original list

foreach (var n in query) 
{
    Console.WriteLine(n); // Reflects the modification: 21, 30, 23
}
```

### Exceptions to Deferred Execution
Some operators trigger **immediate execution**:
- **Aggregation operators**: `Count()`, `First()`, `Last()`, `Average()`, `Sum()`, etc.
- **Conversion operators**: `ToList()`, `ToArray()`, `ToDictionary()`.

---

## 3. Subqueries
A subquery is a query contained within the expression of another query.

```csharp
var query = from s in students
            where s.Age == (from s2 in students 
                             orderby s2.Score 
                             select s2).Last().Age
            select s;
```

---

## 4. LINQ for Entity Framework Core
While LINQ to Objects works with `IEnumerable<T>`, Entity Framework Core uses `IQueryable<T>`. This allows the LINQ provider to translate your C# code into optimized SQL queries that execute on the database server.
