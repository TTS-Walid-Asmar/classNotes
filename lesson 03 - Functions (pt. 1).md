# Lesson 3 - Functions

## Recap & Intro
- Last week we talked about arrays and objects, which hold structured data.
- What can we do with that data? This is where functions come in.
- Today we'll talk about functions, parameters, scope, closures
- JavaScript is a functional language, so it's all about the functions! 
- Functions usually people new to JavaScript, because they work differently than in other languages like Ruby, Java or C#.

## Function Basics
- Functions are procedures that take arguments and return values. 
- Think about them like a sheet of paper with instructions on it.

### Define a function

- A function has a name, an argument list, and a body. 
- Arguments can be named anything you want

```javascript
//Define a function
function saySomething(something) {
	console.log(something);
}
```

### Running a function
Execute a function by calling it's name with () and passing in arguments.

```javascript
saySomething('Hello function!'); //logs 'hello function!'
```

### Return values

Functions can 'return' a value. A function evaluates to its `return` value when run.<br>
*(note to Ruby Developers: Ruby _implicitly_ returns values. JS requires an _explicit_ return statement if you are expecting a return value)* 


##### Wrong

```javascript
function add(number1, number2) {
	number1 + number2;
}

var sum = add(1,2);
console.log(sum) // undefined
```

##### Right

```javascript
function add(number1, number2) {
	return number1 + number2;
}

var sum = add(1,2);
console.log(sum); // 3
```

### Function Arguments
All functions take any number of arguments, regardless of their declared signature.

```javascript
function add(a,b) {
	console.log(a,b)
}

add(1); // '1,undefined'
add(1,2,3,4,5) // '1,2'
``` 

The arguments list simply creates variables that reference the arguments in order they were passed. For functions that take an unknown number of arguments, use the `arguments` object.

```javascript
function add() {
	var sum = 0;
	for(var i = 0; i < arguments.length; i++) {
		sum += arguments[i];
	}
	return sum;
}
	
add(1,2,3,4,5,6,7,8);
```

## Exercise 1 (function basics)

Calculating gratutity is a repetitive task, so let's create a couple of functions that do the work for us.

- create a variable titled `billAmount` and store a random number (ie: 100)
- create a function titled `gratuity()`
	- gratutity should: 
		- multiply the value of billAmount by 20% <br> *hint: use 0.2*
		- return the value
- create a function titled `totalWithGrat()`
	-  totalWithGrat should:
		- take in the amount as an argument
		- call the gratutity function
		- add that to the original bill amount
		- return the total bill + gratuity
- log the total (with gratuity) to the console 
	- append new total to the following phrase: 
		- "your total including gratuity is:" 
- **Limitation: You can only invoke the totalWithGrat function when logging the result**

**Mariel's Bonus Question**

- Find a way to fix the decimal point to only 2 places, ie: 100.00 <br> *(hint: the answer is in the sentence above)*

### Exercise 1 Answer:

```javascript

var billAmount = 100.58;

function gratutity(){
    return billAmount * 0.2;  
  }

function totalWithGrat(amount){
  return gratuity() + amount;
}

console.log("your total, including gratutity is: $" +  totalWithGrat(billAmount).toFixed(2));
```

## Functions as Objects

- Functions are first class Objects in JavaScript.
- This means they can be: 
	- instantiated
	- assigned
	- reassigned 
	- and passed around just like any other variable.
- Again, think of them as a physical piece of paper.

### Functions as Variables
Like other objects, functions can be assigned to variables.

```javascript
var add = function(a,b){return a + b};
```

The difference between declaring a function that way (Function assignment) and the `function add(){}` syntax we've been using (Function Declaration) is that the latter hoists both the declaration and definition. For example:

##### Function Declaration
```javascript
hoisted(); // logs "foo"

function hoisted() {
  console.log("foo");
}
```

##### Function Assignment

```javascript
notHoisted(); // TypeError: notHoisted is not a function

var notHoisted = function() {
   console.log("bar");
};
```

In the above example, we get a `TypeError` because `notHoisted()` is declared, but undefined *until* the assignment expression `var notHoisted = `.

### Anonymous Functions

- It's often handy to declare a function on the fly without a name.
- This is VERY common

```javascript

var calculator = {
	add: function(a,b) {
		return a + b;
	}
}

calculator.add(2,3) // 5
```

So, what is the point of an Anonymous function? 

- Cleaner code
- Scope management, used to create private scope (more on that later)
- Super useful with Closures (more on that later as well)


While, this might look strange, but you've already been doing this with Arrays and Objects. Functions are no different.

```javascript
var arrayOfMystery = [
	['anonymous','array'],
	{ name: 'anonymous object' },
	function(){ return 'Anonymous Function!'}
]

console.log(arrayOfMystery[0][1]) // array
console.log(arrayOfMystery[1].name) // anonymous object
console.log(arrayOfMystery[2]()) // anonymous function!

```
- `[ ]` creates an array in memory
- `{ }` creates an object in memory
- `function(){ }` creates an object in memory


## Exercise 2 (RPS Revisited)

Let's revisit Rock Paper Scissors...

1. Define a `hands` array with the values 'rock', 'paper', and 'scissors';
2. Define a function called `getHand()` that returns a hand from the array using `parseInt(Math.random()*10)%3` 
4. Define two objects for two players. Each player has `name` and `getHand()` properties.
5. Define a function called `playRound()` that 
 	- Takes two player objects as arguments 
 	- Gets hands from each
 	- Determines the winner
 	- Logs the hands played and name of the winner.
 	- If its a tie, log the hands played and "it's a tie".
 	- Returns the winner object (null if no winner)
6. Define a function called playGame() that takes arguments `player1`, `player2`, and `playUntil`.
	- Play rounds until one of the players wins `playUntil` hands
	- When one player has won enough games, return the winning player object
7. Play a game to 5 wins

**Mariel's Bonus Questions**

- Define a function caled playTournament()
	- Take 4 players and `playUntil` as arguments
	- Play a game between the first two players, and the second two players
	- Play a game between the winners of the first round.
	- Announce the tournament winner's name "[name] is the world champion";

### Exercise 2 Answer

```javascript
// figure it out...

```

## Scope
- Scope is the set of variables a piece of code has access to. 
- Functions create a scope, nothing else.
- Parameters and variables declared inside a function are LOCAL to that function's scope

So, what is a lexical scope? Lexical scoping simply means that variables refer to their local environment.

```javascript
function getGreeting(name) {
	var greeting = "hello ";
	return greeting + name;
}
getGreeting('Shane'); // 'Hello Shane'
console.log(greeting) //undefined
```

Blocks DO NOT have their own scope

```javascript
for(var i = 0; i < 10; i++) {
	var x = i;
}

console.log(i,x); //9,9
```

Scope lets you create private variables in a JS program, but be careful, variables defined without the 'var' keyword are global.

```javascript
function greet(name) {
	greeting = "hello ";
	return greeting + name
}
greet('Shane');
greeting //'hello'!
```

### Immediately Invoked Function Expressions (IIFE)

Sometimes you want to create a scope so that you can have private variables. One way to do that is to create an anonymous function and immediately execute it.

```javascript

(function(){
	var myVar = "look mom, private variables!";
	
	//Do some stuff with myVar...
})();

console.log(myVar); //undefined;

```

## Scope Chain and Closures
- When a scope is created, it is nested inside another scope. 
- To resolve a variable, JS looks at the immediate scope, then parent scopes in order. 
- A **closure** is an inner function that has access to the outer function’s variables. Both scopes together are the **scope chain**.

For example, the `city` variable is visible inside the `greet` function because the greet function creates a closure.

```javascript
var city = 'Charlotte';
var greet = function() {
	console.log('Hello ' + city);
}

greet(); // Hello Charlotte
```
Accessing `city` inside the greet function will first look for a variable named `city` in the greet function, then to the parent scope, and so on up the scope chain. 

##### Nested scope Example 1

```javascript
function outer() {
	var x = 'x';
	function inner() {
		var y = 'y';
		console.log(x); //'x'
	}
	console.log(x); // 'x'
	console.log(y); // ReferenceError: y is not defined
}
```

##### Nested scope Example 2
```javascript
var landscape = function() {
    var result = "";
    
    var flat = function(size) {
        for (var count = 0; count < size; count++)
            result += " _ ";
    };
    
    var mountain = function(size) {
        result += "/";
        for (var count = 0; count < size; count++)
            result += " '";
        result += "\\";
    };
    
    flat(3);
    mountain(4);
    flat(6);
    mountain(1);
    flat(1);
    return result;
};

console.log(landscape());
```

- `flat` and `mountain` are functions only available within `landscape`
- The `result` variable is available inside `flat` and `mountain` because

### Variable name conflicts
Sometimes parent scopes have variables with the same name.

```javascript
var name = 'Shane';
var greet = function() {
	var name = 'Matt'
	console.log(name);
}
console.log(name); // Shane
console.log(greet(name)); // Matt
```

Declaring the variable `name` in the inner scope hides the variable with the same name in the outer scope. This is a new variable, so assigning to the inner variable won't affect the outer. 

Sometimes a function defines argument names with the same name as variables in the parent scope. This works the same way as the example above.

```javascript
var name = 'Shane';
var greet = function(name) {
	console.log(name);
}
greet('joe'); // 'joe'

//equivalant to 

var name = 'Shane';
var greet = function(mySuperUniqueVariableName) {
	var name = mySuperUniqueVariableName;
	console.log(name);
}
greet('joe'); // 'joe'


```

## Exercise 3 (Scope & Closures)

Start with the following code template. After each step, run the program and see how the output changes.

```javascript
function outer(){
  
  function inner() {
   
  }

  inner();
}

outer();
```

1. Declare two variables, `a` and `b` in the outer function's scope and set them to a string and an object respectively. Log their values immediately.
2. Log the values of `a` and `b` in the inner function.
3. Update the inner function to take two parameters named `a` and `b`.
4. Pass `a` and `b` in as arguments when you execute inner().
5. Inside the inner function, assign new values to `a` and `b` and log them at the end of the function AND after the execution of `inner(a,b)`.
6. Inside the inner function, update a property of the `b` object.


## Homework

*Due on Thursday:*​
- Complete the Rock Paper Scissors challenge (with your partner)
- Push it to our class repo (you may want to go ahead and push it, so you can both work on it). I’ll list instructions below
- Read through JS for Cats and push your answers to our class repo (remember to do the exercises in your terminal, not the browser)

## Reading
- [http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)
