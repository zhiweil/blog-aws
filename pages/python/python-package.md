---
title: Python package
tags: [python]
keywords: python
last_updated: March 27, 2019
summary: "How to create and reference python packages"
sidebar: mydoc_sidebar
permalink: python-package.html
folder: python
---
## Create Package
1. Related modules can be organized into a package. A package is simple a folder or directory.
2. Inside package root directory, create a special file "__init__.py"
3. Add modules as needed

## Reference Package
* you can import a module of a package by
    ```python
    import mypackage.mymodule
    
    # to call a function, you'll need to use FQDN like
    mypackage.mymodule.myfunc()
    ```
* or import a function of a module inside a package
    ```python
    from mypackage.mymodule import myfunc, myotherfunc
    # or from mypackage import mymodule
    
    # then you just call the function directly
    myfunc()
    myotherfunc()
    ```