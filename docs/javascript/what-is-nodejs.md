---
layout: default
title:  'What is node.js?'
parent: Javascript
nav_order: 1
description: "A breif explaination of node.js and its popularity."
---

## Asynchronous VS Synchronous

One of the terms that we have encountered with NodeJS is ```Asynchronous``` but what does it mean? Asynchronous execution means that the program doesn't run in the sequence it appears in your code editor from line 1 to line 100 chronologically. In NodeJS, async as its default, the program doesn't wait for the fist task to finish and execute the next line of code.

On the other hand, ```Synchronous``` execution generally refers to code that is transpiled or compiled and executed sequentially. In synchronous programming, the program is executed from line 1, then the next line until it reaches the last one at a time. The program waits for function to return something before continuing, everytime a function is called.

```javascript
// Asynchronous: 1,2,3,4,5
alert(1);
setTimeout(() => alert(2), 0);
alert(3);

// Synchronous: 1,3,2,4,5
alert(1);
alert(2);
alert(3);
```

## I/O(input/output)

It refers to program's interaction with the system disk and network.

## Blocking VS Non-blocking

Operations that block further execution until that operation finishes is referred to as blocking while non-blocking refers to code that does not block execution.

Blocking is when the execution of additional JavaScript in the Node.js process must wait until a non-JavaScript operation completes as in Node.js docs.

```javascript
// Blocking
function addNumbers(num1, num2) {
    console.log(num1 + num2);
}

// Blocking
const fs = require('fs');
const data = fs.readFileSync('./test-nodejs.md'); // blocks here until test-nodejs.md is read.
console.log(data);
addNumbers(5, 6); // will run after console.log

// Non-Blocking
const fs = require('fs');
fs.readFile('./test-nodejs.md', 'utf-8', (err, data) => {
    if (err) throw err; // does not block because readFile is asynchronous.
    console.log(data);
});
addNumbers(5, 6); // will run before console.log
```

Non-blocking operations allow a single process to serve multiple requests at the same time. However, Node.js is not suitable for CPU intensive operations which need a lot of calculations.

Most I/O methods in the Node.js standard library automatically serve async methods, which are non-blocking, and accept callback function as its argument. On the other hand, some methods come with ```Sync``` end which is a blocking operation.

[Nodejs](https://nodejs.dev/introduction-to-nodejs) website by Gatsby
