**Thread Creation**

System. Thread is the main library for thread creation and management.
To create the thread, initiate the thread object. The thread object
initialization accepts a function as an input, which will be executed
within the context of that thread.

Thread t = new Thread (myMethod);

void myMethod( ){

while(true){

Console.Writeline("do something in secondary thread");

Thread.Sleep(10)

}

While(true){

Console.writeline("do something in main thread");

}

}

Thread initialization also accept lambda expression.

Thread t = new Thread ( ()=\>{ myMethod() });

Then you call start method provided by the Thread class to signal the
thread to start.

t.start();

After the delegate method in the thread finishes executing, the thread
will be deactivated and cannot be restarted.

The OS performs multithreading differently on single-core and multi-core
environment. For single-core, the OS would allocate the small timeframe
for each thread (typically 20ms in Windows) and switch the context
between each thread to run it simultaneously. Since this happens really
fast for human perceptions, it creates an illusion of concurrency.
However, in multi-core environment, the OS can distribute each thread to
different core to execute it parallelly.

Use Thread.join() to make main thread to wait for the targeted thread to
finish executing. Be careful with the thread whose delegate function has
infinite loop as it makes main thread wait forever.

Thread.Sleep pauses the current thread for a specified period:

Thread.sleep(100) // in millisecond

Thread.sleep(new TimeSpan.FromHours (1));

Thread.Sleep(0) relinquishes the thread's current time slice
immediately, voluntarily handing over the CPU to other threads. Its is
mostly used for performance tweaks.

**Blocking thread**

The thread is considered being blocked when it is pause for some
reasons. Join and sleep are ones of them. To test whether one thread is
blocked or not, use the statement.

bool blocked = (someThread.ThreadState & ThreadState.WaitSleepJoin) !=
0;

the Thread can be blocked by shared state locking mechanic.

**Memory management in multithreading**

Each thread is assigned its own memory stack for storing any local
variable. Moreover, the thread can access and modify the variables in
the main thread too. These variables are called shared state.

**bool \_done = false;\**
new Thread (Go).Start();\
Go();\
void Go()\
{\
if (!**\_done**) { **\_done = true**; Console.WriteLine (\"Done\"); }\
}

Using shared stated across multiple thread can cause the data race which
happens when there are more than one thread trying to modify the shared
variable while it is currently used to perform some sensitive
computation in other thread. To deal with this issue, you can apply
thread locking mechanism.

C# provides us a lock statement for this purpose.

class ThreadSafe\
{\
static bool \_done;\
static readonly object \_locker = new object();\
static void Main()\
{\
new Thread (Go).Start();\
Go();\
} s\
static void Go()\
{\
**lock (\_locker)\
{\**
if (!\_done) { Console.WriteLine (\"Done\"); \_done = true; }\
**}\**
\
}

when the lock is applied to one block of code, it will only allow one
thread to access this block of code at the time. Other thread must wait
until the thread, which possesses the lock, finish its execution and
release the lock.

**Passing argument to method in the thread**

You can pass the argument to the delegate method from the Thread.Start()
method.

Thread t = new Thread (Print);\
t.Start **(\"Hello from t!\")**;\
void Print (object messageObj)\
{\
string message = (string) messageObj; // We need to cast here\
Console.WriteLine (message);\
}

By default, threads you create explicitly are foreground threads.
Foreground threads keep the application alive for as long as any one of
them is running, whereas background threads do not. After all foreground
threads finish, the application ends, and any background threads still
running abruptly terminate.

You can query or change a thread's background status using its
IsBackground property.

static void Main (string\[\] args)\
{\
Thread worker = new Thread ( () =\> Console.ReadLine() );\
if (args.Length \> 0) worker.IsBackground = true;\
worker.Start();\
}

**The Thread Pool**

Thread pool provides us with a prebuilt set of threads which can be
recycled to be used when needed, instead of creating new thread. Thread
pooling is essential to build effective multithreading programming since
thread pool can limit the total amount of threads used in program as
having too many threads can cause oversubscription, the condition where
there are more threads than the cores of the CPU.

Some important thing to be concerned when working with thread pool.

- All threads in pool are background threads.

- Thread in pools cannot be named, which make thread debugging
  inconvenient.

- Blocking thread in pool can degrade the total performance.

Thread pool in C# use the hill-climbing algorithm to manage and control
the workload assigned the its internal threads. This design approach for
thread pools works quite efficiently in many situations. However, there
are some tips to ensure the desired performance for thread pool.

- Work items assigned to thread in pool should be short-running (250ms
  at most and ideally 100ms).

- Work items that spend most of the time blocked do not dominate the
  pool (lowest priority).

**Disadvantage of thread approach**

Thread provides us a reliable low-level tool to perform concurrent and
parallel programming. However, Thread also has some noticeable
shortcoming.

- It is difficult to transfer the return value from one thread to the
  main thread. The only viable option is having the shared state to hold
  the result to passed back to the main thread. Yet, dealing with data
  race and exception for shared state is painful.

- You can't tell a thread to start something else when it's finished;
  instead, you must Join it (blocking your own thread in the process).
