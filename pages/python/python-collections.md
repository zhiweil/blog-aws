---
title: Collections
tags: [python]
keywords: python
last_updated: March 23, 2019
summary: "Python collections such as list, set and etc."
sidebar: mydoc_sidebar
permalink: python-collections.html
folder: python
---

## List
### Basics
* use square brackets to define list such as
    ```text
    [element1, element2, ...]
    ```
* use array style to access each element such as list[index]. the "index" value can be negative.
* A slice of list by:
    ```text
    list[start-index:end-index]
    ``` 
    * end-index is not included
    * both start-index and end-index can be ignored, like string acess. 
    * this slice does not change the original list, it creates a new list.
* elements of list can be changed.
    ```text
    list[0] = "newvalue"
    ```
    
### 2D list
```text
matrix = [
        [1, 2, 3], 
        [4, 5, 6], 
        [7, 8, 9]
    ]
    
-- 2D array elements can be accessed by index. The following gives you number 1
matrix[0][0]
```

### Methods
* append(element) - append an element to the end of a list.
* insert(index, element) - insert an element at position "index".
* remove(element) - remove an element (the first occurrence) from list.
* clear() - remove all elements from list.
* pop() - remove the last element from list.
* index(element) - return the index of the first occurrence of element. This throws exception if element does not exist.
    This method can be replaced by the "in" operator.
* count(element) - count the times of occurrence of an element in list.
* sort() - sort a list in ascending order. This method changes the original list.
* reverse() - this reverses a list in place.
* copy() - this gives a copy of a list.

### List Comprehensions
List comprehensions create a new list out of other collections. List comprehension expressions are 
enclosed by square backets, which is the representation of python list.

* [expr for val in collection]
* [expr for val in collection if <test>]
* [expr for val in collection if <test1> and <test2>]
* [expr for val in collection1 for val2 in collection2]

```text
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

-- square of all numbers
squares = [val**2 for val in nums]
print(squares)

-- square of all odd numbers
square_odds = [val**2 for val in nums if val%2]
print(square_odds)

-- square of all even numbers and the numbers are greater than 5
square_evens = [val**2 for val in nums if not val%2 and val > 5]
print(square_evens)

-- multiply of two lists
nums_1 = [3, 2, 1]
nums_2 = [4, 5, 6]
multiplied_nums = [val1*val2 for val1 in nums_1 for val2 in nums_2]
print(multiplied_nums)
```

## Tuple

Tuple is like list, but it is immutable.

* parenthesis are used to define tuple. 
* methods: 
    * count(element) - count the times of occurrence of an element in tuple.
    * index(element) - return the index of the first occurrence of element.
    
## unpacking 
```text
tuple = (1, 2, 3)

-- unpacking a tuple
-- the following is the same as x = typle[0], y = typle[1], z = tuple[2]
x, y, z = tuple
```
Unpacking can be applied to tuple and list.

## Set
### Basics
Set is builtin data type in python, which is an unsorted list of unique items. 
```text
-- create set by constructor
myset = set()
myset.add(10)
myset.add(False)
myset.add('word')
print(myset)

-- create set by pre-populated list
myset = set([10, False, 'word'])

-- create set by assigning values
myset = {10, False, 'word'}
print(myset)
```

### Methods
* add(element) - add an element to set.
* remove(element) - remove an element from set. An exception is thrown if element does not exist.
* discard(element) - remove an element from set. No exception is thrown if element does not exist.
* clean() - remove all elements from set.
* intersetion(setb) - common elements from both sets.
* union(setb) - all elements from both sets.
* 
## Dictionary
### Basics
Dictionary is key value pair. Dictionary is defined by curly braces. It is very much like JSON definition.
```text
people = {
    "name": "Zhiwei Liu",
    "age": 50, 
    "isOld": True
}
-- to reference a dictionary entry
people["name"]
-- python returns "None" if key does not exist
people["no-such-key"]
python
-- use the get() method of dictionary, you can specify the default value if key does not exit
people.get("no-such-key", "default-value")
-- use assignment operator to change value or put a new key-value pair into dictionary
people["new-key"] = "new-key's value"
```
