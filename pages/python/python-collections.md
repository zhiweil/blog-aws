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
    ```python
    [element1, element2, ...]
    ```
* use array style to access each element such as list[index]. the "index" value can be negative.
* A slice of list by:
    ```python
    list[start-index:end-index]
    ``` 
    * end-index is not included
    * both start-index and end-index can be ignored, like string acess. 
    * this slice does not change the original list, it creates a new list.
* elements of list can be changed.
    ```python
    list[0] = "newvalue"
    ```
    
### 2D list
```python
matrix = [
        [1, 2, 3], 
        [4, 5, 6], 
        [7, 8, 9]
    ]
    
# 2D array elements can be accessed by index. The following gives you number 1
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