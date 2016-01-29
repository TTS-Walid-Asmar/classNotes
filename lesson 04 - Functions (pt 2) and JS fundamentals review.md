
# Lesson 4 - Functions Continued && OOP

## Recap

- Over the past 3 lessons, we've seen all the major building blocks of JS
	- vars, arrays, objects, functions

## Review Functions, Scope and Closures

### Variables
- Every variable you use needs to be declared first
- Declare variables using `var variableName` regardless of what you assign to it.
- A `var` is like a file shortcut, not the file itself.

```javascript
var myVarName = 'AssignedValue';

myVarName; // 'AssignedValue';
```

### Arrays
- Arrays are ordered lists of things.
- Create an array using `[ ]`
- Add and remove items from an array using `.push()` and `.pop()`
- Access or edit array items using `myArray[index]`
- Use arrays to store lists like tweets, slack messages, and RPS hands.

```javascript
var kidNames = ['Evan', 'Noah'];

kidNames[0] //'Evan'
```

### Objects
- Objects are a collection of `key : value` pairs, like a dictionary
- Assign or edit keys in an object using `myObject.myKeyName` syntax
- Use objects when you want to organize and access things by name.

```javascript
var team = {
	quarterback: 'Cam Newton',
	coach: 'Ron Rivera'
}

team.coach //'Ron Rivera'
```

### Functions
- A function is a reusable piece of code that performs a task
- A function is an object, just like objects and arrays
- A function and its return value are not the same thing.
- Defining a function requires PBR (Parameters, body, return value)
- ![PBR](http://i.huffpost.com/gen/2079828/images/n-PABST-BLUE-RIBBON-628x314.jpg)
	- **Parameters** - What input does the function need in order to run?
	- **Body** - What will the function do with that information?
	- **Return** - What output will executing the function give back?

```javascript
function nameOfMyFunction(list, of, parameters) {

	//BODY: Logic and operations based on parameters e.g.

	//RETURN: Value returned to the caller of the function;
	return list;
}

nameOfMyFunction(1,2,3); // 1
```

### Scope
- Only functions create scope
- Variables are only visible inside that scope they are defined in and in its child scopes
- To get the value of a variable `x`, look in the current scope and parent scopes in order.

```javascript
function add() {
	var a = "This variable is only visible inside the add function";
}

console.log(a) // Undefined
```


### Closure
- A Function has access to the variables that were in scope when it was defined, not executed.
- Sorry, this next example is just evil...

```javascipt
//Scope, closure, and hoisting, oh my!

function createFunction() {
	var a = "Hans Zimmer Rules!";
	inception = function() {
		console.log(a);
	}
}

var inception;
createFunction();
inception(); // "Hans Zimmer Rules!"
```


## This Keyword
Inside a function, the keyword `this` refers to the executor of the function.

- PAPER EXERCISE

Again, think of a function as a piece of paper with instructions, a procedure of sorts. One of those instructions might say "touch your nose". But who is this "you" it speaks of? Obviously, the person executing the instructions. Similarly, the keyword 'this' refers to the object that's executing the function.

```javascript
var teacher = {
	name: 'Assaf',
	sayName: function() {
		console.log(this.name);
	}
}
teacher.sayName(); //'Assaf'
```

Different objects can execute the same function, can produce different results because `this` is different.

```javascript
function sayName() {
	console.log(this.name);
}

var teacher1 = {
	name: 'Assaf',
	speak: sayName
}

var teacher2 = {
	name: 'Shane',
	speak: sayName
}

teacher1.speak(); // 'Assaf'
teacher2.speak(); // 'Shane'
```

## Exercise: This

Create a single object named `slideshow` that represents the data and functionality of a picture slideshow. There should be NO VARIABLES OUTSIDE THE OBJECT. The object should have properties for:

1. An array called `photoList` that contains the names of the photos as strings
2. An integer `currentPhotoIndex` that represents which photo in the `photoList` is currently displayed
3. A `nextPhoto()` function that moves `currentPhotoIndex` to the next index `if `there is one, and:
	4. logs the current photo name.
	4. Otherwise, log "End of slideshow";
4. A `prevPhoto()` function that does the same thing, but backwards.
5. A function `getCurrentPhoto()` that returns the current photo from the list.

### Exercise Answer

```javascript
// Have fun :)
```

## Functions as Function Arguments
Because functions are objects, they can be passed in to other functions as arguments.

### A contrived example

```javascript
//Declare an add function
function add(number1, number2) {
	return number1 + number2;
}

//Declare a function that takes a function as an argument
function doMath(operation, number1, number2) {
	return operation(number1,number2);
}

//Pass a function into another.
var sum = doMath(add, 1, 2);
```

### Examples with Arrays
Array's forEach, filter, and sort functions all take a function as an argument.

```javascript
var a = ['ramen', 'pop tarts', 'coffee'];

//Easier iteration
a.forEach(function(item){
	console.log(item);
});

//Custom Filtering
var important = a.filter(function(item){
  return item === 'coffee';
})

//Custom Sorting
a.sort(function(a,b){
	if(a === 'coffee')
		return -1
	else
		return 1
})
```

## Exercise (using array functions)

1. Create an Array named `superHeroes`
	- Inside the superHeroes array create the following arrays: <br>

	```javascript
	["Batman", "Bruce Wayne"],
   ["Spiderman", "Peter Parker"],
   ["The Flash", "Barry Allen"]
   ```
2. Create a `secretIdentity` variable
3. Assign `superHeroes.map()` to the `secretIdentity` variable
4. Assign and anonymous function to `superheroes.map()` as an argument
5. Your anonymous function should have one parameter named `revealArray`
6. Inside the block of your anonymous function:
	- You'll be working with revealArray as an argument
	- Using `revealArray` `return` a string that will `join` the two array items.
	- `join` them together with the string `"is"` <br> *ie: "Batman is Bruce Wayne"*
7. Log the results to the screen and `join` them with a line break.


Exercise Answer:

```javascript
var superHeroes = [ ["Batman", "Bruce Wayne"],
                   ["Spiderman", "Peter Parker"],
                   ["The Flash", "Barry Allen"]];

var secretIdentity = superHeroes.map(function(revealArray){
  return revealArray.join(" is ");
});

console.log(secretIdentity.join("\n"));
```

## Homework
 - Complete the Slideshow challenge and push it to github
 - Read the [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20&%20closures/README.md#you-dont-know-js-scope--closures) book on closures. (chapters 1 - 5)
	- Complete the code examples
	- push to github
- Watch [this](https://teamtreehouse.com/library/understanding-this-in-javascript-2) TeamTreehouse video on scope

### Object Orientation
- https://vimeo.com/69255635
- https://youtu.be/wfMtDGfHWpA
- https://medium.com/javascript-scene/common-misconceptions-about-inheritance-in-javascript-d5d9bab29b0a#.p5acvjy5g
