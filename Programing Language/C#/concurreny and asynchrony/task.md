# Tasks in C#

The `System.Threading.Tasks` namespace provides classes to support concurrency and parallelism. The `Task` class is a high-level abstraction that leverages multithreading efficiently.

## Task Overview
- **Thread Pool Integration**: By default, tasks use the Thread Pool to manage concurrent operations.
- **Exception Propagation**: Tasks automatically rethrow exceptions to the calling thread when `Wait()` or `Result` is accessed.

## Starting and Waiting for Tasks
The easiest way to start a task is with `Task.Run()`.

```csharp
// Starting a task
Task task = Task.Run(() => {
    Thread.Sleep(2000);
    Console.WriteLine("Task execution complete.");
});

Console.WriteLine(task.IsCompleted); // False
task.Wait(); // Blocks the current thread until the task is complete
```

## Returning Values
Use the generic `Task<TResult>` to return a value from a background operation.

```csharp
Task<int> task = Task.Run(() => {
    return 3;
});

// Accessing Result blocks the thread if the task isn't finished
int result = task.Result; 
Console.WriteLine(result); // 3
```

## Handling Exceptions
Tasks propagate exceptions conveniently. If a task faults, the exception is rethrown when you call `Wait()` or access `Result`.

```csharp
Task task = Task.Run(() => { throw new NullReferenceException(); });

try {
    task.Wait();
}
catch (AggregateException ex) {
    if (ex.InnerException is NullReferenceException)
        Console.WriteLine("Caught NullReferenceException!");
}
```

## Continuations
Continuations allow you to specify code that runs immediately after a task finishes.

### Using ContinueWith
The `ContinueWith` method allows you to chain tasks without blocking.

```csharp
Task<int> task1 = Task.Run(() => 10);

task1.ContinueWith(t => {
    Console.WriteLine($"Task 1 returned {t.Result}");
});
```

### Chaining Multiple Continuations
```csharp
int result = Task.Run(() => 1)
    .ContinueWith(t => t.Result + 1)
    .ContinueWith(t => t.Result + 2)
    .Result;

Console.WriteLine($"Final result: {result}"); // 4
```

## Cancelling Tasks
Use `CancellationTokenSource` and `CancellationToken` to cancel tasks midway.

```csharp
var cts = new CancellationTokenSource();
var token = cts.Token;

Task task = Task.Run(() => {
    while (true) {
        token.ThrowIfCancellationRequested(); // Throws OperationCanceledException
        Thread.Sleep(100);
    }
}, token);

cts.Cancel(); // Signal cancellation
```

## Task Combinators
Gather multiple tasks and operate on them together.

- **Task.WaitAll**: Blocks until all tasks complete.
- **Task.WaitAny**: Blocks until at least one task completes.
- **Task.WhenAll / Task.WhenAny**: Non-blocking versions that return a `Task`.

```csharp
await Task.WhenAll(task1, task2, task3);
```

## Task Factory
For advanced configurations (like `LongRunning` tasks), use `Task.Factory.StartNew`.

```csharp
Task.Factory.StartNew(() => {
    // Long running work
}, TaskCreationOptions.LongRunning);
```
