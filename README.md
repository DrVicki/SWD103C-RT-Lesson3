# SWD103C-RT-Lesson3

# Components  

React applications are built from components. We can see our root component defined in the `App.js` file. It’s used inside the `index.js` file like a HTML tag. A component must have only one parent level holder of the HTML code. There are two types of components:     

  - **Class component**: is defined using the class syntax of JavaScript ES6     
  - **Function component**: is defined using the function syntax of JavaScript ES5

## 3.1  Class Component 

Let’s change the `App component` to be a `class` component and try some React features. 

### 3.1.1   Local State 

Local state is the information only the owner component could read and modify. We initialize the `state` in `constructor` of the class component. The local `state` in a class component is `this.state`. We use the `this.setState()` function to change the state. 

In a constructor function, the `super()` method calls the constructor of the base class. 

Example: 

/src/App.js 

```
import { Component } from 'react'; 
import './App.css'; 

class App extends Component { 

  constructor() { 
    super(); 
    this.state = { name: 'Neo' } } 
  
    render() { 
  
    return ( 
  <div className="App"> 
    <p>Hello {this.state.name}</p> 
      <button onClick={() => { 
      this.setState({ name: "John" }) }}>Change name</button> 
  
  </div> ); 
  } 
  } 
  
  export default App; 
  ```
  
  
We can only change state of a component using the `setState()` function. Using `this.state.name = "John"` will not work. 

### 3.1.2   Update A Portion of The State 

Since the state is an object, we can change a portion of it and React will do merging changes for us as the below example. 

Example: 

/src/App.js 

```
import { Component } from 'react'; 
import './App.css'; 

class App extends Component { constructor() { 

  super(); 
  this.state = { firstName: 'Vicki', lastName: 'D. Bealman' } } 
  
  render() { 
  
  return ( <div className="App"> 
    <p>Hello {this.state.firstName} {this.state.lastName}</p> 
    
  <button onClick={() => { 
    this.setState({ lastName: "Bealman" }); 
  
  }}>Change name</button> 
  
  </div> ); 
  } 
  } 
  
  export default App; 
  
  ```
 ### 3.1.3 Access The State Right After Changing 
 
The `setState()` function has two parameters. The second param is a callback function that will be called after changing the state. Therefore, if we want to access the state right after changing it, we can put our code inside the callback. 

Example: 

/src/App.js 

```
import { Component } from 'react'; 
import './App.css'; 

class App extends Component { 

  constructor() { super(); 
  this.state = { firstName: 'Vicki', lastName: 'D. Bealman' } } 
  
render() 
{ return ( 

  <div className="App"> 
  <p>Hello {this.state.firstName} 
  {this.state.lastName}</p> 
  
  <button onClick={() => { 
    this.setState( { lastName: "Bealman" }, () => { 
    
    console.log(this.state); 
    }) 
    }}>Change name</button> 
    
 </div> ); 
 } 
 } 
 
 export default App;  
 ```
 
Put `console.log(this.state);` right after `this.setState()` will not work since `this.setState()` is an asynchronous function so that the `console.log()` will be invoked before changing the state. 

Example: 

```
<button onClick={() => { 
this.setState({ lastName: "Bealman" }); 

  console.log(this.state); // not work correctly 
  
}}>Change name</button> 

```
As we know `this.setState()` is an asynchronous function. We could use async-await to archive the same result with passing a callback function. 

Example: 

```
<button onClick={async () => { 
  await this.setState({ lastName: "Bealman" }); 
  
  console.log(this.state); 
  
}}>Change name</button> 
```

### 3.1.4   Unique Keys for List Items 

While manipulating a list of items, each child in the list should have a unique “`key`” property so that React can manage and update the items. 

Example: 

/src/App.js 

```
import { Component } from 'react'; 
import './App.css'; 

  class App extends Component { constructor() { super(); 
  `this.state = { posts: [ { id: '1', title: 'Post 1' }, { id: '2', title: 'Post 2' }, { id: '3', title: 'Post 3' } ] } } 
  render() { 
  return ( <div className="App"> {this.state.posts.map(post => <h1 key={post.id}>{post.title}</h1>)} </div> ); 
  } } 
  
  export default App;
  ```
  

### 3.1.5 Component Lifecycle 

Each component has several lifecycle methods that you can override to run code at particular times in the process. Below is the lifecycle diagram of a component.

![](https://github.com/DrVicki/SWD103C-RT-Lesson3/blob/main/images-lesson3/Screen%20Shot%202022-10-31%20at%206.01.02%20PM.png)

These methods are called in the following order when an instance of a component is being created and inserted into the DOM:     

  - `constructor()`: to initialize local state of the component or bind event handler methods to an instance.    
  - `render()`: this method is called whenever the local state is changed or the props property of the component is changed.     
  - `componentDidMount()`: this method is invoked when the component completely added into the `DOM`. Therefore, it is a good place to instantiate the network request (loading data from a remote endpoint) or to set up any subscriptions.
  
An update can be caused by changes to props or state. These methods are called in the following order when a component is being re-rendered:     

  - `render()`: renders the UI based on current state of the component.     
  - `componentDidUpdate()`: is invoked when finished updating `DOM`.
  
3.1.5.1   Component Lifecycle in Action 

Let’s try the below example to understand clearly about the React component lifecycle.  In this example, please comment out the `React.StrictMode` code in the `index.js` file to get accurate logs on the console. When you're done debugging, be sure to uncomment it! 

We’ll create a dummy data file for temporary use. 

/public/posts.json 

```
[ { "slug": "getting-started-html", "title": "Getting Started with HTML", "image": "html.jpg", "excerpt": "HTML is the standard markup language for documents designed to be displayed in a web browser.", 

"date": "2022-01-10", "isFeatured": false }, { "slug": "getting-started-css", "title": "Getting Started with CSS", "image": "css.jpg", "excerpt": "CSS is a style sheet language used for describing the presentation of a HTML document.", 
  
"date": "2022-02-05", "isFeatured": false }, { "slug": "mastering-javascript", "title": "Mastering JavaScript", "image": "js.jpg", "excerpt": "JavaScript is the most used programming language for web development.", 

"date": "2022-03-30", "isFeatured": true }, { "slug": "mastering-react", "title": "Mastering React", "image": "react.jpg", "excerpt": "React is a a JavaScript library for building user interfaces.", 

"date": "2022-05-16", "isFeatured": true } ] 
```

Prepare the images for the above posts. Please make the new “`images`” folder as a child of the “`public`” folder. You can download the “`images`” folder from my GitHub repository or make it yourself by following the below screenshot.








