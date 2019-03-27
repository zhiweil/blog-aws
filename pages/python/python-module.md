---
title: Python module
tags: [python]
keywords: python
last_updated: March 27, 2019
summary: "How to define and import python modules"
sidebar: mydoc_sidebar
permalink: python-module.html
folder: python
---
Python modules are python source code files.

## Define module
```python
# put the following code into a file called mymodule.py
def func1():
    print("from mymodule->func1()
```

## Import module
A whole module can be imported by:
```python
# you don't need the .py extension when importing a module
import mymodule

module.func1()
```

Or you can import a specific function of a module.
```python
from mymodule import func1

func1()
```