---
layout: post
title: A look at React Components
date: 2020-02-16 00:00 +0000
---
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">

<div class='text-center mb-5'>
  <img class='img-fluid' src='https://miro.medium.com/max/700/1*dLaDL-lSN0iprzmOpmM7zQ.png' />
</div>


React.js is one of many JavaScript libraries that build upon the vanilla JavaScript base, each of which offer their own pros and cons.  The React library is mainly used for building user interfaces and enables the use of module like pieces of code called "Components".  Each component reflects a piece of the interface and can be abstracted easily so that one component can be reused in multiple different different ways.  This boosts productivity and facilitates maintenance and expansion of code.  Each component contains its own internal logic which has the benefit of creating more stable applications that prone to less issues that cause developers headaches when building and maintaining applications.  

<div class='text-center'>
  <img class='img-fluid' src='https://miro.medium.com/max/602/1*idBf1a4LinjE2myJSEcpSQ.png' />
</div>

[Jordan Walke](https://github.com/jordwalke) is the creator of React.js and was working as a software engineer at Facebook in 2011 when built the initial library that would evolve into React.js and was shortly after implemented on the companies site as the news feed on Facebook pages.  His original creation called [FaxJS](https://github.com/jordwalke/FaxJs) was meant to be performant, structural and reactive, and shines best when used with data that is constantly changing or updating.  After a few years React was born and made open source by Facebook and today has grown far beyond what it originally was back then, maintained and extended by thousands of developers worldwide.  Many major companies that you may use on a daily basis use the library for the products, companies like Twitter, Pintrest, Instagram, Uber, Netflix and many others and is recognized for it's performance and stability.  


<div class='text-center'>
  <img class='img-fluid' src='https://miro.medium.com/max/2416/1*7Ai2KRFAL6Yj6WaKaFOyEw.png' />
</div>

React has its own type of syntax that is like is XML/HTML like and extends the [ECMAscript](https://en.wikipedia.org/wiki/ECMAScript) specification so that it's easier to implement JavaScript and HTML all together, except instead of putting JavaScript into HTML your putting HTML into JavaScript.  

Now that you know a little bit about what React.js is and how it came about lets look at the basic building blocks of the library, components.  There are a few different types of components and we will look at, the two main ones are Class and Functional components.

## Class Components

Class Components are just what they sound like, a component that is a ES6 JavaScript class and are among the most used type.  This is because they have what makes the React.js library so well, reactive.  This is an example of a very simple Class Component called `Hello`.
```
export default class Hello extends React.Component {
    render() {
        return(<h1>Welcome to the World of React!</h1>)
    }
}
```
All this does is render a h1 header saying 'Welcome to the World of React!'.  The power of class components comes from the ability for them to use whats called state, basically the state of the component.  The state property allows us to make logic decisions and changes to the application based on its current state.  To use state within a Class Component we need to add a constructor to the class so its sets up a initial state when the component is rendered.  
```
export default class Hello extends React.Component {
  constructor(props){
    super(props)
    state = {
      message: 'Welcome to the World of React!'
    }
  }
  render() {
      return(<h1>{this.state.message}</h1>)
  }
}
```
Here we are setting the state object with a message key and the value of our message.  You can set the state to anything you like so the options are near limitless, but you should only set the state directly in the constructor method and when you need to update it React provides us with a function called `setState`.  Using the setState function we can update the components state and will trigger the component to re-render.

```
export default class Hello extends React.Component {
  constructor(props){
    super(props)
    state = {
      message: 'Welcome to the World of React!'
    }
  }
  render() {
    return(
      <div>
        <h1>{this.state.message}</h1>
        <button onClick={this.setState({message: 'Goodbye!'})}>Leave</button>
      </div>
    )
  }
}
```
This example would cause the component to re-render when the user clicks the button and the h1 header would update to say 'Goodbye!'.

### Life Cycle Methods

<div class='text-center'>
  <img class='img-fluid' src='https://pbs.twimg.com/media/DZ-97vzW4AAbcZj.jpg' />
</div>

[Life Cycle Methods](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) are another powerful feature of class components that allow you to trigger code on specific events during the life of a component.  There are [many uses](https://blog.bitsrc.io/react-16-lifecycle-methods-how-and-when-to-use-them-f4ad31fb2282) for each life cycle method that we won't cover hear but know that they are extremely handy when fully understood and used properly.  

## Functional Components

[Functional Components](https://programmingwithmosh.com/react/react-functional-components/) are the other main type of components and agian just like the name suggests they are functions.  This component takes in props as arguments and returns JSX, this is an example of what one looks like.
```
// Functional Component Example
import React from 'react';
function HelloWorld(){
   return (
      <div>
         <p>Hello World!</p>
      </div>
   )
}
export default HelloWorld;
```
With functional components you don't have access to life cycle methods, unless you implement [React Hooks](https://reactjs.org/docs/hooks-intro.html) which are a new addition to React in version 16.8, but that is outside the scope of this post.  These components don't have a render method like class components but return JSX just like class components.  We can use whats called props which essentially stands for properties to render data passed to the component.  
```
// Functional Component with Props Example
import React from 'react';
function HelloWorld(props) {
   return (
      <div>
         <p>{this.props.message}!</p>
      </div>
   )
}
export default HelloWorld;
```
This example uses props that are passed in from another component when its called.  For this example it would look like `<HelloWorld message='Welcome to the World of React' />` which would give the component access to a prop called `message` that can be used how ever you like.  

## Arrow Functions
With new coding concepts introduced in ES6 React also inherited some new components.  This example does the same thing as the previous functional component and takes in props the same way.
```
// Functional Component with Props Example
import React from 'react';
const HelloWorld = (props) => {
   return (
      <div>
         <p>{this.props.message}!</p>
      </div>
   )
}
export default HelloWorld;
```
This arrow function can be used without the brackets and just return JSX like this.
```
const HelloWorld = (props) => <h1>{this.props.message}</h1>
```

### Stateless Components
The last three examples are all what is called stateless components, just components that don't use state.  They just take in props from a parent component and render something.

## Pure Components
Pure Components are just like functional components but better for certain use cases like when they only render something passed into it and don't manipulate the data in anyway.  Just like pure functions in JavaScript they basically just return the same values that are passed in, having no observable side effects.  They are the simplest fastest components that React offers.  Pure Components perform whats called 'shallow comparison' on state change and if the state is the same as before then the component will not re-render, saving time and resources that would normally be needed to re-render the component making it slightly faster than a normal component.  Shallow comparison does not compare nested objects and should be used in only specific cases, if your try to use a Pure Component with complex objects there can be unwanted side effects that could cause the application to behave in unexpected ways.

Regular components re-render when one of these three things happens
 1. `setState` is called
 2. the props values are updated
 3. `forceUpdate()` is called
But pure components will only re-render if they detect a change in either the state or the props.  Meaning if the shallow comparison does not detect a change then the component will not re-render.  This makes them useful if you have a component that will be displaying data that isn't changing when a parent component re-renders.

## Higher-Order Components

The last component we will take a look at is called a [Higher-Order Component](https://reactjs.org/docs/higher-order-components.html) and is a more advanced technique in React that is helpful for reusing component logic.  Its basically a component that takes another component as a argument or prop and returns a new component.  
```
import React from 'react';
import { func1, func2 } from '../functions';
export function withFunctions(WrappedComponent) {
   return class extends React.Component {

      render() {
         const newProps = {
            func1: func1,
            func2: func2
         };

         return <WrappedComponent {...this.props} {...newProps} />
      }
   }
}
```
 This is a simple HOC that just wraps a component with functions we want to inject into the original component that are imported from other files.  It's a simple concept that can really expand all the different ways you can use existing components without needing to change them much if at all.  Some of the most widely adopted packages used in React are HOC like `react-redux` and it's `connect` statement.  Some others are `react-router-dom` and `react-cookies` and provide extremely useful tools expand upon components.  And you can wrap any number of HOC around a component.

 I hope this was some help to those who are new to the React.js library  and now that you have a basic understanding of the main types of components you can start building out your own applications.  If you want to see examples of components then you take a look at the site [Bit.dev](https://bit.dev/components) which has thousands of examples of reusable components that you can integrate or build upon in your own projects.  Another great site for learning and tinkering with React code is [CodeSandbox](https://codesandbox.io/s/new) where you can import whatever dependencies you need for free without even needing to create a account.
