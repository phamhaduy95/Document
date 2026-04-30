# The Parallel Class in C#

The `Parallel` class in the Task Parallel Library (TPL) provides a basic form of structured parallelism through three primary static methods:

- **Parallel.Invoke**: Executes multiple delegates in parallel.
- **Parallel.For**: The parallel equivalent of a standard `for` loop.
- **Parallel.ForEach**: The parallel equivalent of a standard `foreach` loop.

> [!IMPORTANT]
> All three methods are **blocking** (they wait until all work is complete). They are optimized for **compute-bound** tasks. Do not use them for long-running I/O-bound operations (like web requests), as they can saturate the thread pool.

---

## Parallel.Invoke
`Parallel.Invoke` allows you to execute an array of `Action` delegates simultaneously. It is highly efficient when dealing with a large number of actions.

```csharp
Parallel.Invoke(
    () => Console.WriteLine("First Task"),
    () => Console.WriteLine("Second Task")
);
```

Since `Action` delegates do not return values, you should use thread-safe collections like `ConcurrentBag<T>` to collect results from parallel actions.

```csharp
var outputBag = new ConcurrentBag<int>();
var inputs = new int[] { 1, 2, 3, 4, 5 };

Parallel.Invoke(
    () => outputBag.Add(inputs.Sum()),
    () => outputBag.Add(inputs.Max())
);
```

---

## Parallel.For and Parallel.ForEach
These methods perform iterations in parallel. Note that **order is not preserved** during execution.

### Parallel.For Example
```csharp
Parallel.For(0, 100, i => {
    Console.WriteLine($"Processing index {i}");
    Thread.Sleep(10); // Simulate work
});
```

### Parallel.ForEach Example
```csharp
var numbers = new List<int> { 1, 3, 4, 5, 6 };

Parallel.ForEach(numbers, (value, state, index) => {
    Console.WriteLine($"Element {index} is {value}");
});
```

---

## Controlling Execution with ParallelLoopState
You can break or stop a parallel loop using the `ParallelLoopState` object provided to the lambda expression.

- **Break()**: Ensures all iterations that were started before this one (in sequence) are completed.
- **Stop()**: Stops the loop as soon as possible, without regard for other iterations.

```csharp
Parallel.For(0, 200, (i, state) => {
    if (i == 10) state.Break();
    Console.WriteLine(i);
});
```
