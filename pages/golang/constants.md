---
title: Constants
tags: [golang]
keywords: Go
last_updated: June 25, 2019
summary: "Golang Constants"
sidebar: mydoc_sidebar
permalink: golang_constants.html
folder: golang
---

## Basics
* Immutable, but can be shadowed (both value and type)
* Replaced by the compiler at compile time
    * value must be calculable at compile time
* Named like variables
    * PascalCase for exported constants
    * camelCase for internal constants

## Typed vs Untyped
* Typed constants work like immutable variables
    * can interoperate only with same type
* Untyped constants work like literals
    * can interoperate with similar types

## Enumerate
* Enumerated constants
    * special symbol iota allows related constants to be created easily
    * iota starts at 0 in each const block and increments by 1.
    * watch out of constant values that match zero values for variables. 
        ```go
        const (
            // ignore 0 for this enumeration
            _ = iota
            firstEnum
            secondEnum
        )
        ``` 
        
* Enumerated expressions
    * Operations that can be determined at compile time are allowed:
        * Arithmetic
        * Bitwise
        * Bitshifting
        
