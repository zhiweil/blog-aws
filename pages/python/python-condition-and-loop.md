---
title: Condition and Loop
tags: [python]
keywords: python
last_updated: March 23, 2019
summary: "Python conditional and loop statements"
sidebar: mydoc_sidebar
permalink: python-conditional-and-loop.html
folder: python
---

## Conditional statement
```python
if CONDITION1:
    print("CONDISTION 1 is True")
elif CONDITION2:
    print("CONDITION 2 is True")
else:
    print("None of CONDITION 1 or CONDITION 2 is True")
```
Several key points:
* ":" is used to end conditions.
* parenthesis is no needed in python.
* curly brackets are not needed, python uses indentations instead.

CONDITION is constructed by logical operators and comparison operators.

## Logical Operators
* and, or, not 
* parenthesis can change precedence. Otherwise, not -> and -> or

## Comparison Operator
* >, >=, <, <=, ==, !=

## While loop
**The "while" loop supports "else" statement. the "else" statement will be executed if loop is not
ended by "break".** 
```python
while CONDITION:
    ...
else 
    ....    
```

## For loop
For loop uses a loop variable to control the loop flow. 

The built-in function range(lower-bound, upper-bound, step) gives you a range. The lower bound parameter 
can be ignored and it is 0 by default. The step parameter can be ignored, it is 1 by default.

**The "for" loop supports "else" statement. the "else" statement will be executed if loop is not
ended by "break".** 

```python
for item in "a string, a collection such as list, set or a range":
    ...
else:
    ...
```

## Change loop flow
* break - quite a loop
* continue - skip the rest of a loop flow and do the next iteration. 
* pass - do nothing