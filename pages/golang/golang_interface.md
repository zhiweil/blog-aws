---
title: Interface
tags: [golang]
keywords: Go
last_updated: June 29, 2019
summary: "Golang Interface"
sidebar: mydoc_sidebar
permalink: golang_interface.html
folder: golang
---
Interfaces don't describe data, interfaces describe behaviours. 

# Basics
```go
type Writer interface {
    Write(data []byte) (int, error)
}

type ConsoleWrite struct {}

func(cw ConsoleWrite)Write(data []byte) (int, error) {
    n, err := fmt.Println(string(data))
    return n, err
}
```

## Composing Interface
Instead of writing long, monolithic inteface, create small ones and compose more complicated one by these small ones. 
```go
type Writer interface {
    Write(data []byte) (int, error)
}

type Closer interface {
    Close() error
}

type WriterClose interface {
    Writer
    Closer
}
```

## Type conversion
```go
var wc := NewBufferedWriterCloser()
wcp := wc.(*BufferedWriterCloser)
```
* empty interfaces and type switches
```go
var i interface{} = 0
switch i.(type) {
    case int:
        .....
}
```

## Implement as values vs pointers
* method set of value is all methods with value receivers
* method set of pointer is all methods, regardless of receivers


## Best Practices
* Use many, small interfaces
    * single method interfaces are the most powerful and flexible
* Don't export interfaces for types that will be consumed
* Do export interfaces for types that will be used by package
* Design functions and methods to receive interfaces whenever possible

