---
title: Event & EventEmitter
tags: [NodeJS]
keywords: NodeJS
last_updated: July 16, 2019
summary: "NodeJS event driven programming"
sidebar: mydoc_sidebar
permalink: nodejs_event.html
folder: nodejs
---
## Basics
```js
const EventEmitter = require('events');
var eventEmitter = new EventEmitter();

eventEmitter.on("tutorial", () => {
    console.log("tutorial event triggered.");
});

eventEmitter.emit("tutorial");
```

## Pass Parameters
```js
var eventEmitter = new EventEmitter();

eventEmitter.on("tutorial", (num1, num2) => {
    console.log("tutorial event triggered: " + (num1 + num2))
});

eventEmitter.emit("tutorial", 1, 2);
```

## Object with event support
```js
const EventEmitter = require('events');
var eventEmitter = new EventEmitter();

eventEmitter.on("tutorial", (num1, num2) => {
    console.log("tutorial event triggered: " + (num1 + num2))
});

eventEmitter.emit("tutorial", 1, 2);

class People extends EventEmitter {
    constructor(name) {
        super();
        this._name = name;
    }

    getName() {
        return this._name;
    }
}

var people = new People("Zhiwei");
people.on("name", () => {
    console.log("Nam changed event triggered for " + people.getName());
});

people.emit("name");
```