# Lesson 14 - React Part 2

## Recap & Intro

- Last time we learned about rendering a component in React
- Today we'll continue learning about React components and composition

## Event Handling
JSX has a similar event system to the DOM, but better. Because events are supplied by the React library, not the web browser, you get 100% consistency. No browser quirks!

### Wiring up a handler

```javascript
class HelloMessage extends React.Component {

  constructor() {
    super();
    this.state = {
  		clickCount : 0
  	};
  }

  _handleClick(event) {
  	console.log('clicked!', event)
  }

  render() {
    return (
    	<button onClick={this._handleClick.bind(this)}>Click me!</button>
    	<div>{this.state.clickCount}</div>
    );
  }
}
```

### Updating state
A user event like a click probably means an update to your state. When updating state make sure to use `this.setState()` and not update state directly. This is how React keeps track of the fact that state is changing!

```javascript
_handleClick(event) {
  	var clicks = this.state.clickCount;
  	this.setState({clickCount: clicks + 1})
 }

```

## Forms and User Input
Forms are where the difference between classic DOM elements and JSX components becomes more evident.

### Controlled Components

Using the "value" property, this text input will ALWAYS have the value "Hello!", no matter what the user does

```
render() {
    return <input type="text" value="Hello!" />;
  }
```

In order to let the user update its value, use the onChange event handler to update state.

```javascript
_handleChange: function(event) {
	this.setState({value: event.target.value});
}

render() {
	return (
	  <input
	    type="text"
	    value={this.state.value}
	    onChange={this._handleChange.bind(this)}
	  />
	);
```

### Uncontrolled Components
If you omit the value property, and use defaultValue instead, you get more familiar behavior.

```javascript
render: function() {
    return <input type="text" defaultValue="Hello!" />;
  }
```

### Refs
Because an uncontrolled component isn't bound to any value in our state, getting its value requires us to name it using a "ref" attribute

```javascript

_clickHandler(event) {
	console.log(this.refs.message.value)
}

render() {
    return (
    	<div>
    		<input type="text" ref="message" defaultValue="Hello!" />
    		<button onClick={this._clickHandler.bind(this)}>Click me</button>
    	</div>
    );
  }
```

## Exercise 1: Handling Events

1. Wire up a click event handler to your button.
2. Clicking the button takes the message from the text input and adds it to the message list.
3. Clicking the button clears the message from the input box.
4. If there is no text in the input box, the button should do nothing.
5. Add an edit button to messages sent by the current user.
6. Clicking the edit button sets a state property to the index of the item being edited
