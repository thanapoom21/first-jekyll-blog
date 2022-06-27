---
layout: default
title:  'Conditional Rendering'
parent: React
nav_order: 4
description: "Conditionally Rendering Components"
---

## [Conditional Rendering](https://reactjs.org/docs/conditional-rendering.html)

### Conditional rendering is the same in React and JavaScript

It is possible to create distinct components that encapsulate a specific behavior in React. Also, components can be rendered depending on the state of the application.

`if()` or `condition ? expressionTrue : expressionFalse` can be used to create elements representing the current state, and React will update the UI to match them.

Greeting components displaying either of UserGreeting or GuestGreeting components depend on whether a user is logged in:

```js
function UserGreeting(props) {
    return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
    return <h1>Please sign up.</h1>;
}

function Greeting(props) {
    const isLoggedIn = props.isLoggedIn;
    if (isLoggedIn) {
        return <UserGreeting />;
    }
    return <GuestGreeting />;
}

React.DOM.render(
    <Greeting isLoggedIn={false} />,
    document.getElementById('root')
);
```

[Try it on CodePen](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)

### Element Variables

Elements can be stored in variables and will help conditionallt render a part of the component while the rest of the output doesn't change.

This is an example showing a stateful component called `LoginControl`.

```js
function LoginButton(props) {
    return (
        <button onClick={props.onClick}>
            Login
        </button>
    );
}

function LogoutButton(props) {
    return (
        <button onClick={props.onClick}>
            Logout
        </button>
    );
}

class LoginControl extends React.Component {
    constructor(props) {
        super(props);
        this.handleLoginClick = this.handleLoginClick.bind(this);
        this.handleLogoutClick = this.handleLogoutClick.bind(this);
        this.state = { isLoggedIn: false };
    }

    handleLoginClick() {
        this.setState({ isLoggedIn: true });
    }

    handleLogoutClick() {
        this.setState({ isLoggedIn: false });
    }

    render() {
        const isLoggedIn = this.state.isLoggedIn;
        let button;

        if (isLoggedin) {
            button = <LogoutButton onClick={this.handleLogoutClick} />;
        } else {
            button = <LoginButton onClick={this.handleLoginClick} />;
        }

        return (
            <div>
                <Greeting isLoggedIn={isLoggedIn} />
                {button}
            </div>
        );
    }
}

ReactDOM.render(
    <LoginControl />,
    document.getElementById('root')
);
```

While declaring a variable and using an `if` statement is a fine way to conditionally render a component, sometimes you might want to use a shorter syntax. There are a few ways to inline conditions in JSX.

### Inline If with Logical && Operator

You may embed any expressions in JSX by wrapping them in curly braces. This includes the JavaScript logical && operator. It can be useful for conditionally including an element:

```js
function Mailbox(props) {
    const unreadMessages = props.unreadMessages;
    return (
        <div>
            <h1>Hello!</h1>
            {/* check if unreadMessages length is more than 0 and render the JSX */}
            {unreadMessages.length > 0 &&
                <h2>
                {/*
                    If the condition is true, this element will be rendered
                    However, if it is not, React will ignore and skip it.
                 */}
                    You have {unreadMessages.length} unread messages.
                </h2>
            }
        </div>
    );
}
```

In JavaScript, `true && expression` always evaluates to expression, and `false && expression` always evaluates to false.

### Inline If-Else with Conditional Operator

Another method for conditionally rendering elements inline is to use JavaScript conditional (ternary) operator `condition ? true : false`.

```js
render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
        <div>
            The user is {isLoggedIn ? 'currently' : 'not'} logged in.
        </div>
    );
}
```

The condition can get larger due to the context.

```js
render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
        <div>
            {isLoggedIn ? (
                <LogoutButton onClick={this.handleLogoutClick} />
            ) : (
                <LoginButton onClock={this.handleLoginClick} />
            )}
        </div>
    );
}
```

Whenever conditions become too complicated, extracting a component is a great idea before things get out of hand.

## Preventing Component from Rendering

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return `null` instead of its render output.

In the example below, the `<WarningBanner />` is rendered depending on the value of the prop called `warn`. If the value of the prop is false, then the component does not render:

```js
function WarningBanner(props) {
    if (!props.warn) {
        return null;
    }

    return (
        <div className="warning">
            Warning!
        </div>
    );
}

class Page extends React.Component {
    constructor(props) {
        super(props);
        this.state = {showWarning: true};
        this.handleToggleClick = this.handleToggleClick.bind(this);
    }

    handleToggleClick() {
        this.setState(state => ({
            showWarning: !state.showWarning
        }));
    }

    render() {
        return (
            <div>
                <WarningBanner warn={this.state.showWarning} />
                <button onClick={this.handleToggleClick}>
                    {this.state.showWarning ? 'Hide' : 'Show"}
                </button>
        )
    }
}
```
