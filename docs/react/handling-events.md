---
layout: default
title:  'Handling Events'
parent: React
nav_order: 6
description: "Handling Events"
---

## [Handling Events](https://reactjs.org/docs/handling-events.html)

### Handling events with React is similar to handling events on DOM elements

```js
// HTML using lowercase
<button onclick="activateLasers()">
    Activate Lasers
</button>

// React using camelCase
<button onClick={activateLasers}>
    Activate Lasers
</button>
```

In React, the false value cannot be returned to prevent default behavior. Explicitly use prevenDefault. For example:

```js
// HTML
<a href="#" onclick="console.log('The link was clicked.'); return false">
    Click me
</a>

// React
function ActionLink() {
    function handleClick(e) {
        // "e" is a synthetic event. No need to worry about cross-browser compatibility.
        e.preventDefault();
        console.log('The link was clicked.');
    }

    return (
        <a href="#" onClick={handleClick}>
            Click me
        </a>
    );
}
```

No need to call addEventListener to add listeners to DOM element after it is create.
Just provide a listener when the element is initially rendered.

When using ES6 class, a common pattern is for an event handler to be a method on the class.
For example, this `Toggle` component renders a button that lets the user toggle between ON and OFF states:

```js
class Toggle extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isToggleOn: true
        };

        // Using this in JSX callbacks is kind of tricky.
        // If this.handleClick is not bound and pass it to onClick, it will be undefined.
        // This binding is necessary to make `this` work in the callback.
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        this.setState(prevstate => ({
            isToggleOn: !prevstate.isToggleOn
        }));
    }

    render() {
        return (
            <button onClick={this.handleClick}>
                {this.state.isToggleOn ? 'On' : 'OFF'}
            </button>
        );
    }
}

ReactDOM.render(
    <Toggle />,
    document.getElementById('root')
);
```

There are 2 ways to get around calling `bind`. If you are using the experimental public class field syntax, you can use class fields to correctly bind callbacks:

```js
class LoggingButton extends React.Component {
    // This syntax ensures `this` is bound withing handleClick.
    // Warning: this is experimental syntax.
    handleClick = () => {
        console.log('this is: ', this);
    }

    render() {
        return (
            <button onClick={this.handleClick}>
                Click me
            <button>
        );
    }
}
```

If you aren’t using class fields syntax, you can use an arrow function in the callback:

```js
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}> {/* Arrow function is used */}
        Click me
      </button>
    );
  }
}
```

A different callback is created each time the LoggingButton component renders. If this callback is passed as a prop to lower components, those components might do an extra re-rendering. To avoid this performance problem, consider using the class fields syntax.

### Passing arguments to event handlers

Inside a loop it is common to want to pass an extra parameter to an event handler. For example, if id is the row ID, either of the following would work:

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

The above two lines are equivalent, and use arrow functions and Function.prototype.bind respectively.

In both cases, the e argument representing the React event will be passed as a second argument after the ID. With an arrow function, we have to pass it explicitly, but with bind any further arguments are automatically forwarded.
