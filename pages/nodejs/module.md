---
title: Module
tags: [NodeJS]
keywords: NodeJS
last_updated: July 17, 2019
summary: "NodeJS Module definition and Reference"
sidebar: mydoc_sidebar
permalink: nodejs_module.html
folder: nodejs
---
## Definition
```js
const sum = (x, y) => {
    return x + y
}

const MyConst = 0630

class MyObject {
    constructor(name) {
        this.name = name;
    }
}

module.exports = {
    sum: sum, 
    MyConst: MyConst, 
    MyObject: MyObject
}
```

## Reference
```js
mymodule = require('./mymodule');
console.info(mymodule.sum(1,2));
```