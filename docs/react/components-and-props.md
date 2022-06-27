---
layout: default
title:  'Components and Props'
parent: React
nav_order: 1
description: "Components and Props"
---

## Components and Props

Components let you split the UI into independent, reusable piecess, and think about each piece in isolation. This page provides an introduction to the idea of components.

Conceptually, components are like JS functions. They accept arbitrary inputs called `props` and return React elements describing what should appear on the screen.

---

### Function and Class Components

The simplest way to define a component is to write a JavaScript function:

```js
function Welcome(props) { // function components
    return <h1>Hello, {props.name}</h1>
}

ReactDOM.render(
    <Welcome name="Thanapoom"/>,
    document.getElementById('root')
);
```

This function is a valid React component because it accepts a single "props" object argument with data and returns a React element. We call such components "function components" because they are literally JavaScript functions.

```js
class Welcome extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}

ReactDOM.render(
    <Welcome name="Thanapoom"/>,
    document.getElementById('root')
);
```

---

### Rendering a Component

```js
const element = <div />; // React element that represent DOM tags.

const element = <Welcome name="Sara" />; // Some elements can also represent user-defined components
```

When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object "props".

```js
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

const natalie = <Welcome name="Natalie" />;

ReactDOM.render(
    natalie,
    document.getElementById('root');
)
```

> Always start component names with a capital letter.

---

### Composing Components

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, all those are commonly expressed as components.

For example, we can create an App component that renders Welcome many times:

```js
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

function App() {
    return (
        <div>
            <Welcome name="Natalie" />
            <Welcome name="Scott" />
            <Welcome name="Aoki" />
        </div>
    );
}

ReactDOM.render(
    <App />,
    document.getElementById('root');
);
```

Typically, new React apps have a single App component at the very top. However, if you integrate React into an existing app, you might start bottom-up with a small component like Button and gradually work you way to the top of the view hierarchy.

---

### Extracting Components

Try to split components into smaller components if it gets confusing.

```js
function formatDate(date) {
    return date.toLocaleDateString();
}

function Avatar(props) { // This component doesn't need to know it is being rendered in Comment.
    return (
        <img className="Avatar"
            src={props.user.avatarUrl}
            alt={props.user.name}
        />
    );
}

function UserInfo(props) {
    return (
        <div className="UserInfo">
            <Avatar user={props.user} />
            <div className="UserInfo-name">
                {props.user.name}
            </div>
        </div>
    );
}

function Comment(props) {
    return (
        <div className="Comment">
            <UserInfo user={props.author} />
            <div className="Comment-text">
                {props.text}
            </div>
            <div className="Comment-date">
                {formatDate(props.date)}
            </div>
        </div>
    );
}

const comment = {
    date: new Date(),
    text: 'I hope you enjoy learning React!',
    author: {
        name: 'Hello Kitty',
        avatarUrl: 'https://placekitten.com/g/64/64',
    },
};

ReactDOM.render(
    <Comment
        date={comment.date}
        text={comment.text}
        author={comment.author}
    />,
    document.getElementById('root')
);
```

---

### Props are Read-Only

Whether you declare a component as a function or class, it must never modify its own props. Consider this sum function.

```js
function sum(a, b) {
    return a + b;
}
```

Such functions are called "pure" because they do not attempt to change their inputs, and always return the same result for the same inputs.

In contrast, this function is impure because it changes its own input.

```js
function withdraw(account, amount) {
    account.total -= amount;
}
```

React is pretty flexible but it has a singple string rule.

All React components must act like pure functions with respect to their props.

Of course, application UIs are dynamic and change over time. In the next section, we will introduce a new concept of "state". State allows React components to change their output over time in response to use actions, network responses, and anything else, without violating this rule.