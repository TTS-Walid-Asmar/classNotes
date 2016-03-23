# Lesson 18 - Firebase

## Recap & Intro
- We've learned a lot about React
- Last lesson, we learned how to get data into our React components
- Today we'll learn how to integrate a real-time data source
- We'll also learn how to deploy our application

## What is Firebase
Firebase is a back end as a service (BaaS) platform. It provides:

- Hosting for your static application
- Data read/write
- User Authentication 

## Hosting
Your React app is static, meaning that it doesn't have to run any code on the server. All a server needs to do is host your files and provide them to a client when requested. Firebase does this for you easily.


## Exercise 1: Deploying your app
1. Sign up for firebase using your Google account at [http://www.firebase.io](http://www.firebase.io)
2. Install Firebase tools using your commandline - `npm install -g firebase-tools`
3. Log in to your firebase account from commandline `firebase login`
4. From your React application directory, create a new Firebase app by running `firebase init` (tell firebase where your dist directory is)
5. Deploy using `firebase deploy`
6. See your app by running `firebase open`

## Data
A Firebase data store can be thought of simply as a JSON object in the cloud. When you create a Firebase application, a URL is created that represents the root node of the JSON object. To work with it, you need to create a "Ref"

```javascript
var ref = new Firebase("https://tts-demo.firebaseio.com/");
```


### Writing Simple Data

```javascript
var ref = new Firebase("https://tts-demo.firebaseio.com/");

ref.set({
  message: 'hello world'
});
```

If you go to https://tts-demo.firebaseio.com/, you'll see that the data's been saved.

The example above replaces the data completely. Instead, you can provide an object with a partial set of keys to update.

```javascript
var ref = new Firebase("https://tts-demo.firebaseio.com/");

ref.set({
  className: 'JS',
  weeks: 10
});

ref.update({
  className: 'JS Application Development',
  students: 15
});

/* Ref is now
{
  className: 'JS Applications',
  weeks: 10,
  students: 15
}
*/

```

### Reading Simple Data
Because Firebase is real-time, you don't request data in a discrete way like you do from a REST API. Instead, you listen for events on your data that signal when the data has been updated and run a callback when that happens. 

For example, we can run a callback function every time our JSON object is changed

```javascript
ref.on("value", function(snapshot) {
  console.log(snapshot.val());
})
```

**Note - Events are fired immediately for existing data.**

### Child References
Most times, you don't want to listen to the whole JSON object, just a specific subsection. You can create a reference to a portion of your JSON object by using the reference's `child()` function.

Let's say your data looks like this:

```javascript
{
	class: 'JS Applications',
	teacher: {
		name: 'Shane',
		computer: {
			type: 'mac',
			size: '15 inch'
		}
	}
}
```

You can read and listen for changes in just the `teacher` object by creating a child reference like this:

```javascript
var ref = new Firebase("https://tts-demo.firebaseio.com/");
var teacher = ref.child('teacher');
teacher.on('value', function(v){/*... */})
```

Child paths can be nested as deeply as you'd like.

```javascript
var ref = new Firebase("https://tts-demo.firebaseio.com/");
var ShanesComputer = ref.child('teacher/computer');
```


## Firebase with React
Let's add Firebase data to a React component. Let's start with a simple controlled component.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import classNames from 'classnames';

class MessageApp extends React.Component {
  constructor() {
    super();
    this.state = {
      inputValue: ''
    }
  }

  _handleChange(event) {
    this.setState({
      inputValue: event.target.value
    })
  }

  render() {
  
    return (
      <div>      
        <input ref="messageInput" value={this.state.inputValue} onChange={this._handleChange.bind(this)} />
      </div>
    )
  }
}

var mountPoint = document.querySelector("#app");
ReactDOM.render(<MessageApp />,mountPoint)
```

#### Install Firebase
Next, we'll bring in Firebase via npm `npm install --save firebase`. We can now import firebase just like all of our other dependencies

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import classNames from 'classnames';

//Import firebase!
import Firebase from 'firebase';

//...
```

#### Create a ref
For now, let's have our component create a firebase reference.

```javascript
  constructor() {
    super();
    
    this.state = {
      inputValue: ''
    }
    
    this.ref = new Firebase("https://tts-demo.firebaseio.com/");
  }
```

#### Get Data from Firebase
We can bring in data from Firebase in the `componentDidMount` event.

```javascript
componentDidMount(){

	//Arrow function so callback can use component's this.setState
	ref.on("value", (snapshot) => {
	  this.setState({
	  	inputValue: snapshot.val()
	  })
	})
}
```
This creates a 1-way data binding from firebase to the component. That is, every time Firebase is updated, the component's state will be updated.

#### Update data in Firebase
If we want updates to be saved to firebase, we'll have to update our reference object.

```javascript
_handleChange(event) {
	this.ref.update({inputValue: event.target.value})
}
```

Notice that this.setState() isn't necessary! This is because your callback in componentDidMount will set update the component state for you.

**Note - Your data is being updated in real time. Open another browser to the same local address and watch the magic happen**
 
## Exercise 2: Binding data
Let's create a theme preview tool with React

- Create a form with controlled text inputs for color and background
- Create a 'save' button
- Create a div (500px by 500px) with an h1 that reads 'This is a preview'
- When the 'save' button is clicked, save the two values currently in the inputs to a firebase reference
- Style the div to display the text color and background color stored in Firebase at all times
- Set up the inputs to contain the names of the text and background colors saved in firebase.


## Lists of data
One thing we haven't covered yet, is how to deal with arrays or lists with firebase. This is very similar to how we deal with other data, but with different events. We'll be using `ref.push(obj)` to write data and `child_added`, `child_removed` and `child_changed` events to read updated data


## Chat app
We'll start with our basic, one component, message app:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import classNames from 'classnames';

class MessageApp extends React.Component {
  constructor() {
    super();
    this.state = {
      newMessageInput: '',
      messages: []
    }
  }

  _handleButton(event) {
    var newMessage = this.refs.messageInput.value;
    this.setState({
      messages: this.state.messages.concat(newMessage)
    })
  }

  _handleChange(event) {
    this.setState({
      newMessageInput: event.target.value
    })
  }

  render() {
    var messages = this.state.messages.map((message, i)=>{
      return <li key={i}>{message}</li>
    })

    return (
      <div>
        <h1>Slick</h1>
        <input ref="messageInput" value={this.state.newMessageInput} onChange={this._handleChange.bind(this)} />
        <button onClick={this._handleButton.bind(this)}>Submit</button>
        <ul>
          {messages}
        </ul>
      </div>
    )
  }
}

var mountPoint = document.querySelector("#app");
ReactDOM.render(<MessageApp />,mountPoint)
```

We're going to want to read and write the state.messages array from Firebase following the same steps are before:

#### Import firebase 
`import Firebase from 'firebase';`

#### Create a reference to messages
```javascript
constructor() {
   super();
    this.state = {
      newMessageInput: '',
      messages: ['hello', 'world']
    }

  	this.messagesRef = new Firebase("https://tts-demo.firebaseio.com/messages");
}
```

#### Get data in componentDidMount using 'child_added' event

```javascript
componentDidMount(){
  	this.messagesRef.on("child_added", (snapshot) => {
  	  this.setState({
  	  	messages: this.state.messages.concat(snapshot.val())
  	  })
    })
  }
```

#### Push new messages in the _buttonHandle function

```javascript
_handleButton(event) {
	this.messagesRef.push(this.refs.messageInput.value)
}
```

## Exercise 3 + Homework
Write a persistant chat app with React + Firebase.

- Create a login page where the user enters their name
- Validate that the name is more than 1 character and show error otherwise
- Upon login show all previously written messages along with
	- Username
	- Message
	- Time
- Let users enter new messages and show them in real time
- Read about firebase Authentication - https://www.firebase.com/docs/web/guide/user-auth.html
- Implement user authentication using email and password
- Style the app to your heart's content
- Deploy your app to firebase and send us a link in slack

