# Parallel LINQ (PLINQ)

PLINQ is a parallel implementation of LINQ. it automatically partitions the input sequence into several chunks, processes them concurrently on multiple threads, and then collates the results into a single output sequence.

## Enabling PLINQ
To transition from standard LINQ to PLINQ, call `.AsParallel()` on the input sequence.

### Query Expression
```csharp
var query = from n in numbers.AsParallel()
            where n > 20
            select n;
```

### Fluent Syntax
```csharp
var query = numbers.AsParallel()
                   .Where(n => n > 20)
                   .Select(n => n);
```

---

## Ordering in PLINQ
By default, PLINQ does not guarantee that the order of the output sequence will match the input sequence. You can enforce ordering by using the `.AsOrdered()` method.

```csharp
var orderedQuery = sequence.AsParallel()
                           .AsOrdered()
                           .Where(n => n % 2 == 0);
```

> [!WARNING]
> Enforcing order can significantly reduce performance due to the overhead of tracking and sorting elements across multiple threads.

---

## Limitations
- **Local Only**: PLINQ is only applicable to local collections (LINQ to Objects). It cannot be used with database providers like Entity Framework Core.
- **Overhead**: For small collections or simple operations, the overhead of partitioning and threading can make PLINQ slower than sequential LINQ.
- **Operation Performance**:
    - **Fast**: `Select`, `SelectMany`, and built-in aggregations (`Sum`, `Min`, `Max`).
    - **Potentially Slower**: `Join`, `GroupBy`, `Distinct` (due to the need for cross-thread coordination).

---

## Optimizing with ForAll
The `.ForAll()` method allows you to process results as soon as they are ready on their respective threads, bypassing the final collation step.

```csharp
"abcdef".AsParallel()
        .Select(c => char.ToUpper(c))
        .ForAll(Console.Write);
```
