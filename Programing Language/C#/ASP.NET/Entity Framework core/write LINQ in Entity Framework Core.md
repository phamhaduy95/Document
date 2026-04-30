# Writing LINQ in Entity Framework Core

Entity Framework Core allows you to write C# LINQ queries that are automatically translated into SQL and executed on the database server. This provides a type-safe way to interact with your data.

---

## 1. Basic Queries
You can use standard LINQ operators like `Where`, `Select`, `OrderBy`, and `Include`.

```csharp
using var context = new MyDbContext();

// Get all blogs containing "C#" in the name, ordered by name
var blogs = context.Blogs
    .Where(b => b.Name.Contains("C#"))
    .OrderBy(b => b.Name)
    .ToList();
```

---

## 2. Joins and Relationships

### Eager Loading (`Include`)
Use `Include` to load related entities in a single query (this results in a SQL `JOIN`).
```csharp
var blogsWithPosts = context.Blogs
    .Include(b => b.Posts)
    .ToList();
```

### Manual Joins (Query Syntax)
For complex joining logic across unrelated tables, you can use the `join` keyword.
```csharp
var query = from b in context.Blogs
            join p in context.Posts on b.BlogId equals p.BlogId
            select new { BlogName = b.Name, PostTitle = p.Title };
```

---

## 3. Projections
Projecting into an anonymous type or a DTO is more efficient than loading the entire entity because it only selects the columns you actually need.

```csharp
var postSummaries = context.Posts
    .Select(p => new { p.Title, p.Summary, p.AuthorName })
    .ToList();
```

---

## 4. Grouping and Aggregation
EF Core translates `GroupBy` and aggregate functions (like `Sum`, `Count`, `Average`) directly into SQL `GROUP BY` statements.

```csharp
var postCountsByBlog = context.Posts
    .GroupBy(p => p.BlogId)
    .Select(group => new { 
        BlogId = group.Key, 
        Count = group.Count() 
    })
    .ToList();
```

---

## 5. Subqueries

### Uncorrelated Subquery
A subquery that can be evaluated independently.
```csharp
var latestBlogs = context.Blogs
    .Where(b => b.BlogId > context.Blogs.Average(x => x.BlogId));
```

### Correlated Subquery
A subquery that refers to columns in the outer query.
```csharp
var result = context.Blogs
    .Select(b => new {
        b.Name,
        // Subquery to get only the most recent post for this blog
        LatestPost = b.Posts.OrderByDescending(p => p.PublishedDate).FirstOrDefault()
    });
```

---

## 6. Client vs. Server Evaluation
EF Core attempts to evaluate as much of the query on the database server as possible. If a part of the query cannot be translated to SQL, EF Core will throw an exception (in modern versions) or switch to client-side evaluation (older versions).

> [!WARNING]
> Avoid using complex C# methods or custom logic inside LINQ queries that must be translated to SQL, as they will likely trigger a runtime error or cause significant performance issues.
