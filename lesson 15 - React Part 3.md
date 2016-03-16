## Recap & Intro

- The last 2 classes have been an intro to the basics of React
- We've covered: 
	- Components
	- JSX
	- Event Handlers
	- State
- Tonight we'll continue by discussing: 
	- Component Composition
	- Props 


## Composition
Up to now we've done all of our work in a single component, but React's strength is to break up applications into many re-usable components and compose them. React components we define can be used the same way as built-in react components like `<div>` and `<h1>`.

For example, if we define a ToDoItem component, we can re-use it in a ToDoList component.

```javascript
class TodoItem extends React.Component {
	constructor(){
		super();
		this.state = {text: "default text"}
	}
	render() {
   		return (
    		<div>{this.state.text}</div>
    	);
  }
}

class TodoList extends React.Component {
	constructor(){
		super();
		this.state = {listItems: [
			{text: "Gym"},
			{text: "Tan"},
			{text: "Laundry"}
		]}
	}
	render() {
		var comps = this.state.listItems.map(function(item){
			return <TodoItem />
		})
		
		return (	
			<div>{comps}</div>
		);
	}
}
```

## Props
In the previous example, you probably noticed that we didn't pass any data from our `listItems` to our child components. We can do this using Props.

From HTML, we're familiar with the concept of "properties" that configure an element's behavior. For example, an `<a>` tag accepts properties including `href` and `name` that are supplied by user of the element 

```
<a href="# name="myLink"></a>
```

React components have a similar concept called `props`. Props are JavaScript objects supplied by creator of a component instance.

##### A Simple Example
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class HelloMessage extends React.Component(
  render() {
    return <div>Hello {this.props.name}</div>;
  }
);
ReactDOM.render(<HelloMessage name="Shane" />, 
					document.getElementById('root'));
```


### Passing Props to Children

```javascript
class ChildComponent extends React.Component{
  render() {
    return <div style={{
      color      : this.props.color,
      background : this.props.background
    }}>
      I am {this.props.color}
    </div>
  }
}

class ParentComponent extends React.Component{
  render() {
    return (
      <ChildComponent background={this.props.background} color={this.props.color} />
    );
  }
)

ReactDOM.render(
  <ParentComponent color="green" background="red" />, 
  document.getElementById('root'));
```



### Passing JS Objects as Props
In the previous example we passed a simple string in as the property value like we're used to doing in HTML. With JSX, you can pass any JS object in as a property. This includes objects, arrays, and even functions!


#### todoList.jsx
```javascript
class TodoList extends React.Component {
  constructor() {
   super();
   this.state = {listItems: [
     {text: "Gym"},
     {text: "Tan"},
     {text: "Laundry"}
   ]}
  }
  render() {
		var comps = this.state.listItems.map(function(item, i ){
			return <TodoItem key={i} item={item} />
		})

		return (
			<span>{comps}</span>
		);
	}
}

class TodoItem extends React.Component {
	render() {
		return (
		  <div>{this.props.item.text}</div>
		)
	}
}


ReactDOM.render(
  <TodoList/>,
  document.getElementById('root'));
```

### Passing Functions as Props
Remember, functions are Objects! This means you can pass them in as props as well. This is a very powerful feature.

```javascript
class TodoList extends React.Component {
   constructor() {
    super();
    this.state = {listItems: [
      {text: "Gym"},
      {text: "Tan"},
      {text: "Laundry"}
    ]}
  }
  completeItem(item) {
    console.log('completed item', item);
  }
  
  render() {
    var comps = this.state.listItems.map((item,i) => {
      return <TodoItem key={i} item={item} completeItem={this.completeItem.bind(this, item)} />
    })
    
    return (  
      <span>{comps}</span>
    );
  }
}

class TodoItem extends React.Component {
  constructor() {
    super();
  }

  render() {
    return (
      <h1 onClick={this.props.completeItem}>{this.props.item.text}</h1>
    )
  }
}
``` 

### Default Props
You can set default values for properties by setting a `defaultProps` property on the class.

```javascript
TodoItem.defaultProps = {text: "default text"}
``` 

### Validating Props
You can also specify the types of properties 
```javascript
TodoItem.propTypes = { item: React.PropTypes.object };
```

## Exercise 1: Composition
Let's break apart our monolithic component a bit.

1. Create a component that displays a single message based on a "message" object prop.
2. Validate that the message prop is an object using `propTypes`
3. Refactor your main component to use the message component to display messages.
4. Create a component for your message inputs (text and save button)
5. Refactor your main component to use the messageInputs component
	- Make sure all buttons and events still work as expected.


# Homework

- finish the [Thinking In React](https://github.com/arkency/reactjs_koans) module 
- Pass the first 4 [React Koans](https://github.com/arkency/reactjs_koans) tests
