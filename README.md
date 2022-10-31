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
As we know `this.setState()` is an asynchronous function, we could use async-await to archive the same result with passing a callback function. 

Example: 

```
<button onClick={async () => { 
  await this.setState({ lastName: "Bealman" }); 
  
  console.log(this.state); 
  
}}>Change name</button> 
```

### 3.1.4   Unique Keys for List Items While manipulating a list of items, each child in the list should have a unique “key” property so that React can manage and update the items. Example: /src/App.js import { Component } from 'react'; import './App.css'; class App extends Component { constructor() { super(); this.state = { posts: [ { id: '1', title: 'Post 1' }, { id: '2', title: 'Post 2' }, { id: '3', title: 'Post 3' } ] } } render() { return ( <div className="App"> {this.state.posts.map(post => <h1 key={post.id}>{post.title}</h1>)} </div> ); } } export default App;

D. Truman, Neo. Web Development Crash Course - React JS Application Development: Build Scalable Websites with React, Redux JS, and Firebase - Create Your Own Website The Easy Way With Node React, Road To Learn React (pp. 49-54). Kindle Edition. 
