# Threads in C#

The `System.Threading.Thread` class is the primary low-level tool for creating and managing threads in .NET.

## Thread Creation
To create a thread, instantiate a `Thread` object and pass it a delegate (method or lambda) to execute.

```csharp
Thread t = new Thread(MyMethod);
t.Start();

void MyMethod()
{
    Console.WriteLine("Executing on secondary thread.");
}
```

### Thread Lifecycle
Once the delegate finishes executing, the thread is deactivated and cannot be restarted.

## Thread Management
- **Thread.Sleep(ms)**: Pauses the current thread for a specified period.
- **Thread.Join()**: Blocks the current (calling) thread until the target thread finishes.
- **IsBackground**:
    - **Foreground Threads**: Keep the application alive as long as they are running.
    - **Background Threads**: Do not keep the application alive; they terminate abruptly when all foreground threads finish.

```csharp
Thread worker = new Thread(() => Console.ReadLine());
worker.IsBackground = true; // Set as background thread
worker.Start();
```

---

## Memory and Shared State
Each thread has its own **stack** for local variables, but multiple threads can access **shared state** (variables in the heap or static fields).

### Data Races
A data race occurs when multiple threads attempt to modify shared state simultaneously, leading to unpredictable results.

```csharp
bool _done = false;

void Go()
{
    if (!_done) { _done = true; Console.WriteLine("Done"); }
}
```

### Locking
Use the `lock` statement to ensure that only one thread can access a block of code at a time.

```csharp
private static readonly object _locker = new object();
private static bool _done;

static void Go()
{
    lock (_locker)
    {
        if (!_done)
        {
            Console.WriteLine("Done");
            _done = true;
        }
    }
}
```

---

## The Thread Pool
The Thread Pool provides a set of recycled threads, avoiding the overhead of creating new threads for every task. It helps prevent **oversubscription** (having more threads than CPU cores).

### Key Characteristics of Pool Threads:
- They are always **background threads**.
- They cannot be named.
- They are best used for **short-running** tasks (ideally < 100ms).

---

## Limitations of the Thread Class
While powerful, the `Thread` class has several drawbacks compared to modern **Tasks**:
1. **Difficult to Return Values**: Requires manual synchronization and shared state.
2. **No Chaining**: You cannot easily tell a thread to "do this after that" without blocking via `Join()`.
3. **Expensive**: Creating new threads consumes significant system resources.
