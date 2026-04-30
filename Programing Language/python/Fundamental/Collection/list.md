
#### Built-in list methods 

Python offers handful of methods out of the box to deal with several important operations on list object such as:
 1. __Sorting__ the list
 2. __Appending__ or __removing__ item from both ends of the list
 3. __Reversing__ the list  

```python
list_1 = [2, 32, 4, 5]
list_1.sort()

print(list_1)  # [2, 4, 5, 32]

list_2 = [2, 3, 4, 5]
list_2.reverse()

print(list_2) # [5, 4, 3, 2]
```
These methods, however, modify directly the original list which can make our program more prone to bugs. To work around this limitation, you can utilize their stand alone counterparts which return a new list and leave the origin intact.
```python
list_1 = [2, 32, 4, 5]
sorted_list = sorted(list_1)

print(f"original list= {list_1}, sorted list= {sorted_list}" )  
# original list = [2, 32, 4, 5], sorted list = [2, 4, 5, 32]

list_2 = [2, 3, 4, 5]  # [5, 4, 3, 2]
reversed_list = list(reversed(list_2))

print(list_2, reversed_list)  # [2, 3, 4, 5][5, 4, 3, 2]
```

#### Query the length of the list

``` python
original_list = [1, 2, 4, 5, 3]

print(len(original_list)) #5
```
python `list` object does not have its own method for acquire its length but instead rely on built-in function `len()` to do so.
`len()` is often used with  __range__() to iterate through a typical list via item's index

#### Combining multiple lists into single list

``` python
list_1 = [1, 2, 3, 4]

list_2 = [5, 6, 7]

combined_list = [*list_1, *list_2, 8] # [1, 2, 3, 4, 5, 6, 7, 8]
```
The __unpacking operator ( * )__ does not just work with __list__ but also any iterable object such as __set__, __tuple__ and  generator

``` python
original_list = [1, 2, 4, 5, 3]

from_generator = (item + 2 for item in original_list)

from_set = set([1, 3, 3, 4])

print([*from_set]) # [1, 3, 4]

print([*from_generator]) # [3, 4, 6, 7, 5]
```
#### Extracting subsequence from list
``` python
original_list = [1, 2, 4, 5, 3]

sub_list = original_list[0:3] #[1, 2, 4]
```

>[!warning] The item at __end__ index will be excluded from the result subsequence

The array produced from this operator is a completely new array, hence, don't need to worry about accidental mutation.

It 's possible to modify a list segment as well. 
``` python
my_list = [1, 2, 3, 4, 5]  
print(f"Original list: {my_list}")  
  
# Replace a slice with a new sequence  
my_list[1:4] = [10, 20, 30]  
print(f"List after slice assignment: {my_list}")  
  
# Replace a slice with a shorter sequence (removes elements)  
my_list[0:2] = [0]  
print(f"List after replacing with shorter sequence: {my_list}")  
  
# Replace a slice with a longer sequence (inserts elements)  
my_list[1:1] = [50, 60] # Inserts at index 1 without replacing anything  
print(f"List after inserting with slice assignment: {my_list}")
```
#### List iteration

- To iterate through list items via their indices, you can use the combination of len and range. This loop statement is equivalent to for-loop with index access in C family languages.
``` python
original_list = [1, 2, 4, 5, 3]

for index in range(len(original_list)):
    print(original_list[index])
```
- You can enumerate list items directly with out index access as well.
```python
for item in original_list:
    print(item)
```

- In order to acquire both item and its index,  `enumerate` can be used to transform list into a sequence of tuple which includes both item  index and value. 
```python
seasons = ["Spring", "Summer", "Fall", "Winter"]

list(enumerate(seasons)) 
#[(0, "Spring"), (1, "Summer"), (2, "Fall"), (3, "Winter")]

list(enumerate(seasons, start=1))
#[(1, "Spring"), (2, "Summer"), (3, "Fall"), (4, "Winter")]

for index, season in enumerate(seasons):
    print(index, season)

```
#### List initiation
There are several ways to initiate a list beside a conventional approach. One of which rely on `list` class default constructor method.
``` python
empty_list = list() # the same with []

from_iterable =list((1, 2, 3))
```

If you want to create a list with predefined size and default value, consider using multiply operator __( * )__ :
```python
list_1 = [0] * 4  #[0, 0, 0, 0]
```

Last but not least, list creation can also be done with list comprehension. For example, the output from above statement can be achieved similarly via list comprehension. 
``` python
list_1 = list_1 = [0 for item in range(5)] * 4  #[0, 0, 0, 0]
```

The primary benefit of this syntax is the ability to carry out filtering, mapping and data transforming in one concise and intuitive statement. In spite of its simple syntax, list comprehension is well-known for its fast performance among any regular approach.
```python
original_list = [1, 2, 4, 5, 3, 3, 4, 2]

processed_list = list(map(lambda x: x * 3, filter(lambda x: x > 2, original_list)))

print(processed_list)  # [12, 15, 9, 9, 12] 

list_from_comprehension = [number * 3 for number in original_list if number > 2]

print(list_from_comprehension)  # [12, 15, 9, 9, 12]
```
#### Working with List of object
All example I have covered use simple list of integers as demo subject. However, in reality, developers often deal with collection of more complex data type such as object. Fortunately, many list methods work fine with this data type without any extra step. However, there are few exceptions that requires additional data to function properly.
For instance, ==`sorted()`== function needs to know which value from the object for ordering. In the example below, I explicitly tell python to sort student list base on grade.  
```python
@dataclass
class Student:
    name: str
    grade: int
    def __repr__(self) -> str:
        return f"{self.name}: {self.grade}"

students = [
    Student(name="John", grade=10),
    Student(name="Sam", grade=4),
    Student(name="Henry", grade=5),
    Student(name="Tom", grade=1),
    Student(name="Hahn", grade=7),
]

leaderBoard = sorted(students, key=lambda student: student.grade, reverse=True)

print(leaderBoard)  # [John: 10, Hahn: 7, Henry: 5, Sam: 4, Tom: 1]
```

This technique can be also applied to other built-in aggregation function such as ==`max()`== , ==`min()`== 
``` python
best_student = max(students, key=lambda student: student.grade)
print(best_student)  # John: 10

worst_student = min(students, key=lambda student: student.grade)
print(worst_student)  # Tom: 1
```