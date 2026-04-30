Introduction

PLINQ is the LINQ but operated in parallel fashion. It automatically
divides the input sequence into several chunks of elements, processes
them concurrently and then collate them into one output sequence.

To transform regular LINQ to PLINQ, add ".AsParallel()" to the input
sequence.

For query expression.

var query = from n in numbers.AsParallel()

where n \> 20;

select n;

for fluent syntax.

var query = numbers.AsParallel().Where(n =\> n \> 20).select(n =\> n);

By default, the PLINQ doesn't preserve the initial order of the input
sequence into the output sequence . Yet, you can enforce the PLINQ to do
so by put the AsOrdered() following AsParallel(). For example.

var orderedQuery = sequence.AsParallel().AsOrdered().Where(n=\>n%2 ==
0).select( n = \> n+1);

Enforcing the order for PLINQ can throttle the overall performance of
unordered parallel query as it requires some overhead and checking to
ensure each element being put in right order.

Limitation of PLINQ

The PLING is only appliable to local collection and unusable in entity
framework.

Not all LINQ operation used in PLING is optimized and may lower the
performance.

- Select, SelectMany are implemented efficiently for parallel operation.

- Join, GroupBy, GroupJoin, Distinct when be used in parallel fashion
  can sometimes be slower than regular sequence ones.

- All built-in Aggregate operations such as Max, Min, Average, Sum
  perform well but not for custom Aggregate ones as its performance
  depends on its own implementation by users.

Optimizing PLINQ

There are some optimization techniques you can utilize to boost the
performance for PLINQ.

One of PLINQ's advantages is that it conveniently collates the results
from parallelized work into a single output sequence. However, we
sometimes don't want this mechanism to happen and instead process each
chunk of data independently.

To prevent this default mechanism, you use ForAll() method which hooks
directly into PLINQ's internals, bypassing the steps of collating and
enumerating the results.

\"abcdef\".AsParallel().Select (c =\> char.ToUpper(c)).ForAll
(Console.Write);
