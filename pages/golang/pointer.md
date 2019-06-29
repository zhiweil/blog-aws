---
title: Pointer
tags: [golang]
keywords: Go
last_updated: June 29, 2019
summary: "Golang Pointer"
sidebar: mydoc_sidebar
permalink: golang_pointer.html
folder: golang
---

## Creating pointers
* Pointer types use an asterisk(*) as a prefix to type pointed to
    * *int - a pointer to integer
* Use the addressof operator (&) to get address of variable
* Dereferencing pointers
    * Dereference a pointer by preceding with an asterisk(*)
    * Complex types (such as slice, struct) are automatically dereferenced 
    
* Create pointers to objects
    * Can use the addressof operator (&) if value type already exists
        ```go
        ms := myStruct{foo: 42}
        p := &ms
        ```
    * Use address of operator before intializer
        ```go
        &myStruct{foo: 42}
        ```
    * Use the new keyword
        * Can't initialize fields at the same time
* Types with internal pointers
    * All assignment operations in Go are copy operations
    * Slices and maps contain interal pointers, so copies point to the same inderlying data
    