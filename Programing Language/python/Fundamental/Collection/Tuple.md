#### Introduction
Tuple is an immutable sequence that is often used for storing mixed-type (heterogeneous) data. Although it shares many functionalities with a list such as random access and item ordering, the primary difference is its immutability.

To create a tuple,  place elements in a parentheses (`()`)  instead of square brackets. 
#### Applications
what makes a tuple different from a list is its special generic typing definition. With a list, we can only declare a single type for every list element while tuples allow us to declare typing for each individual element. This makes tuples ideal for defining functions that return multiple data type.

``` python
def generateTuple()->tuple[int,str]:
	return (2,'text')

# consume returned tuple via deconstructuring operator
number, text =  generateTuple()
print(number,text) # 2 text
```

#### Name tuples 
`Named tuples` are the variant of standard tuples that allow accessing elements by a descriptive name instead of numerical index like regular tuple. This feature helps code more readable and self-explanatory.
Unlike regular tuples, `Named tuples` are not a built-in data type and must be imported from `collection` module. 

