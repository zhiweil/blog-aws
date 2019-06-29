---
title: Function
tags: [golang]
keywords: Go
last_updated: June 29, 2019
summary: "Golang Function"
sidebar: mydoc_sidebar
permalink: golang_function.html
folder: golang
---

## Basic Syntax
    ```go
    func foo() {
        ...
    }
    ```

## Parameters
* comma delimited list variables and types
    * func foo(a int, b string)
* parameters of the same type are listed type once: func foo(a, b int)
* When pointers are passed in, function can change the value of caller
    * this is always true for slices and maps
* Use variadic parameters to send list of same types:
    * must be the last parameter
    * receive as slice
    * func foo(a int, ...b string)

## Return values
* Single return value just list type
    * func foo() int
* Multiple return values lists types with parenthesis
    * func foo() (int, string)
    * the (result type, error) is very common idiom
* Can use named return values
    * initialize return variables
    * return use keyword return on its own
    * name return values readibility is generally bad especially for long functions
* Can return address of local variables in functions
    * automatically promoted from local memory (stack) to shared memory (heap)
    
## Anonymous function
* invoked immediately
    ```go
    func foo() {
        ...
     }()
    ```
* assigned to a variable and then invoked when needed
    ```go
    a := func() {
        ...
     }
     ...
     
     a()
    ```
    
## Functions as types
* Functions can be assigned to variables or used as values and return values from other functions
* Type signature is just like function signatures without parameter names
    * var f func(int, string, string) (string, error)
    
## Methods
* Functions that are executed in context of a type
* Format
    * func (t SomeType) myfunc(a int, b string) (int, error) {}
* Receivers can be values or pointers
    * Value receivers get copy of data
    * Pointer receivers get address of data
     