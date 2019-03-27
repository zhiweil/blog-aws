---
title: Exception
tags: [python]
keywords: python
last_updated: March 27, 2019
summary: "Python error handling"
sidebar: mydoc_sidebar
permalink: python-exception.html
folder: python
---

Excepions in python can be handled nicely by try-except statement.
```python
try:
    num = int(input("Number> "))
    print(num)
except ValueError:
    print("invalid number")
```

**There can be more than one except so that we can handle different kinds of errors.**