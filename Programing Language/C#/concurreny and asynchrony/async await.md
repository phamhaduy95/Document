Async and wait

Prior to the introduction of Async and Await syntax, if you want to
register any action which will be executed immediately after one
asynchronous operation finishes, you can count on Awaiter object which
can be obtained via GetAwaiter from any Task object.

Suppose we have asynchronous operation:

Task\<int\> GetSumAsync(IEnumerable\<int\> numbers)

{

return Task.Run(() =\> {

int result = 0;

foreach (var n in numbers)

{

result += n;

Thread.Sleep(100);

}

return result;

});

}

We want to print the result of the getSumAsync right after it finishes
its execution. Then we can follow the code below.

void displaySumResult()

{

var awaiter = GetSumAsync(new List\<int\> { 1, 2, 3, 4, 5, 6
}).GetAwaiter();

awaiter.OnComplete(() =\> {

int result = awaiter.GetResult();

Console.WriteLine(result);

});

}

To facilitate this process, async and await was added to C# as standard
syntax of C#. The program above can be rewritten using new syntax.

async void displaySumResult()

{

int result = await getSumAsync(new List\<int\> { 1, 2, 3, 4, 5, 6 });

Console.WriteLine(result);

}

Using this syntax help the code much shorter and more readable.

Note: The await keyword is only usable in the function which is given
async modifier.

The real power of await expressions is that they can appear almost
anywhere in code. Specifically, an await expression can appear in place
of any expression (within an asynchronous function) except for inside a
lock expression or unsafe context.

**Creating asynchronous function.**

Async function may return two types of value. The first type is
Task\<T\>

**async** Task DoSomething () {

**await** Task.run( ()=\>{ Thread.Sleep(200)});

}

To enable asynchronous method to return value, switch to Task\<TResult\>
instead of Task.

**async** Task\<int\> GetLongWaitingValue () {

**await** Task.Delay(2000);

int result = 20;

**return** result;

}

You don't need to explicitly return any Task object, the complication of
CLR will do it for you automatically.

Having asynchronous method defined this way has one unignorable
advantage, which grants us the ability to chain multiple asynchronous
operation together.

**async** Task PrintValue () {

var result = **await** GetLongWaitingValue();

Console.WriteLine(result);

}

**async** Task DoSomeThingAfter () {

**await** PrintValue();

Console. WriteLine("Finish Print Value");

}

**Asynchronous Lambda Expressions.**

Beside the regular declaration, you can define a asynchronous method
using lambda expression.

Func\<**Task**\> unnamed = **async** () =\>\
{\
await Task.Delay (1000);\
Console.WriteLine (\"Foo\");\
};

Comparison with ContinueWith method.

ContinueWith method and async and await syntax are both the main
approach to add the continuations to

If you specify that a method is an async method by using
the [async](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/async) modifier,
you enable the following two capabilities.

- The marked async method can
  use [await](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/await) to
  designate suspension points. The await operator tells the compiler
  that the async method can\'t continue past that point until the
  awaited asynchronous process is complete. In the meantime, control
  returns to the caller of the async method.

> The suspension of an async method at an await expression doesn\'t
> constitute an exit from the method, and finally blocks don\'t run.

- The marked async method can itself be awaited by methods that call it.

An async method typically contains one or more occurrences of
an await operator, but the absence of await expressions doesn\'t cause a
compiler error. If an async method doesn\'t use an await operator to
mark a suspension point, the method executes as a synchronous method
does, despite the async modifier. The compiler issues a warning for such
methods.
