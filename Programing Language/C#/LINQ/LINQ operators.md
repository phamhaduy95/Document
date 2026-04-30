# LINQ Standard Query Operators

LINQ operators can be divided into several functional categories. This guide covers the most commonly used operators for local collections.

## 1. Filtering
Filtering operators return a subset of the original elements that satisfy a condition.

- **Where**: Returns elements that satisfy a predicate.
- **Take / Skip**: Takes or skips the first `n` elements.
- **TakeLast / SkipLast**: Takes or skips the last `n` elements.
- **Distinct / DistinctBy**: Removes duplicate elements.

```csharp
List<int> numbers = new List<int> { 1, 3, 5, 6, 3, 2, 3, 4 };

// Filter elements > 3
var filtered = numbers.Where(v => v > 3); // 5, 6, 4

// Indexed filtering
var indexed = numbers.Where((v, index) => v > 3 && index < 5);
```

## 2. Projection
Projection operators transform each element into a new form.

### Select
Transforms each element using a lambda function.

```csharp
var names = new List<string> { "Hung", "Cao", "Tung" };
var upperNames = names.Select(n => n.ToUpper());
```

### SelectMany
Concatenates subsequences into a single flat output sequence. Useful for flattening nested collections.

```csharp
var listOfArrays = new List<int[]> { new[] { 1, 2 }, new[] { 3, 4 } };
var flattened = listOfArrays.SelectMany(x => x); // 1, 2, 3, 4
```

## 3. Joining
Joining operators correlate elements from two different data sources.

- **Join**: Performs an inner join.
- **GroupJoin**: Performs a grouped join (useful for hierarchical data).

## 4. Ordering
- **OrderBy / OrderByDescending**: Sorts elements in ascending or descending order.
- **ThenBy / ThenByDescending**: Adds subsequent sorting criteria.

## 5. Grouping
- **GroupBy**: Groups elements that share a common key.

## 6. Aggregation
- **Count / LongCount**: Returns the number of elements.
- **Sum / Min / Max / Average**: Performs numerical calculations.

> [!WARNING]
> Be careful with `.Count()`. In some LINQ providers (like EF Core), it may trigger an expensive database query to evaluate the entire sequence just to get a number.
