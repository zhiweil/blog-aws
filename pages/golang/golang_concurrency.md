---
title: Concurrency
tags: [golang]
keywords: Go
last_updated: June 29, 2019
summary: "Golang concurrency"
sidebar: mydoc_sidebar
permalink: golang_concurrency.html
folder: golang
---

## Creating goroutines
* use the "go" keyword in front of function call
* when using anonymous functions, pass data as local variables (should not rely on closures)

## Synchronization
* Use sync.WaitGroup to wait for groups of goroutines to complete
* Use sync.Mutex and sync.RWMutex to protect data access

## Parallelism
* by default, Go will use CPU threads eaual to available cores
* change with runtime.GOMAXPROCS(numProcs)
* more threads can increase performance, but too many can slow it down

## Best practices
* Don't create goroutines in libraries
    * Let cunsumer control concurrency
* when creating a gorountine, know how it will end
    * avoid subtle memory leaks
* check for race conditions at compile time
    ```go
    go run --race path/to/main/function.go
    ```

