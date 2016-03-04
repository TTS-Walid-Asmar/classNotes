# Lesson 14 - React

## Recap & Intro
- We've come a long way! We've learned
	- JavaScript Basics
	- Browser & DOM
	- API's and Async
	- Application setup & Compilation
- At this point we have all the concepts we need
- Today, we learn about React!

## What is React?

- Open source view framework developed by Facebook
- React lets you define interfaces in JavaScript
- React can render anywhere to anything
- Simple, powerful, and composable

## What React Isn't
- React is not a web framework (not tied to html/css). 
- React isn't a full fledged application framework
- React is not Flux or Redux

## Core concepts

### Components
Everything in React is a component. Think of a component as custom html tag that renders its own interface and comes packaged with its own behavior (click events, etc)


### Virtual DOM
When you render a React component, it renders to a virtual representation of the interface in memory. This is called the "Virtual DOM". React then uses any number of open source plugins to render the virtual DOM state to an interface like the real DOM, a canvas, or a native mobile app.

### ReactDOM
The library we'll be using to render React components to an HTML document is called reactDOM. ReactDOM converts your React state to HTML. It manages the HTML and event handlers for you so that subsequent renderings of your application make the fewest possible changes to the actual DOM, increasing performance.

**Important** - React abstracts away the DOM interface. You're not really writing HTML.

### JSX
Most React code is written in a superset of JavaScript called JSX. It lets you create components in your JS code in a way that looks more familiar (like HTML). Keep in mind, this IS NOT HTML. It's also not really JS. It needs to be compiled by webpack into a .js file that a browser can read.


## Defining & Rendering Components
Let's define our first component.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class HelloMessage extends React.Component {
  render() {
    return (
    	<div>Hello World</div>
    );
  }
}
var mountPoint = document.querySelector('#app');
ReactDOM.render(<HelloMessage/>, mountPoint);

```

A few things to note:

- We are importing React and ReactDOM separately.
- We are using ES6 class syntax to define a new component class that inherits from React's default component
- We are defining a `render` method, which is the only method required to define a component class.
- The render method returns a React component. Use parens to allow multi-line component definitions, and make sure you're returning a single root component. The root component can have children, but not siblings.
- We are using ReactDOM to render an *instance* of our class to an element on the html document.

### JSX Caveats
Like we said before, JSX isn't really HTML. Because it lives in the same context as JavaScript, it has some special rules you need to follow when returning components from your render method.

- Attributes with the same names as reserved JS keywords can't be used. For example `class` and `for` are changed to `className` and `htmlFor` in JSX.
- All tags have to be closed. `<input type="text">` for example, has to include a closing tag `<input type="text" />`.

## Exercise 1: Rendering a Simple Component
Let's create our first component! 

1. Clone the boilerplate project from `git@github.com:ttsJavaScriptApps/webpack-boilerplate.git`
2. In index.jsx, create a component class called `MessageInput` that renders:
	1. A text input called
	2. A label for the text input (clicking it should focus the input)
	3. A 'send' button
3. Render an instance of your component to the DOM.

## Making our JSX Dynamic
Let's take a deeper look at the render method. This is essentially the equivalant of our view template in other frameworks (ejs, handlebars, mustache, jade, haml, etc.).

The basic functionality needed in any templating language is 

1. Token Replacement
2. Conditions
3. Iteration

Let's take a look at how we do that in JSX.

### State
Before we make our templates dynamic we need data to drive them. The data that components need to render is kept in a special property object called `state`.

We add a constructor method to define our component's state object.

```javascript
class HelloMessage extends React.Component {

  constructor() {
    super(); //needed for inheritance
    
    //Define an intial state object
    this.state = {
  		message : 'Hello World'
  	};
  }

  render() {
    return (
    	<div>Hello World</div>
    );
  }
}
```

Now that we have data, we can start making our templates dynamic.

### Token Replacement
Token replacement is simply taking variables and putting their value into the template. In JSX, use curly braces with a variable name:

```javascript
// this.state = { message : 'Hello World'};
render() {
	return (
		<div>{this.state.message}</div>
	);
}
```

Tokens don't have to come exlusively from state. They can be calculated. e.g.

```javascript
render() {
	var statement = "message is: " + this.state.message;
	return (
		<div>{statement}</div>
	);
}
```

### Conditions
Often times we want to render one thing in one case, and something else in another case. There are a couple of ways to do this with JSX

#### Use JavaScript!

```javascript
// this.state = { person : 'Matt'};

render() {
	
	//Figure out the elements in advance
	if(this.state.person)
		message = "Hello " + this.state.person;
	else
		message = "I'm all alone... and sad";
	
	return (
		<div>{message}</div>
	);
}
```

### Inline
If you prefer an inline look to your template, remember, you can run functions inside those curly braces. Use an IIFE to generate the value you want

```javascript
render() {
	return (
		<div>
			{(() => {
	       	if(person)
					return "Hello " + this.state.person;
				else
					return "I'm all alone... and sad";
	      	})()}
		</div>
	);
}
```

### Iterative Rendering
Sometimes you want to render the same elements repeatedly for every item in an array. JSX lets you put an array of components inside the `{}` for rendering. All you have to do is create it based on the data you want to render.

```javascript
//this.state = { studentNames: ['Matt', 'Katy', 'Mariel', 'Lee'] }

render() {	
	//Figure out the elements in advance
	var students = this.state.studentNames.map(function(name){
		return (<li key="name">{name}</li>)
	})
	
	return (
		<ul>{students}</ul>
	);
}
```

Something to note above is the use of the `key` property. In order for React to keep track of and property update dynamically generated elements, give them a unique "key". Above, we're using the student name, which isn't guaranteed to be unique, so be careful choosing your key.

## Dynamic Class Names
One of the most common dynamic elements is the list of classes an element has applied. The simple way to do this is to use javascript logic to concat a string.

```javascript
//this.state = {active: true}

render() {
	var classes = 'item'
	
	if(isActive) {
		classes += 'active';
	}
	
		
	return (
		<div className={classes}></div>
	);
}

```

A better approach is to use the 'classnames' module

```javascript
import classNames from 'classnames';

render() {
	var classes = classNames({
		'item' : true,
		'active' : isActive
	})	
		
	return (
		<div className={classes}></div>
	);
}

```

## Inline styles
Because React brings HTML, CSS, and JS into the same context, you can use POJOs(Plain ol' JavaScript Objects) as styles for your components!

```javascript
render() {
	var headerStyle = {
		color: 'red',
		textDecoration: 'underline'
	}
		
	return (
		<h1 style={headerStyle}></h1>
	);
}
```

You can define styles in another module and import them.

## Exercise 2: Rendering Components with Logic
Let's add some logic to our little component.

1. Declare an array of message objects with properties for `text`, `time`, and `user`
2. Add a `ul` with an `li` element for each message containing the message, user's name and timestamp.
3. Declare a variable `currentUser` and set it to one of the users in the messages array
4. If a message's author is the `currentUser`, add a class to it that turns the message red.
5. Only show the most recent 3 messages.
6. If there are more than 3 messages, show a "load more" button

## Homework 

- Start Reading the [React Documentation](https://facebook.github.io/react/docs/getting-started.html)
- Complete the first 3 challenges (stop at Step 2 - Part 3) of [Thinking in React](https://github.com/asbjornenge/thinking-in-react) module


- [https://facebook.github.io/react/docs/getting-started.html](https://facebook.github.io/react/docs/getting-started.html)
- [https://www.nczonline.net/blog/2016/01/react-and-the-economics-of-dynamic-web-interfaces/](https://www.nczonline.net/blog/2016/01/react-and-the-economics-of-dynamic-web-interfaces/)
- [https://github.com/eanplatter/react-starter](https://github.com/eanplatter/react-starter)
