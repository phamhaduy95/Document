#### Introduction
`dataclass` is a python decorator which simplifies class declaration. It automatically generate necessary `dunder` methods such as `__init__`, or `__equal__` which allows developers to easily control class's initiation, comparison and immutability.

`dataclass` can be found within the dataclasses built-in module.

you can provide additional parameters for `dataclass` to configurate further
```python
@dataclass(
    init=True,
    repr=True,
    eq=True,
    order=False,
    unsafe_hash=False,
    frozen=False,
    match_args=True,
    kw_only=False,
    slots=False,
    weakref_slot=False,
)

class C: ...
```
#### Object initiation
`dataclass` automatically creates `__init__` method base on data fields declared in a class. 
The order of  field defined in class determines the order of parameter  in  `__int__` method.
To make one specific field optional, provide default value for it. 
```python
from dataclasses import dataclass
from uuid import UUID, uuid4

@dataclass
class Point:
    x: int
    y: int
    id: UUID = uuid4() # optional field


class RegularPoint:
    x: int
    y: int
    id: UUID

    def __init__(self, x: int, y: int, id=uuid4()) -> None:
        self.x = x
        self.y = y
        self.id = id

point_1 = Point(x=2, y=4)
```

In this example, The class Point using `dataclass` looks cleaner and more concise than regular one  as  the `__init__` method is no longer needed.

#### Object comparison 
if eq=True:

`dataclass` 





when set frozen=True as `dataclass` parameter, then `dataclass` will add `__setAtt__` and `__delAtt__` method to the class. These method will raise error whenever they are invoked (user try to modify value or delete any class field)



