---
layout: default
title: Looping Through Arrays
parent: Javascript
nav_order: 4
description: "Looping Through Arrays in JavaScript"
---

## Looping Through Arrays

### Simplify loops with ```.map()```, ```.filter()```, ```.reduce()```

Let's start with ```.map()```

```js
let arr = [1, 2, 3, 4, 5];

let doubleArr = arr.map(item => {
  return item * 2;
});

console.log(doubleArr);
// expected output arr = [2, 4, 6, 8, 10]
```

Then ```.reduce()```

```js
let arr = [1, 2, 3, 4, 5];
let reducer = (accumulator, currentValue) => accumulator + currentValue;

console.log(arr.reduce(reducer));
// expected output: 15

console.log(arr.reduce(reducer, 5));
// expected output: 20
```

Finally ```.filter()```

```js
let words = ['map', 'reduce', 'filter', 'forEach', 'fill', 'concat'];

let result = words.filter(word => word.length > 4);

console.log(result);
// expected output: Array ["reduce", "filter", "forEach", "concat"]
```
