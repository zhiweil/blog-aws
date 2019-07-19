---
title: JS Basics
tags: [NodeJS]
keywords: JS Javascript
last_updated: July 17, 2019
summary: "Javascript basics"
sidebar: mydoc_sidebar
permalink: javascript_basics.html
folder: javascript
---
## Data type
* undefined
* null
* boolean
* string
* symbol
* number
* object

## Variable
* var keyword - used throughout project or function scope.
* let keyword (since ES6) - used within a block scope. This does not allow to define a variable twice.
* const keyword (since ES6) - same as let by readonly. 
    * a const variable cannot be reassigned but can be mutated (add/change properties).
    * an object is not allowed to mutate by: Object.freeze(obj);
* name is case sensitive

## String literal
* double quotes
* single quotes
* backticks
* special characters must be escaped (\) to be put into a string literal.
* strings can be concatenated by + or += operators
* find the lenght of string by the "length" property
* find the letter of string by [] operator
* use backticks to create string template which:
    * create multi-line string
    * embed variables into template
    * embed single or double quotes
    ```js
    let n = "Zhiwei";
    let s = `Hi ${n}
    How are you.
    Cheers
    Somebody`;
    console.info(s);
    ```

## Array
```js
var a = [1, 2, "word"];
var b = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

### Methods
The following methods can be used to manipulate arrays:
* push(ele) - if ele is another array, it is treated as an element of the target array (use spread operator ... to insert individual element).
* pop() - remove and return the last element of array
* shift() - remove and return the first element of array
* unshift() - add an element to the start of an array

### Spread operator ...
This is exactly the same as function's rest operator.
```js
// copy of array
a2 = [...a1];
// insert each element in a1 as an element in a2, this won't insert a1 as a single element in a2
a2.push(...a1);
```

## Function
* use keyword function
* if a function does not return anything explicitely, it returns "undefined". 
* if a variable inside function is defined without any keyword (var, let, const etc), it has global scope!!!
* JS allows default values for function arguments such as function(arg1, arg2=0){}

### Anonymous function 
```js
let f = function() {
    // function body goes here
};
```

### Lambda function
```js
let f = (arg1, arg2) => {
  // function body goes here
};
```

### Rest operator ...
```js
let f = function(a, b, ...args) {
  // args is an array
}
```
**The args must be the last argument of the function**

## Conditional statements
* strict equals "===" compares without type conversion. 
* strict unequals "!==" compares without type conversion.

## Object

### Definition
An object literal can be defined like:
```js
var myObj = {
    "name": "Zhiwei",
    "age": 29,
    "say": function() {}
};
```

Function definition can be simplified by using symbols and special grammar for methods.
```js
var myObj = {
  name: "Zhiwei",
  age: 29,
  say() {}
};
```

The new class keyword and constructor can be used to further simplify the object definition.
```js
class MyClass {
  constructor(arg1, arg2) {
    
  }
};

let myObj = new MyClass(1, 3);
```

### Reference
The properties can be accessed by "." notation or square bracket notation or by a variable.

```js
console.info(myObj.name);
console.info(myObj["age"]);
var n = "age";
console.info(myObj[n]);
```

Object properties can be deleted by the delete keyword.
```js
delete myObj.age;
```

### Getter and setter
```js
class MyClass {
    constructor(name) {
      this._name = name;
    }
    
    set name(name) {
      this._name = name;
    }
    
    get name() {
      return this._name;
    }
}

let c = new MyClass("Zhiwei");
console.info(c.name);
c.name = "Liu";
console.info(c.name);
```

### Useful functions
The functions are available for any object:
* hasOwnProperty(propName) - check if a property exists.

### Destructuring assignment
```js
// for object
let obj = {x: 1, y: 2, z: 3};
const {x: a, y: b, z: c} = obj;
console.info(a, b, c);
// a=1, b=2, c=3

// for array
const [a, b,,,c] = [1, 2, 3, 4, 5, 6];
// a=1 b=3 c=5

// swap elements of arrays
const [a,b] = [b,a];

// use rest operator to remove the first two elements
const [,,...eles] = a;

// for function calls - you don't need to put a whole object in function definition
let obj = {
    attr1: 1,
    attr2: 2,
    attr3: 3
};
let f = function({attr1, attr2}) {
    console.info(attr1, attr2);
};
f(obj);
```

## Loop
* for loop
* while loop
* do - while loop
* for (let ele of collection) { ... }

## Collection Manipulation
* filter
    ```js
    a = [1,2,3, -4, "b"];
    let b = a.filter(ele => Number.isInteger(ele) && ele > 0);
    b = [1,2,3]
    ```
* map
    ```js
    const a = [1, 2, 3, -4];
    let b = a.filter(ele => Number.isInteger(ele)).map(ele => ele * ele);
    console.info(b);
    // b = [1, 4, 9, 16]
    ```
* reduce - convert collection to a single value by a function
    ```js
    const a = [1, 2, 3, -4];
    let b = a.reduce((total, num) => {
        return total + num;
    });
    console.info(b);
    // b = 2
    ```
    
## Export and import

```js
import {func1} from "./func1";
import * as m from "./mymodule";
```

```js
const func1 = (a, b) => {
  console.info(a, b);
}
export {func1}
export const CONSTANT1 = 1;
```

### Export default
Default export is used to export a single thing from a module.

```js
export default const func1 = function(a, b) {
  // function body goes here
}
```

At import, the {} are not needed for default export.
 
```js
import func1 from "./func1";
```