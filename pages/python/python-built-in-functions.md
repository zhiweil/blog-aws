---
title: Python 3 Built-in functions
tags: [python]
keywords: python Built-in function
last_updated: March 9, 2019
summary: "Built-in functions are always available in Python."
sidebar: mydoc_sidebar
permalink: python_built-in-functions.html
folder: python
---
## Arithmetic
* abs() - get absolute value of a number
* divmod(a, b) - returns quotient and remainder.

## Containers
* dict() - create a dictionary
* list() - create a list
* set() - create a set
* tuple() - create a tuple

## Iterable
* all(iterables) - true if all iterables are true or iterables are empty
* any(iterables) - true if any iterable is true, return false if iterables are empty.
* enumerate(iterable, start = 0) - returns an enumerate object.

## Type & Conversion
* ascii(chars) - returns a printable string representation of "chars" with all special characters escaped.
    The returned string is enclosed by single quotes.
* repr(chars) - similar to ascii(). You can think of this as toString() and can be overriden by defining a method "__repr__()".
* bin(number) - covert a number to binary string enclosed by single quotes and prefixe by "0b".
* bool(expression) - return boolean constants True or False depending on the evaluaion of "expression".
* bytearray([source [,encoding [,errors]]]) - returns a byte array object.
* byte([source [,encoding [,errors]]]) - turns a byte object.
* chr(unicode) - return string representation of a unicode number.
* compile(source, file, mode) - convert python code to AST object (Abstract Syntax Trees).
* complext(real, image) - return a complex number.

## Metadata
* callable(object) - return True if an object is callable, which means that the object has a "__call__()" method.
* classmethod() - used as annotation like @classmethod, which converts a method to be a class method.
* setattr(object, name) - set attribute to object.
* delattr(object, name) - remove attribute from object.
* dir(object) - returns a list of names of the current local scope without argument; or a list of names of the specified object.


## Resources
* [Python built-in functions](https://docs.python.org/3.6/library/functions.html)