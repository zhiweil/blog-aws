---
title: Readline
tags: [NodeJS]
keywords: NodeJS
last_updated: July 16, 2019
summary: "NodeJS Readline module"
sidebar: mydoc_sidebar
permalink: nodejs_readline.html
folder: nodejs
---

Readline module is for user interaction via a command line interface. 
```js
const readline = require("readline");
let r = readline.createInterface({ 
    input: process.stdin, 
    output: process.stdout 
});

let num1 = Math.floor(Math.random() * 10 + 1);
let num2 = Math.floor(Math.random() * 10 + 1);
let answer = num1 + num2;

r.question(`What is ${ num1 } + ${ num2 }?\n`, (userInput) => {
    if (userInput.trim() == answer) {
        r.close();
    } else {
        r.setPrompt(`Your answer ${ userInput.trim() } is incorrect, try again.\n`);
        r.prompt();
        r.on("line", (userInput1) => {
            if (userInput1.trim() == answer) {
                r.close();
            } else {
                r.setPrompt(`Your answer ${ userInput1.trim() } is incorrect, try again.\n`);
                r.prompt();
            }
        });
    }
});

r.on("close", () => {
    console.log("Correct!");
});
```