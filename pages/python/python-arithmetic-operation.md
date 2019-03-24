---
title: Arithmetic Operations
tags: [python]
keywords: python
last_updated: March 23, 2019
summary: "Python arithmetic operations"
sidebar: mydoc_sidebar
permalink: python-arithmetic.html
folder: python
---

There are two arithmetic types in python: integer (those numbers without decimal point) and float (those numbers with decimal point). 

## Operators
* +, - * - these operators are nothing different from the other programming languages. 
* / (unlike most of the other languages, this returns float number), // (this returns integer), % (modulus)
* "**" - power operator like "10 ** 2" 
* +=, -=, *=, /= - these are assignment operators.

## Precedence
1. exponential
2. multiplication and division
3. addition and subtraction
4. left-to-right for operations at the same level
5. brackets can change the order of operations.

## Math functions
The following are built-in functions
* round() - round a float number to integer.
* abs() - absolute value.

The following are in the ["math" module](https://docs.python.org/3/library/math.html), so you'll need to "import math".
* ceil() - convert float to integer, the result is always greater than or equal to the original float.
* floor() - convert float to integer, the result is always smaller than or equal to the original float.
