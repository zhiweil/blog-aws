---
title: Go
tags: [golang]
keywords: Go
last_updated: June 23, 2019
summary: "Golang variables"
sidebar: mydoc_sidebar
permalink: golang_variables.html
folder: golang
---

## Declare variable
Three ways to declare a variable.
```go
var i int
var i int = 100
i := 100
```

## CANNOT redeclare but CAN shadow
Variables can be declared at different levels such as package and function levels. You cannot redeclare 
variables at the same level. Variables at function level can shadow the variables at the package level.

## All variables MUST be used
Unused variables cause compile time errors. 

## Visibility
* Lower case first letter has package level visibility.
* Upper case first letter exports.
* No private scope.

## Naming Conventions
* Pascal or camelCase
    * Capitalize acronyms (HTTP, URL etc)
* As short as reasonable
    * longer name for longer lives.

## Type Conversions
* destinationType(variable) - golang has no automatic type conversions.
* Use strconv package for strings. 