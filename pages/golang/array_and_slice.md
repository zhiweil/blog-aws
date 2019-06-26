---
title: Array and Slice
tags: [golang]
keywords: Go
last_updated: June 26, 2019
summary: "Golang array and slice"
sidebar: mydoc_sidebar
permalink: golang_array_and_slice.html
folder: golang
---

## Array
* Collection of items of the same type
* fixed size
* Declaration stypes
    ```go
    a := [3]int{1, 2, 3}
    a := [...]int{1, 2, 3}
    var a [3]int
    ```
* Access by zero-based syntax
* len() function returns the size of array
* copies refer to different underlying set of data
    ```go
    array1 = array2 // array2 holds different set of data than array1
    ```
    
## Slice
* Backed by array
* Creation styles:
    * slice existing array or slice
    * literal style: []int{1, 2, 3}
    * via make function
        ```go
        make([]int, 8) // create slice of length and capacity of 8
        make([]int, 8, 100) // create slice of length 8 and capacity 100
        ```
    * len() function returns length of slice
    * cap() function returns the length of underlying array
    * append() function adds elements to slice
        * may cause expensive operation if the underlying array is too small
    * copies of slices refer to the same underlying array
    
