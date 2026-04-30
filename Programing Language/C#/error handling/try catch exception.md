**try-catch clause**

try-catch statement is used as the main tool for error handling in C#.
try-catch statement, as its name, consists of two parts. The first is
try block which contains the code which potentially has errors and the
second is the catch clauses is where the code error handling resides.
The typical try catch statement in your code may has the structure
below.

try {

> \... // exception may get thrown within execution of this block192

}

catch (ExceptionA e) {

> \... // handle exception of type ExceptionA

}

catch (ExceptionB e) {

> \... // handle exception of type ExceptionB

}

The try-catch structure may possess more than one catch block following
one try statement for our code since code may introduce more than one
type of errors each of which has its own corresponded Exception type.

A catch clause also has access to an Exception object that contains
information about the error.

try{

byte b = byte.Parse (args\[0\]);

Console.WriteLine(b);

}

catch(IndexOutOfRangeException) {

> Console.WriteLine(\"Please provide at least one argument\");

}

catch(FormatException) {

Console.WriteLine(\"That\'s not a number!\");

}

catch(OverflowException) {

Console.WriteLine(\"You\'ve given me more than a byte!\");

}

All Exception objects are the subclasses from the System.Exception.
There are plentiful of types whose name are expressive and meaningful
about the error program encounters.

As rule of thumbs, always let the more specific Exception to be listed
first in catch clause then the most general one to be last. As result,
you should put the code for handling System.Exception last in the chain
of catch clause.

**finally clause**

Any code in finally clause will be executed no matter the code is
interrupted by the error or not. It mostly used for cleaning up any
system resource which is registered in the try block. Without the
finally clause, whenever there is any exception throwed, any code right
after the break point won't be executed so the supposed code for
resource clean-up won't be reached and memory leaks may happen.

void ReadFile () {

StreamReader reader = null; // In System.IO namespace

try {

reader = File.OpenText(\"file.txt\");

if (reader.EndOfStream) return;

Console.WriteLine(reader.ReadToEnd());

}

> finally {

if (reader != null) reader.Dispose();

}

}

Like the example above, stream reader open data stream from the file in
the filesystem. To safely close the file and release the resource back
to the system, we put any code, where we dispose the reader object, in
the finally clause

**Using statement**

It may not convenient to have each finally clause for every statement
which requires access to system resources. C# offers us a shorthand to
make the process of freeing resource automatic.

using (StreamReader reader = File.OpenText(\"file.txt\")) {

}

The code has using statement is equivalent the code using file above.

Any object which implements interface IDisposable may be allowed to be
used in using statement.
