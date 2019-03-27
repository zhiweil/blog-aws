---
title: Function
tags: [python]
keywords: python
last_updated: March 23, 2019
summary: "Python function"
sidebar: mydoc_sidebar
permalink: python-function.html
folder: python
---
## Basics
* use the "def" keyword to define a function
* a best practice is to put two blank lines after function definition.

```python
# funciton body is identified by indentation
def function_name():
    .....
    
# To call a function
function_name()
```

## Parameter & argument
Parameters are the variable names of a function definition; arguments are the variables passed to call a function.
 
### Positional argument
```python
def function_name(param1, param2):
    .....
    
# to call the function above, you'll just need to pass in all the arguments
function_name(arg1, arg2)
```

### Keyword argument
```python
def function_name(param1, param2):
    .....
    
# to call the function above, you'll just need to pass in all the arguments
function_name(param1 = arg1, param2 = arg2)
```

**Keyword arguments should always come after positional arguments.**

## return 
* The keyword "return" is used to return value from function.
* If a function does not return anything, you'll get "None" as return value.
