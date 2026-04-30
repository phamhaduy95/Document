Initially [**PEP 484**](https://peps.python.org/pep-0484/) defined the Python static type system as using _nominal subtyping_.
This is known as _structural subtyping_ (or static duck-typing
bottom type is a subtype of all other type.
 
 serve as type hint for developer but not used by Python complier or run time

in order to use type annotation in python, you should
1. remember all available types from Python
2.

naming convention
for function and variable: use lowercase with words separated with underscore
since Error is class, use CapWord with  suffix Error: 
### [Blank Lines](https://peps.python.org/pep-0008/#blank-lines)

Surround top-level function and class definitions with two blank lines.

Method definitions inside a class are surrounded by a single blank line.


### [Overriding Principle](https://peps.python.org/pep-0008/#overriding-principle)

Names that are visible to the user as public parts of the API should follow conventions that reflect usage rather than implementation.

Threads require extra operating system resources to create, such as preallocated,  
per-thread stack space that consumes process virtual memory up front.

### `if __name__ == '__main__':`

This line checks whether the current Python file is being run **directly** by the Python interpreter, rather than being **imported** as a module in another script.

- `__name__` is a special built-in variable in Python.
    
- If the script is being **run directly**, `__name__` is set to `'__main__'`.
    
- If the script is being **imported**, `__name__` is set to the **name of the module** (i.e., the filename without `.py`).
    

This is a common Python idiom that protects code from being run when the module is imported.