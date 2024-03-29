---
layout: default
title:  Functional Programming
parent: Javascript
nav_order: 2
description: "Functional Programming"
---

## Learn About Functional Programming

1 Isolated functions - there is no dependence on the state of the program, which includes global variables that are subject to change.

2 Pure functions - the same input always gives the same output.

3 Functions with limited side effects - any changes, or mutations, to the state of the program outsite the function are carefully controlled.

```js
/**
 * A long process to prepare tea.
 * @return {string} A cup of tea.
 **/
const prepareTea = () => 'greenTea';

/**
 * Get given number of cups of tea.
 * @param {number} numOfCups Number of required cups of tea.
 * @return {Array<string>} Given amount of tea cups.
 **/
const getTea = (numOfCups) => {
  const teaCups = [];

  for(let cups = 1; cups <= numOfCups; cups += 1) {
    const teaCup = prepareTea();
    teaCups.push(teaCup);
  }

  return teaCups;
};

// Add your code below this line

const tea4TeamFCC = getTea(40); // :(

// Add your code above this line

console.log(tea4TeamFCC);
```
