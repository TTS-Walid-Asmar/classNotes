# Lesson 10 - REST APIs & Promises

## Recap & Intro
- Last time we talked about jQuery and DOM manipulation
- So far, we've been on an island on the page. It's time to leave the island and communicate with the server.
- Data API's let JS get/set data to a server

## XHR
JavaScript has built in support for creating HTTP requests using the XML HTTP Request object (XHR) also known as AJAX.

- Requests are just like those your browser makes when navigating
	- URL - location of the resource
	- Header - Metadata about the request (e.g. type, expected response, cookies...)
	- Body	- Payload (e.g. post data)
- Response is text. API responses are usually JSON.
- Imagine navigating to a webpage, but instead of getting HTML, you get JSON text.
- Native API is low level, so libraries like jQuery are used.

Using raw XHR is pretty verbose, so we'll use jQuery for making requests for now. Below is a simple 'GET' request using jQuery.

```javascript
$.get('http://jsonplaceholder.typicode.com/posts', function(posts){
	console.log(posts);
})
```
Notice the familiar callback pattern.

## REST API Principles
REST is a pattern for organizing API's, with a few simple rules
	- Uses HTTP request types as VERBS
	- Uses URL's to represent unique resources (NOUNS)

## Requests
Request types make up the verbs in a REST API.

	1. GET - Requests data, parameters in querystring
	2. POST - Submits data to be processed, parameters in body
	3. PUT - Adds item at a specific location
	4. DELETE - Deletes an item at a specific location
	5. PATCH - Like put, but you can send a partial object to update a few fields

jQuery provides `$.get()` and `$.post()` methods for convenience and `$.ajax()` for general purpose.

#### Restful
- GET '/items'
- GET '/items?type=3'
- POST '/items/3'

#### NOT Restful

- POST '/items/new'
- GET '/items/add'
- DELETE '/items/removeItem/1'

### GET Requests
Get all students

```javascript
$.get('/students', {lastName: 'jones'}, function(results){
		//do something with results
	})

```

### POST Requests
Update last name of student with ID=3 to 'Smith'

```javascript
$.post('/student/3', {lastName: 'Smith'}), function(response){
		//Handle response, which usually contains the updated object including new ID's

	})
```

### PUT and DELETE
These are idempotent. You can put something 100 times and you'll only get one object saved.

```javascript
//Replace the object at '/student/3'

$.ajax({
	method: 'PUT',
	url: '/class/student/3',
	data: {firstName: 'Cam', lastName: 'Newton'},
	complete: function(response){
		//handle response which usually includes the updated object.
	}
})

//Remove the object at '/student/3'
var newObject = $.ajax({
	method: 'DELETE',
	url: '/class/student/3',
	complete: function(response){
		//handle response
	}
})

```

## Rendering data from an API
Rendering data returned by an API is pretty straight forward because the response is already a JS object or array.

Lets say you want to list the users...

```javascript
//Get reference to the list element
var list = $('ul#userList');

//Make a GET request for the items to render
$.get('http://jsonplaceholder.typicode.com/users', function(users){
	//Iterate over the response, adding elements to DOM
	users.forEach(function(user){
		var li = $('<li></li>');
		li.text(user.name);
		list.append(li);
	})
})
```

## Cross Origin Policy
By default, XHR requests are only allowed to the same domain the browser is currently on. Server has to enable CORS to allow browsers from other domains to make a successful request.

Our demo API has CORS support enabled, so you don't have to worry about it here. Just be aware of this.


## Exercise 1: REST
Use jQuery to do the following things with the http://jsonplaceholder.typicode.com/ API.

- Create buttons that do each of the tasks below
- Render the results to the page in html elements.
- Hide the results from the previous actions

1) Get all posts
2) Get post with id of 10
3) Get the comments from post with id of 12
4) Get all the posts from user with id of 2
5) Create a new post and log the id generated for it by the server
6) Replace the post with id of 12 and render the responseJSON
7) Update the title of post with id of 12 and render responseJSON
8) Delete the post with id of 12 and render a success message
9) Display a list of posts.
	- When the user clicks on a post, display all the comments from that post
	- Display a link back to all posts


## Exercise 2: Answers
Just showing the REST parts

```javascript
// 1) Get all posts
$.get('http://jsonplaceholder.typicode.com/posts', function(data){
	console.log(data);
});

// 2) Get post with id of 10
$.get('http://jsonplaceholder.typicode.com/post/10', function(data){
	console.log(data);
});

// 3) Get the comments from post with id of 12
$.get('http://jsonplaceholder.typicode.com/post/12/comments', function(data){
	console.log(data);
});

// 4) Get all the posts from user with id of 2
$.get('http://jsonplaceholder.typicode.com/user/2/posts', function(data){
	console.log(data);
});

// 5) Create a new post and log the id generated for it by the server
$.post('http://jsonplaceholder.typicode.com/posts',{
	userId: 1,
	title: "My New Post",
	body: "This is the body"
}, function(data){
	console.log(data.id)
})

// 6) Replace the post with id of 12
$.ajax({
	method: 'PUT',
	url: 'http://jsonplaceholder.typicode.com/posts/12',
	data: {
		userId: 1,
		title: "New Post",
		body: "New post added"
	},
	complete: function(response){
		console.log(response.responseJSON);
	}
})

// 7) Update the title field of the post with id of 12
$.ajax({
	method: 'PATCH',
	url: 'http://jsonplaceholder.typicode.com/posts/12',
	data: {
title: "patched it"
},
	complete: function(response){
		console.log(response.responseJSON);
	}
})

// 8) Delete the post with id of 12
$.ajax({
	method: 'DELETE',
	url: 'http://jsonplaceholder.typicode.com/posts/12',
	complete: function(response){
		console.log(response.statusText);
	}
})
```


## Problems with Callback Pattern
To this point, we've been using the **'Callback Pattern'**, which works great for simple cases, but breaks down in real world situations

### Callback Hell
Often times you want to perform multiple operations. For example, lets say you want all the posts and all the users.

```javascript
$.get('http://jsonplaceholder.typicode.com/posts', function(posts){
	$.get('http://jsonplaceholder.typicode.com/users', function(users)){
		//Do something with posts and users
	}
});
```
There are a couple of problems with following this pattern.

1. Its hard to follow
2. It executes in serial

### Messy error handling
With Callbacks, errors are typically handled by a second error handling function. E.g.

```javascript
somethingAsync(successFn, errFn);
```

Combine this with real world callback hell, and you have a real mess.

```javascript
var loggedInUsername = 'Bret';
$.get('http://jsonplaceholder.typicode.com/users?username=' + loggedInUsername, function(users){
	$.get('http://jsonplaceholder.typicode.com/posts?userId=' + users[0].id, function(posts)){
		//Do something with posts
	},
	function(err){ //handle error from posts call

	}
}, function(err){ //handle error from users call.

});
```

Besides being butt ugly, it prevents you from handling an error and executing the rest of the async operations. Imagine you had an error finding a user with that username, but wanted to show posts from a default user Id.

## Promises

- A promise is an object that represents an *eventual* value.
- Promises allow async code to behave synchronously
- There are many libraries that create promises, but they are built in to ES6

### Creating a Promise

```javascript
var p = new Promise(function(resolve,reject){
	//do something async...and get a value
	var myAsyncValue = 'abc'
	resolve(myAsyncValue);
});

p.then(function(data){
	//console.log(data);
})
```

Ok, so what's the big deal, aren't these just fancy callbacks?? Actually, Promises offer a number of benefits:

### Chaining
Promises can easily be chained to order multiple dependent asyc operations

```javascript
//------------ dataService.js ------------
function getUserByName(username) {
return new Promise(function(resolve,reject){
	 $.get('http://jsonplaceholder.typicode.com/users?username=' + username, function(users){
		 resolve(users[0]);
	 });
 })
}

function getPostsByUser(user) {
return new Promise(function(resolve,reject){
	 $.get('http://jsonplaceholder.typicode.com/posts?userId=' + user.id, function(posts){
		 resolve(posts);
	 });
})
}

//------------ interface.js ------------

function render(posts) {
//render the posts data
console.log(posts);
}

function showUserItems(){
getUserByName('Bret')
 .then(getPostsByUser)
 .then(render);
}

showUserItems();
```

### Error handling
Using .catch(), you can handle errors in order

```javascript

function getUserByName(username) {
	return new Promise(function(resolve,reject){
		 $.get('http://jsonplaceholder.typicode.com/users?username=' + username, function(users){
			 if(users.length)
			 	resolve(users[0]);
			else
				reject('User with name ' + username + 'not found');
		 });
	})
}

function handleUsernameError(err){
		console.warn(err, 'Using id=1 instead');
		return {
			username: "Sample User",
			id: 1
		};
}

function showUserItems(){
	getUserByName('Bret')
		.catch(handleUsernameError)
		.then(getPostsByUser)
		.then(render);
}
```

Once an error is thrown All `then` functions are skipped until a `catch` handles the error;

### Parallel execution
Promises let you perform a bunch of operations and do something when they've all completed. Lets say you want all the users and all the posts before rendering

```javascript
function getAllUsers() {
	return new Promise(function(resolve,reject){
			$.get('http://jsonplaceholder.typicode.com/users', function(users){
				resolve(users);
			})
	})

}

function getAllPosts() {
	return new Promise(function(resolve,reject){
			$.get('http://jsonplaceholder.typicode.com/posts', function(posts){
				resolve(posts);
			})
	})
}

var usersPromise = getAllUsers();
var postsPromise = getAllPosts();

Promise.all([userPromise, postsPromise])
	.then(function(results){
		var users = results[0];
		var posts = results[1];

		//Do something with users and posts
})

```

As you'd imagine, you can combine parallel and serial requests in a single promise chain.

```javascript
Promise.all([userPromise, postsPromise]) //parallel
	.then(function(results){ //serial
		var users = results[0];
		var posts = results[1];

		var p3 = somethingAsync(users);
		var p4 = somethingAsync2(posts);
		return Promise.All([p3,p4]); //parallel
})
	.then(/*...*/) //serial
```


### Instantly resolving promises
Sometimes you want a function that returns a promise object, but you already know the result. This is often done to "mock" API's.

```javascript
function getAllUsers() {
	return new Promise(function(resolve,reject){
		resolve(['joe','lisa','shane']);
	})

	//or

	return Promise.resolve(['joe','lisa','shane']);
}

```
Even if a promise is resolved before a `.then()` is attached to it, the `then()` function is guaranteed to run.



## Exercises 2 & Homework: Promises
Using the API at http://jsonplaceholder.typicode.com/ create an app with the following functionality. Use Promises for everything.

- Complete the [promise it won't hurt](https://github.com/stevekane/promise-it-wont-hurt) nodeschool tutorial and send a screenshot to Shane

1. **Login page** - Display a login form asking user for a username.
	- On Submit, look up the username entered by the user.
	- If no user is found, display a detailed error message.
	- If the user is found, store user object and render User's homepage
2. **User's homepage** displays:
	- User's name
	- User's post titles
	- User's albums
3. **Post View** - Clicking on a post renders a post view, including:
	- Post title
	- Post text
	- A list of comments on the post
	- A back link to the user homepage
4. **Album View** - Clicking on an album renders the album page, including:
	- Thumbnails of all the photos and the photo's title
	- A search box that filters the photos by title as the user types
	- A back link to the user homepage

- [http://code.tutsplus.com/tutorials/a-beginners-guide-to-http-and-rest--net-16340](http://code.tutsplus.com/tutorials/a-beginners-guide-to-http-and-rest--net-16340)
- [https://learn.jquery.com/code-organization/deferreds/examples/](https://learn.jquery.com/code-organization/deferreds/examples/)
- [http://restcookbook.com/](http://restcookbook.com/)
- [http://www.datchley.name/asynchronous-in-the-browser/](http://www.datchley.name/asynchronous-in-the-browser/)
- [http://www.datchley.name/es6-promises/](http://www.datchley.name/es6-promises/)
- [http://www.datchley.name/promise-patterns-anti-patterns/](http://www.datchley.name/promise-patterns-anti-patterns/)
- [http://www.html5rocks.com/en/tutorials/es6/promises/](http://www.html5rocks.com/en/tutorials/es6/promises/)
