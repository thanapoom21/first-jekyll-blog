---
layout: default
title:  'React - Lists and Keys'
parent: React
nav_order: 3
description: "Why Are Lists And Keys Important in React?"
---

## [Lists and Keys](https://reactjs.org/docs/lists-and-keys.html)

Function called `map()` is used here to take an array of numbers and double their values. We assign the new array returned by `map()` to the variable `doubled` and log it to the console.

```js
const numbers = [100, 2019, 1989, 93000000, 555];
const double = numbers.map((number) => number * 2);
console.log(doubled); // result [200, 4038, 3978, 186000000, 1110]
```

### Rendering Multiple Components

Collections of elements can be built and included in JSX using curly braces {}.

```js
const numbers = [1, 2, 3, 4, 5];

// Looping through the `numbers` array using the JavaScript `map()` function
const listItems = numbers.map((number) => <li>{number}</li>);
// Returning a `<li>` element for each item.

ReactDOM.render(
    // Assigning the resulting array of elements to `listItems`.
    <ul>{listItems}</ul>,
    document.getElementById('root')
);
```

This code will render an unordered list with numbers between 1 to 5.

### Basic List Component

```js
function NumberList(props) {
    const numbers = props.numbers; // Save the props in the variable called numbers.
    const listItems = numbers.map((number) => // Loop throught numbers using map().
        // Key attribute should always be provided for list items in React.
        <li key={number.toString()}>{number}</li> // Return 5 list items.
    );
    return (
        <ul>{listItems}</ul> // Return 5 list items in an unordered list.
    );
}

const numbers = [1, 2, 3, 4, 5]; // Create an array with 5 numbers.
ReactDOM.render(
    <NumberList numbers={numbers} />, // Send numbers' array to NumberList function props.
    document.getElementById('root') // Grab an ID and render the specified element.
);
```

### Keys

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity.

```js
const items = ["iPhones", "Car Keys", "Books", "Coins", "Pecans"];
const loopedItems = items.map((item) => <li key={item.id}> {item.text} </li>);
```

IDs from the data are most often used. However, index can also be used as a key but as a last resort.

```js
// Only use index if  items have to stable IDs.
const loopedItems = items.map((item, index) => <li key={item.index}> {item.text} </li>);
```

### Extracting Components with Keys

Keys only make sense in the context of the surrounding array.

For example, if you extract a ListItem component, you should keep the key on the `<ListItem />` elements in the array rather than on the `<li>` element in the `ListItem` itself.

```js
function ListItem(props) {
    // No need to use key here.
    return <li>{props.value}</li>;
}

function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map((number) => <ListItem key={number.toString()} value={number});

    return (
        <ul>
            {listItems}
        </ul>
    );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
);
```
