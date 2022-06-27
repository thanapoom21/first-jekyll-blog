---
layout: default
title:  'React - State and Lifecycle'
parent: React
nav_order: 2
description: "Introducing the concept of state and lifecycle in a React component"
---

## [State and Lifecycle](https://reactjs.org/docs/state-and-lifecycle.html)

Calling `ReactDOM.render()` to change the rendered output:

```js
function tick() { // a functional component
    const element = (
        <div>
            <h1>Hello, world!</h1> {/* A JSX element */}
            <h2>It is {new Date().toLocaleTimeString()}.</h2> {/* create and use Date object and convert it to locale time. */}
        </div>
    );
    ReactDOM.render(
        element, // element constant to be rendered.
        document,getElementById('root') // to where the selected element should be rendered.
    );
}

setInterval(tick, 1000); // wait 1 second before executing tick function component.
```

Making the `Clock` component reuseable and encapsulated. It will set up its own timer and update itself every second.

We can start by encapsulating how the clock looks:

```js
function Clock(props) {
    return (
        <div>
            <h1>Hello, world!</h1>
            <h2>It is {props.date.toLocaleTimeString()}.</h2>
        </div>
    );
}

function tick() {
    ReactDOM.render(
        <Click date={new Date()} />,
        document.getElementById('root')
    );
}

setInterval(tick, 1000);
```

However, it misses a crucial requirement: the fact that the Clock sets up a timer and updates the UI every second should be an implementation detail of the Clock.

Ideally we want to write this once and have the Clock update itself:

```js
ReactDOM.render(
    <Clock />,
    document.getElementById('root')
);
```

To implement this, we need to add `state` to the `Clock` compoment.

State is similar to props, but it is private and fully controlled by the component.

### We Can Convert a Function to a Class

* Create an ES6 class that extends `React.Component`.
* Add a single empty method to it called `render()`.
* Move the body of the function into the `render()` method.
* Replace `props` with `this.props` in the `render()` body.
* Delete the remaining empty function declaration.

```js
class Clock extends React.Component {
    render() {
        return (
            <div>
                <h1>Hello, world!</h1>
                <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
            </div>
        );
    }
}
```

`Clock` is now defined as a class rather than a function.

The `render` method will be called each time an update happens, but as long as we render `<Clock />` into the same DOM node, only a single instance of the `Clock` class will be used. This lets us use additional features such as local state and lifecycle methods.

### Adding Local State to a Class

```js
class Clock extends React.Component {
    constructor(props) { // add a class constructor with props that assigns the initial this.state
        super(props);
        this.state = {date: new Date()};
    }

    render() {
        return (
            <div>
                <h1>Hello, world!</h1>
                {/* replace this.props.date with this.state.date */}
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
            </div>
        )
    }
}

ReactDOM.render(
    <Clock />, // remove props from this element because we use state.
    document.getElementById('root')
);
```

### Adding Liftcycle Methods to a Class

It's very important to free up resources taken up by the components when they are destroyed. Setting up a timer whenever the Clock is rendered to the DOM for the first time is called `mounting` and clearing the timer whenver the DOM produced by the Clock is removed is called `unmounting`.

Special methods for mounting and unmounting are `componentDidMount()` and `componentWillUnmount()`. These methods are called lifecycle methods.

```js
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }

    // The componentDidMount() methods runs after the component output has benn rendered to the DOM.
    // This is a good place to set up a timer.
    componentDidMount() {
        this.timerID = setInterval(
            () => this.tick(),
            1000
        );
    }

    componentWillUnmount() {
        clearInterval(this.timerID);
    }

    // Finally, we will implement a method called tick() that the Clock component will run every second.
    // It will use this.setState() to schedule updates to the component local state.
    tick() {
        this.setState({
            date: new Date()
        });
    }

    render() {
        return (
        <div>
            <h1>Hello, world!</h1>
            <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        </div>
        );
    }
}
```

### Using State Correctly

`Do Not Modify State Directly`

```js
// Wrong
this.state.comment = 'Hello';

// Correct
this.setState({comment: 'Hello'});

// The only place to assign this.state is the constructor.
```

`State Updates May Be Asynchronous`

React may batch multiple setState() calls into a single update for performance.

Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state.

For example, this code may fail to update the counter:

```js
// Wrong
this.setState({
    counter: this.state.counter + this.props.increment,
})

// Correct with an arrow function
this.setState((state, props) => ({
    counter: state.counter + props.increment
}));

// Also correct with a regular function
this.setState(function(state, props) {
    return {
        counter: state.counter + props.increment
    };
});
```

`State Updates are Merged`

When you call setState(), React merges the object you provide into the current state.

For example, your state may contain several independent variables:

```js
constructor(props) {
    super(props);
    this.state = {
        posts: [],
        comments: []
    };
}

// Update variables independently with separate setState() calls.
componentDidMount() {
    fetchPosts().then(response => {
        this.setState({
            posts: response.posts
        });
    });

    // This.setState({comments}) leaves this.state.posts intact, but it completely replaces this.state.comments.
    fetchComments().then(response => {
        this.setState({
            comments: response.comments
        });
    });
}
```

### The Date Flows Down

Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn't care whether it is defined as a function or a class.

This is why state is ofter called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

A component may choose to pass its state down as props to its child components:

```js
function FormattedDate(props) {
    // this component would receive the date in its props and wouldn't know whether it came from the Clock's state,
    // from the Clock's props, or was typed by hand.
    return <h4>It is {props.date.toLocalTimeString()}.</h4>
}

class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            date: new Date()
        };
    }

    componentDidMount() {
        this.timerID = setInterval(
            () => this.tick(),
            1000
        );
    }

    componentWillUnmount() {
        clearInterval(this.timerID);
    }

    tick() {
        this.setState({
            date: new Date()
        });
    }

    render() {
        return (
            <div>
                <h1>Hello, world!</h1>
                <FormattedDate date={this.state.date} />
            </div>
        );
    }
}

// An App component that renders 3 `<Clock />`s showing that all components are truly isolated.
function App() {
    return (
        <div>
            <Clock />
            <Clock />
            <Clock />
        </div>
    );
}

ReactDOM.render(<App />, document.getElementById('root'));
```

It is a top-down or unidirectional data flow. Any state is always owned by some specific component, and any date or UI derived from that state can only affect components below them in the tree.

If you imagine a component tree as a waterfall of props, each component's state is like an additional water source that joins it at an arbitrary point but also flows down.
