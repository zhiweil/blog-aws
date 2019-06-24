---
title: Primitives
tags: [golang]
keywords: Go
last_updated: June 24, 2019
summary: "Golang Primitives"
sidebar: mydoc_sidebar
permalink: golang_primitives.html
folder: golang
---
## Boolean types
* Values are true or false.
* Not an alias for other types (e.g. int)
* Zero value is false

## Numeric types
* integers 
    * Signed integers
        * int type has varying size, but min 32 bits
        * 8 bit (int8) through 64 bit (int64).
    * Unsigned integers
        * 8 bit (byte and uint8) through 32 bit (uint32).
    * Arithmetic operations
        * addition, subtraction, multiplication, division and remainder
    * Bitwise operations
        * and (&), or (|), xor (^), not (^) and not (&^).
    * Zero value is 0.
    * Cann't mix types in the same family! (uint16 + uint32 = error).
* Floating point numbers
    * Follow IEEE-754 standard
    * Zero value is 0.
    * 32 and 64 bit versions
    * Literal styles:
        * Decimal (3.24)
        * exponential (13e8, 13E8)
        * mixed (3.14e8)
    * Arithmetic operations
        * addition, subtraction, multiplication, division
* Complex numbers
    * Zero value is (0+0i)
    * 64 and 128 bit versions
    * built-in functions
        * complex - make complex number from two floats
        * real - get real part as float
        * imag - get imaginary part as float
    * Arithmetic operaitons
        * addition, subtraction, multiplication, division
## Text types
    * Strings
        * UTF-8
        * Immutable
        * Can be concatenated with plus (+) operator
        * Can be converted to []byte
    * Rune
        * UTF-32
        * Alias for int32
        * Special methods normally required to process
            * e.g. strings.Reader#ReadRune