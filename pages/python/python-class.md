---
title: Class
tags: [python]
keywords: python
last_updated: March 23, 2019
summary: "Python Class"
sidebar: mydoc_sidebar
permalink: python-class.html
folder: python
---

## Basics
* keyword "class" is used to define a class in python.
* python class naming follows Pascal naming convention, all words in a class name should be capitalized.
* it is recommended to put two lines after a class definition
* unlike the other languages, you don't need the "new" keyword to create an object.

```python
class SomeClass:
    def func1(self):
        .....
        
    def func2(self):
        .....
        
        
# to create objects of SomeClass 
some_class1 = SomeClass()
some_class2 = SomeClass()

# this will add a new attribute to class
some_class1.x = 1
```

## Constructor
Constructor is a function which gets called when creating a new object.
```python
class SomeClass:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        
        
some_class1 = SomeClass(1, 2)
print(some_class1.x)
```

## Inheritance
```python
class Parent:
    def say(self):
        print("I'm in parent class")
        
class Child(Parent):
    # python does not like empty class, put pass here to make python happy
    pass
    
child = Child()
child.say()
```