---
title: Control Flow
tags: [golang]
keywords: Go
last_updated: June 27, 2019
summary: "Golang control flow"
sidebar: mydoc_sidebar
permalink: golang_control_flow.html
folder: golang
---
## If statement
* Initializer syntax
    ```go
    if i := 1; i > 0 {
        statements
    }
    ```
* Comparison operators (same as C)
* Logical Operators (same as C)
* Short circuiting (same as C)
* if-else statement (same as C)
* if-else-if statement (same as C)
* equality of floating numbers: should not compare directly, calculate difference instead)


## Switch statement
* switch on a tag (tag is a special variable)
* cases with multiple tests such as: 
    ```go
    switch num {
        case 1, 2, 3:
            fmt.Println("1, 2, 3")
        case 4, 5, 6:
            fmt.Println("4, 5, 6")
    }
    ```
* initializer syntax
    ```go
    switch num := 5; num {
        statements
    }
    ```
* switch with no tag (not just checking equality)
    ```go
    i := 5
    switch {
        case i < 0:
            statements
    }
    ```
* fallthrough is not implied
* break to quit a case
* type switch
    ```go
    var t interface{} = 1
    switch i.(type) {
        case int:
            statements
    }
    ```
    
## Defer
* Used to dealy execution of a statement until function exits.
* Useful to group "open" and "close" functions together
    * be careful in loops (better not to defer)
* Run in LIFO (last-in, first-out) order
* Arguments evaluated at the time fefer is executed, no at time of called function execution

## Panic
* Occur when program cannot continue at all
    * Don't use when file can't be opened, ,unless it is critial
    * Use for unrecoverable events - cannot obtain TCP port for web server
* Function will stop executing
    * Deferred functions will still fire
* If nothing handles panic, program will exit

## Recover
* Used to recover from panics
* Only useful in deferred functions
* Current function will not attemp to continue, but higher funcitons in call stack will

