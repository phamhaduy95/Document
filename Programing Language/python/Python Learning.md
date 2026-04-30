### roadmaps
1. install python and setup programming environment.
2. learn python fundamental and basic.
	1. variable and function 
	2. basic type
	3. collection
	4. closure
	5. class and OOP
	6. module 
	7. async and await
### Tool
`pyenv` : python version manager allow switching between different version of python. ([download](https://github.com/pyenv-win/pyenv-win))  
`ruff` : tool for linting and formatting python code.
`poetry` : a simple tool for

eslint will try merge config from both parent and its extended version. 

You only need to override `__new__` when:

1. You're subclassing **immutable types** like `int`, `str`, `tuple`, etc.
    
2. You want to **control instance creation** (e.g., singleton pattern, caching, object pooling).
Enum vs Literal 
Enum is a class provide run time validation 
good for global constant 

Literal : more light-weight, only check statically, great for function argument

**Python’s `dataclass` also needs a default value** to treat it as optional in the constructor


In Python, two dictionaries are considered **equal** (`==`) when:

> ✅ **They have the same keys and the same values for those keys, regardless of insertion order.**


Assertions Versus Exceptions  
Throughout this book, I will use assertions in some cases, and raise exceptions in  
other cases. When an assertion fails, it raises an AssertionError, which is a type of  
exception. This may make assertions and exceptions seem interchangeable, but I am  
actually picking one or the other intentionally.  
Assertions are not guaranteed to execute at runtime, as your code may be deployed  
with options that disable assertions. In this case, I use them for things that I always  
expect to be true, unless a developer in the system messes up. It is intended to catch  
mistakes during development, and it signals to other developers that it is up to them  
to not create a situation that fails an assertion.  
Exceptions, on the other hand, indicate to a developer that something may be possi‐  
ble due to user error or malicious actors. It is unlikely to happen, but other developers  
must be prepared to catch the exception if something goes wrong

language is composed of sequence of statement
list of statement used in 
assignments
declarations
loop
variable
function
expression: logical expresson


prior to version 3.10, python did not support official switch case statement so user had to rely on 
dictionary. With the introduction of match case statement, now developer can utilize more powerful technique
match case is base pattern matching 

Dictionary lookup tables can only do **equality** checking. However, switch/match blocks can check patterns and do captures.