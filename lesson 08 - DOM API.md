# Lesson 8 - DOM

## Recap & Intro
- Last week we introduced working with JavaScript in the browser
	- window object
	- DOM
- Today, we're going to dive in to working with the DOM.

## DOM
To recap, the Document Object Model (DOM) is a JavaScript representation of the HTML document loaded into the browser. The DOM API lets you:

- Find elements (nodes) in the document
- Edit, add, or remove nodes
- Attach event handlers that respond to user input.

## Find Nodes
There are a few different ways to get a node or array of nodes from the document.

### getElementById()
Gets a single node based on id attribute

**HTML**<br>
```html
<input id="username"></input>
```

**JS**<br>
```javascript
//Get a single node
var el = document.getElementById('username');
```

### getElementsByTagName() & getElementsByClassName()
Gets an array of nodes based on a tag name or className

**HTML**<br>
```html
<input type="text" class="error"></input>
<input type="password"></input>
<button>submit</button>
```

**JS**<br>
```javascript
//Get all inputs
var inputs = document.getElementsByTagName('input');
var inError = document.getElementsByClassName('error');

inputs.length; //2
inError.length; //1

```

### querySelector & querySelectorAll
In practice, these are the only DOM selectors you will ever need. They take a CSS selector as an argument, which means you can easily duplicate the functionality from the other DOM selection functions.

**HTML**<br>
```html
<input type="text" class="error"></input>
<input type="password" class="password"></input>
<button>submit</button>
```

**JS**<br>
```javascript
//Get all inputs
var firstButton = document.querySelector('button');
var inError = document.querySelectorAll('input.error');

firstButton //single button node
inError //Node list of inputs with class 'error'

```

## NodeList vs Array
It seems like querySelectorAll should return an Array of elements. In fact, it returns a nodeList, which offers a similar, but not identical API to Array.

```javascript
var links = document.querySelectorAll('a');

//Works!
var linkCount = links.length;
var firstLink = links[0];

//Doesn't work!
links.forEach(function(link){
	//do something with link
});

```

Array methods like `forEach`, `map`, `reduce`, and so on, don't work. Luckily, its easy enough to convert a nodeList into an Array;

```javascript
var links = document.querySelectorAll('a');
var arrayOfLinks = Array.from(links);
```

## Traversing the DOM
You can use the children, parent, nextElmentSibling, and previousElementSibling attributes to find nodes relative to a node you have. This is called "traversing the DOM".

### Children
Use the children property to gets a **nodeList** of all the nodes contained in the node.

**HTML**<br>
```html
<ul>
  <li>Item 1</li>
	<li>Item 2</li>
</ul>
```

**JS**<br>
```javascript

var listItems = document.querySelector('ul').children;
listItems.length; //2

```

### Siblings and Parents
Use parent, nextElementSibling, and previousElementSibling to find nodes up the tree and across it.

**HTML**<br>
```html
<header>
	<ul>
		<li class="first">Item 1</li>
		<li class="selected">Item 2</li>
		<li class="last">Item 3</li>
	</ul>
</header>
<section>
	Hello!
</section>
```

**JS**<br>
```javascript

var selectedItem = document.querySelector('li.selected')
var first = selectedItem.previousElementSibling;
var last = selectedItem.nextElementSibling;
var list = selectedItem.parent;
var header = selectedItem.parent.parent;
var section = selectedItem.parent.parent.nextElementSibling;
```

## Exercise 1: Selecting Nodes
```html
<html>
	<body>
		<header>
			<ul>
				<li class="first">Item 1</li>
				<li class="selected">Item 2</li>
				<li class="last">Item 3</li>
			</ul>
		</header>
		<div class="col">
			<section>
				<h2>Section 1</h2>
			</section>
			<section class="current">
				<h2 class="highlight">Section 2</h2>
			</section>
			<section>
				<h1>Section 2</h1>
			</section>
		</div>
	</body>
</html>
```

1. Get the header element
2. Get all the section elements
3. Get the section element with class="current"
4. Get the section that comes after the current section
5. Get the h2 node from the section before the 'current' section
6. Get the div that contains the section that has an h2 with a class of 'highlight'
8. Get all the sections that contain an H2 (using a single statement);


## Exercise 1 Answer

```javascript
//Laziness is your friend...
var q = document.querySelector.bind(document),
	qa = document.querySelectorAll.bind(document);


q('header');
qa('section');
q('section.current');
q('section.current').nextElementSibling;
q('section.current').previousElementSibling.children[0];
q('h2.highlight').parent.parent;

Array.from(qa('section h2'))
	.map(function(headline){
		return headline.parent;
	})
```

## Editing a node
A **Node** object has some useful properties and methods to let you access its contents and edit its appearance and content.

### innerHTML
The sledgehammer approach. Get or set the html text inside a node. This is really simple and sufficient in most cases.

**HTML**<br>
```html
<div>
  <a href="#">Click me</a>
</div>
```

**JS**<br>
```javascript
//Get all inputs
var div = document.querySelector('div');
var a = document.querySelector('a');

a.innerHTML; //"click me"
div.innerHTML; //'<a href="#">Click me</a>'

a.innerHTML = "Updated link text";

```

### Attributes
Get and set attributes like object properties

```html
<a href="http://google.com" name="googleLink">Click me</a>
```

```javascript
var a = document.querySelector('a');

//Get an attribute
a.href; //"http://google.com"

//Set an attribute
a.name = 'new link name';

//Add a new attribute
a.target = "_blank";

```

### Removing nodes
Use `remove` to remove a node from a document.

**HTML**<br>
```html
<header>
	<ul>
		<li class="first">Item 1</li>
		<li class="selected">Item 2</li>
		<li class="last">Item 3</li>
	</ul>
</header>
<section>
	Hello!
</section>
```

**JS**<br>
```javascript
//Remove the first list item
document.querySelector('.first').remove();
```

### Adding nodes
Create a node using `document.createElement('tagname')` and `node.appendChild(el)`

**HTML**<br>
```html
<header>
	<ul>
		<li>Item 1</li>
	</ul>
</header>
```

**JS**<br>
```javascript
var newLI = document.createElement('li');
newLI.innerHTML = "Item 2";

var list = document.querySelector('ul');
list.appendChild(newLI); //Insert after item 1
```

## Exercise 2: Editing the DOM

```html
<html>
	<body>
		<h2>Shopping List</h2>
		<ul id="list">
			<li>Ramen</li>
			<li>Coffee</li>
			<li>Poptarts</li>
			<li>Twinkies</li>
		</ul>

	</body>
</html>
```

1. Update the 'Coffee' item to say 'Fair Trade Coffee'
2. Remove 'Twinkies' from the list
3. Add an item 'Cheese Whiz'
4. Clear the list and programmatically add items from the array `['protein powder', 'muscle milk', 'power bars']`
5. Add the class 'important' to the muscle milk item.

## Exercise 2: Answer
```JavaScript
var list = document.querySelector('#list');

//1
list.children[1].innerHTML = "Fair Trade Coffee";

//2
list.children[3].remove();

//3
var cheese = document.createElement('li');
cheese.innerHTML = 'Cheese Whiz';
list.appendChild(cheese);

//4
list.innerHTML = '';
['protein powder', 'muscle milk', 'power bars'].forEach(function(itemText){
	var li = document.createElement('li');
	li.innerHTML = itemText;
	list.appendChild(li);
})

//5
list.children[1].className = "important"

```

- Update text of an item
- Remove an item
- Add an item
- Add a series of items based on data
- Update attributes based on data

## DOM Events
Elements emit events based on user input. You can run code in response to them. Events include

Async programming is important in JS.

```javascript
var myElement = document.getElementById('myEl');
el.addEventListener('click', function(event){
	alert('clicked!');
})

//Combine with DOM editing
el.addEventListener('mouseover', function(event){
	el.innerHTML('over');
})

```

- **Mouse events** - click, mouseover, mouseout
- **Keyboard events** - keydown, keyup, etc
- **Form events** - submit, blur, focus, change,
- **window events** - load, hashchange, etc.
- **touch events** - touchstart, touchend, etc.


[http://www.w3schools.com/jsref/dom_obj_event.asp](http://www.w3schools.com/jsref/dom_obj_event.asp)

### Event object
You may be wondering what that event parameter is. An event object is passed to the event handler that describes what happened. The event object is different depending on the type of event.

- target - element where event occurred
- Mouse: clientX, clientY
- Keyboard: keyCode, shiftKey

### Event Bubbling
When an event is triggered on an element, it then gets fired on that element's parents, all the way to the top.

- event.target is the element where the event originally occurred
- event.currentTarget is the element running the event handler (this!).

```html
<div class="outer">
	<div class="inner">click me</div>
</div>
```

```javascript
document.querySelector('.outer').addEventListener('click', function(e){
	console.log(e.target, e.currentTarget);
	//inner, outer
})
```

## Exercise & Homework

Let's create a simple todo application

- Show an unordered list of todo's
- Show an input to enter a new todo
- Show a button to add a todo. When the button is clicked:
	- The text from the input box is used to add a list item to the bottom of the list
	- The text from the input box is cleared out.
- When the user clicks on a list item, it is removed
- **Extra Credit:** - When a list item is clicked, cross it out, then remove it after 1 second.

- Complete the [CodeSchool jQuery](https://www.codeschool.com/courses/try-jquery) course


## Reading
[https://developer.mozilla.org/en-US/docs/Web/API/Node](https://developer.mozilla.org/en-US/docs/Web/API/Node)

[https://developer.mozilla.org/en-US/docs/Web/API/Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)

[https://developer.mozilla.org/en-US/docs/Web/API/NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)
