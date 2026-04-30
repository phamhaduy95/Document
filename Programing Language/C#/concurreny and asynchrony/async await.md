# Async and Await in C#

The `async` and `await` keywords are the modern standard for writing asynchronous code in C#, making it much shorter and more readable compared to older approaches.

## Evolution: Before Async/Await
Prior to these keywords, you had to use the `GetAwaiter` object and register a callback using `OnCompleted` (or use `ContinueWith`).

```csharp
void DisplaySumResult()
{
    var awaiter = GetSumAsync(new List<int> { 1, 2, 3 }).GetAwaiter();
    awaiter.OnCompleted(() => {
        int result = awaiter.GetResult();
        Console.WriteLine(result);
    });
}
```

## The Modern Approach
With `async` and `await`, the same logic becomes synchronous-looking:

```csharp
async Task DisplaySumResult()
{
    int result = await GetSumAsync(new List<int> { 1, 2, 3 });
    Console.WriteLine(result);
}
```

> [!NOTE]
> The `await` keyword can only be used inside a method marked with the `async` modifier.

## Creating Asynchronous Functions
Async functions typically return one of the following:
- `Task`: For methods that perform an operation but return no value (equivalent to `void`).
- `Task<T>`: For methods that return a value of type `T`.
- `ValueTask<T>`: A more memory-efficient alternative for high-performance scenarios.

```csharp
async Task<int> GetLongWaitingValue() 
{
    await Task.Delay(2000); // Non-blocking delay
    return 20; // The compiler automatically wraps this in a Task<int>
}
```

### Chaining Async Methods
One of the main advantages of this syntax is the ability to chain multiple operations easily.

```csharp
async Task DoSomethingAfter() 
{
    var result = await GetLongWaitingValue();
    Console.WriteLine(result);
    Console.WriteLine("Finished operation.");
}
```

## How It Works
1. **Suspension Points**: When the compiler hits an `await` expression, it suspends the execution of the async method until the awaited task completes.
2. **Control Returns to Caller**: While suspended, control returns to the caller of the async method, allowing the application to stay responsive (e.g., UI or handling other web requests).
3. **Resumption**: Once the task is done, the method resumes from where it left off.

> [!IMPORTANT]
> Suspending a method at an `await` expression is **not** an exit. `finally` blocks do not run until the method actually completes or throws an unhandled exception.

## Asynchronous Lambda Expressions
You can also define async logic using lambdas:

```csharp
Func<Task> unnamed = async () => 
{
    await Task.Delay(1000);
    Console.WriteLine("Foo");
};
```
