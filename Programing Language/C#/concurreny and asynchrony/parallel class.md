Introduction

PFX provides a basic form of structured parallelism via three static
methods in the Parallel class:

- Parallel.Invoke: Executes an array of delegates in parallel.

- Parallel.For: Performs the parallel equivalent of a C# for loop.

- Parallel.ForEach: Performs the parallel equivalent of a C# foreach
  loop.

All three methods block until all work is complete. These three preform
well with compute-bound not IO-bound so you should not assign any
long-running task to them such as downloading data from internet.

Parallel.Invoke

Parallel.Invoke allow to execute every delegate in Action array
parallelly. Compared with Task combinators WaitAll() , the
Parallel.Invoke has much better performances when dealing with the
Action array whose size is large (1 million for example).

Parallel.Invoke(() =\> { Console.WriteLine(\"first Task\"); },

() =\> { Console.WriteLine(\"second Task\");});

The Action delegate only allows function with no input and output
argument. To get data from outside or collect data generated inside each
Action delegate, the thread-safe shared variable is used.

ConcurrentBag\<IEnumerable\<int\>\> outputBag = new
ConcurrentBag\<IEnumerable\<int\>\>();

ConcurrentBag\<int\> inputBag = new ConcurrentBag\<int\> { 1, 2 ,5,6,8};

Action Transformation = () =\>{

var sequence = inputBag.Select(n =\> n + 2);

outputBag.Add(sequence);

};

Action TakeFirstThree = () =\> {

var sequence = inputBag.Take(2);

outputBag.Add(sequence);

};

Action FilterOut = () =\> {

var sequence = inputBag.Where(n =\> n \> 2);

outputBag.Add(sequence);

};

Parallel.Invoke(TakeFirstThree, FilterOut, Transformation);

Parallel.For and Parallel.ForEach

Parallel.For and Parallel.ForEach perform the equivalent of a C# for and
foreach loop but with each iteration executing in parallel instead of
sequentially. However, unlike its sequential counterpart, both
Parallel.For and Parallel.ForEach don't preserve the order of the
original iterated object.

Parallel.For example

for (var i = 0; i \< 100; i++) {

Console.Write(\$\"{i}\");

Thread.Sleep(100);

}

/\*\* The parallel version for the regular sequential \"for\" iteration
above\*/

Parallel.For(0, 100, (i) =\> {

Console.Write(\$\"{i}\");

Thread.Sleep(100);

});

Parallel.ForEach example

List\<int\> numbers = new List\<int\> {1, 3, 4, 5, 5, 6};

Parallel.ForEach(numbers, (value, state, index) =\> {

Console.Write(\$\"element {index} is {value}\");

});

The lambda expression used in Parallel.For and Parallel.ForEach may
accept three input arguments.

value: the value of iterated element.

state: the ParallelLoopState object used to break or stop the iteration
manually

index: the index of the iterated element (only exists in
Parallel.ForEach).

public class ParallelLoopState {\
public void Break();\
public void Stop();\
public bool IsExceptional { get; }\
public bool IsStopped { get; }\
public long? LowestBreakIteration {get; }\
public bool ShouldExitCurrentIteration { get; }\
}

You can use break or continue inside Parallel.For and Parallel.ForEach
like regular sequential ones with the help of ParallelLoopState object.

Parallel.For(0, 200, (value, state) =\> {

if (value == 10) state.Break();

Console.WriteLine(value);

});
