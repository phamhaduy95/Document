Namespace System.Threading.Tasks comprises these classes to support
concurrency and parallelism.

![](media/image1.png){width="6.5in" height="2.3375in"}

**Task**

Task is flexible high-level abstraction for leverage multithreading in
your program.

By default, Task is implemented using Thread pools to handle concurrent
tasks. However, you can switch the Task implementation to traditional
callback approach to deal with heavy IO-bond operation.

Compared to the low-level thread, Task offers some great advantages over
its counterpart.

- flexible to switch to suitable mode for different type of concurrent
  problem (Thread pool for Compute-bond, callback for IO-bond).

- automatically rethrow the exception to main thread when Wait() or
  Result() is called.

**Starting the task**

The easiest way to start a Task backed by a thread is with the static
method Task.Run (the Task class is in the System.Threading.Tasks
namespace), simply pass in an Action delegate:

Task task = Task.Run (() =\> Console.WriteLine (\"Foo\"));

**Waiting the task**

Initially, the delegate given to the Task object is on pause. Call Wait
will trigger the Task to execute the delegate contained within it and
will block the thread where it is located until its completes the
operation.

Task task = Task.Run (() =\> {\
Thread.Sleep (2000);\
Console.WriteLine (\"Foo\");\
});\
Console.WriteLine (task.IsCompleted); // False\
task.Wait(); // Blocks until task is complete.

Once task.Wait() is completed, you cannot rerun its again. To execute
the delegate one more time, initiate the task again.

**Getting return value from task**

Beside non-generic Task, C# also has a generic subclass called
Task\<TResult\>, which allows a task to emit a return value. You can
obtain a Task\<TResult\> by calling Task.Run with a Func\<TResult\>
delegate (or a compatible lambda expression) instead of an Action.

(The Task\<TResult\> is analog to "Future", the term used in other
language such as Java).

> Task\<int\> task = Task.Run (() =\> {
>
> Console.WriteLine (\"Foo\");
>
> return 3;
>
> });

Then you can use Task.Result property in the main thread to acquire the
result. However, using Result property will block the main thread until
the result is returned.

int result = task.Result; // Blocks if not already finished\
Console.WriteLine (result); // 3

**Handling Exception**

Unlike with threads, tasks conveniently propagate exceptions. So, if the
code in your task throws an unhandled exception (in other words, if your
task faults), that exception is automatically rethrown to whoever calls
Wait()---or accesses the Result property of a Task\<TResult\>:

// Start a Task that throws a NullReferenceException:\
Task task = Task.Run (() =\> { throw null; });\
try{\
task.Wait();\
}catch (AggregateException ex) {\
if (ex.InnerException is NullReferenceException)\
Console.WriteLine (\"Null!\");\
else\
throw;\
}

**Continuations**

You can assign callback to be happen right after the thread operation
finishes its execution. There are two methods to achieve this.

Assign delegate to Awaiter object:

Task\<string\> task = Task.Run(()=\>{

> Thread.sleep(1000);
>
> return "returned message from thread"

})

// call GetAwaiter to get Awaiter object from Task.

var awaited = task.GetAwaiter();

> // the Awaiter object contains OnCompleted method for registering the
> delegate.

awaiter.OnCompleted (()=\>{

> String mesg = awaiter.GetResult();

Console.WriteLine(mesg);

});

This approach is cumbersome and tedious, prefer using async and await
syntax for simplicity or ContinueWith for more control.

Using ContinueWith:

You can use ContinueWith method to assign the delegate directly without
using intermediate Awaiter object.

task1.ContinueWith((T) =\> { Console.WriteLine(\"1\"); }); // the T here
is represent for task1.

By default, the continuation created through ContinueWith may be on a
different thread. To force continuation executed in the same thread with
the original task, provide TaskContinuationOptions.ExecuteSynchronously
to it.

task.ContinueWith((T) =\> { Console.WriteLine(\"1\");
},TaskContinuationOptions.ExecuteSynchronously);

you can serialize multiple tasks in one statement using
Task.ContinueWith.

int initialValue = 0;

Task\<int\> task1 = Task.Run(() =\>{

Thread.Sleep(200);

Console.WriteLine(\"the attendant\");

return initialValue + 1;

});

int result = task1.ContinueWith((task) =\>{

Thread.Sleep(100);

Console.WriteLine(\"the first continuation \");

return task.Result + 1;

}).ContinueWith((task) =\> {

Thread.Sleep(100);

Console.WriteLine(\"the second continuation \");

return task.Result + 2; // the task here is the first continuation task
resulted from task1

}).ContinueWith((task) =\>{

Thread.Sleep(100);

Console.WriteLine(\"the last continuation \");

return task.Result + 2;

}).Result;

Console.WriteLine(\$\"final result is {result}\"); // final result is 6.

You can have multiple continuations operated parallelly from single
task.

Task task2 = Task.Run(() =\> {

Thread.Sleep(200);

Console.WriteLine(\"task completed\");

});

task2.ContinueWith((t) =\> {

Console.WriteLine(\"first continuation completed\");

});

task2.ContinueWith(t =\> {

Console.WriteLine(\"second continuation completed\");

});

task2.Wait();

**Cancelling the Task**

Operation executed in Task can be canceled midway. C# consists
CancellationToken class which enables us to do so.

class CancellationToken {\
public bool IsCancellationRequested { get; private set; }\
public void Cancel() { IsCancellationRequested = true; }\
public void ThrowIfCancellationRequested(){\
if (IsCancellationRequested)\
throw new OperationCanceledException();\
}\
}

Inspect the whole CancellationToken class, it can be seen that the main
mechanism to cancel the thread used here is throwing the
OperationCancelException to force the thread to stop midway.

CancellationToken cannot be initiated directly but through the
CancellationTokenSource.

var TokenSource = new CancellationTokenSource();

var token = TokenSource.Token;

Since we use exception to cancel the Task, try catch expression is
required.

try {

Task task = Task.Run(() =\> {

> int i = 0;

while (true){

Console.Write(\$\"{i++} \");

Thread.Sleep(100);

token.ThrowIfCancellationRequested();

}

}, token); // CancellationToken is required as the second argument

Thread.Sleep(2000);

TokenSource.Cancel(); // trigger Cancel Signal to all task which uses
token

}

catch (OperationCanceledException e){

Console.WriteLine(\"The Thread has been cancelled\"); // Throw the error

}

finally{

TokenSource.Dispose(); // Dispose the TokenSource object to clean up

}

**Reporting Progress**

Sometimes, you'll want an asynchronous operation to report back progress
as it's running. The CLR provides a pair of types to solve this problem:
an interface called IProgress\<T\> and a class that implements this
interface called Progress\<T\>. Their purpose, in effect, is to "wrap" a
delegate so that UI applications can report progress safely through the
synchronization context.

**Task combinator**

Task combinators is used to gather one or many Task objects and make
them to be operated in parallel fashion. There are two primitive method
whose purpose are Task.WaitAll and Task.WaitAny.

Suppose we have three tasks which completed in different duration.

var task1 = Task.Run(() =\> {

Thread.Sleep(120);

Console.WriteLine(\"Task 1 finishes\");

});

var task2 = Task.Run(() =\> {

Thread.Sleep(200);

Console.WriteLine(\"Task 2 finishes\");

});

var task3 = Task.Run(() =\>

{

Thread.Sleep(200);

Console.WriteLine(\"Task 3 finishes\");

});

Task.WaitAll(task1, task1, task3);

Task.WaitAny waits until the first task is completed and then terminate
others task while Task.WaitAll waits all its assigned Tasks to finish.

In order to operate async function in parallel using Task.WaitAny or
Task.WaitAll, the async function must return the Task object.

async Task op1() {

await Task.Delay(200);

Console.WriteLine(\"op1 finishes\");

}

async Task op2() {

await Task.Delay(300);

Console.WriteLine(\"op1 finishes\");

}

Task.WaitAny(op1(), op2());

For collection of Task\<T\> Task with return value, you can use
Task.WhenAll and Task.WhenAny instead.

**Task Factory**

Beside Task.Run, we can initiate the Task object with
Task.Factory.StartNew static method.

Task task1 = Task.Factory.StartNew(() =\>{

Thread.Sleep(500);

Console.WriteLine(\"finish task1\");

});

Creating Task object this way allows us to carry some extra
configuration by specifying a TaskCreationOptions when calling StartNew.

Task task2 = Task.Factory.StartNew(() =\> {

Thread.Sleep(500);

Console.WriteLine(\"finish task2\");

}, TaskCreationOptions.LongRunning);

TaskCreationOptions enum offers us several options to choose from:

LongRunning: makes the Task replace the thread pool with the regular
callback assignment

AttachedToParent: set an inner Task inside one parent task to be a child
of the parent task. (apply to inner Task only).

One important application of Task.Factory.StartNew is the ability to
create the child Tasks.

Task task = Task.Factory.StartNew(() =\> {

> // create a detached Task inside one Task. The detached task is
> operated independently from the its creation.

Task.Factory.StartNew (() =\>{

Thread.Sleep(200);

Console.WriteLine(\"the detached task is finished\");

});

// create a child Task inside one parent Task.

Task.Factory.StartNew(() =\>{

Thread.Sleep(100);

Console.WriteLine(\"the child task is finished\");

},TaskCreationOptions.AttachedToParent);

Console.WriteLine(\"The parent task is finished\");

})

.ContinueWith((t)=\>{

> // the continuation is only executed when all child tasks and the
> parent task finish executing

Console.WriteLine(\" on task completed\");

});

task.Wait();
