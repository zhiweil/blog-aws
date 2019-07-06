---
title: Channle
tags: [golang]
keywords: Go
last_updated: June 29, 2019
summary: "Golang channel"
sidebar: mydoc_sidebar
permalink: golang_channel.html
folder: golang
---
##  Basics
* create a channel with make command (the second parameter is the buffer size of channle)
    * make(chan int)
    * make(chan int, size int)
* send message into channel
    * ch <- val
* receive value from channel
    * val := <- ch
* can have multiple senders and receivers

## Restricting data flow
* channel can be cast into send-only or receive-only versions by the "chan" keyword
    * send-only: chan <- int
    * receive-only: <-chan int
    
## Buffered channel
* channels block sends side till receiver is available
* channels block receiver side till message is available
* can decouple sender and receiver with buffered channels
    * make(chan int, 50)
* user buffered channels when sender and receiver have assymmetric loading

## Loop with channle
* for ... range loos with channels (no index returns from channel)
    ```go
    for v := range ch {
    
    }
    ```
    * used to monitor channel and process messages as they arrive, for loop blocks for another message.
    * loop exit when channel is closed by close(ch).
    
## select statement
* allows goroutine to monitor several channels at once
    * blocks if all channels block
    * if multiple channel receive value simultaneously, behaviour is undefined
    * exit select by the break statement
