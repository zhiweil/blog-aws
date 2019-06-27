---
title: Map and Struct
tags: [golang]
keywords: Go
last_updated: June 26, 2019
summary: "Golang map and struct"
sidebar: mydoc_sidebar
permalink: golang_map_and_struct.html
folder: golang
---

## Map
* Collection of key values that are accessed via keys
* created via literals or by make() function: map[string]int{}
* Members area accessed via ["key] syntax
* check presence via "value, ok" syntax
* pass by reference (like slice) - multiple assignments refer to the same underlying data

## Struct
* collection of disparate dafa types that describe a single concept
* keyed by named field
* normally created as types, but anonymous structs are allowed usually for short-lived situations
* structs are value types (passed by value not reference like array)
* no inheritance, by can use composite via embedding
* tags can be added to struct fields to describe fields (reflect package is used to access tags)

