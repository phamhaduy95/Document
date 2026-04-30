#### Introduction
==__Set__== is a built-in data type that stores an unordered collection of unique items. It ensure no duplicated element inside.
#### Set initiation
to initiate a ==__set__==, use __brace symbol__ (`{ }`) instead of __square bracket__ (`[ ]`).
``` python
set_1 = {1, 4, 5, 6, 2, 3, 4, 2, 3}

print(set_1)  # {1, 2, 3, 4, 5, 6}
```

besides this convention, we can create a new set using built-in `set` class as well. its constructor accepts an iterable object for its creation.
```python
set_from_list = set([1, 4, 1, 2, 3, 4])  

print(set_from_list)  # {1, 2, 3, 4}
```

For complex data type, we must explicitly define the comparison logic for ensuring the uniqueness among its items.

In addition to uniqueness, ==__Set__== implements hash table internally, which help removing, adding or retrieving operation take near linear time. This make `set` excellent choice to manage collection of unique elements or to perform data lookup.

To verify element stored inside a `Set`
```python
personnel = {"Jou", "Hahn", "kahn", "Violet", "Tim", "Hugh"}
print("Jou" in personnel) # True
```

To update set with a collection of new elements
```python
new_personnel = {"Tim", "Kaka", "Vance"}
personnel.update(new_personnel)
print(personnel) # {'kahn', 'Hahn', 'Violet', 'Vance', 'Jou', 'Kaka', 'Tim', 'Hugh'}
```

> [!note] The update function inserts new items into the existing set directly.
