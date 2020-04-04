---
layout: post
title: Introduction to React Hooks
date: 2020-04-03 23:04 -0400
---
![React Hooks Visual](https://res.cloudinary.com/practicaldev/image/fetch/s--8ZFuZgNR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/2v9z9mh5dhxm311vxwkq.jpg)

## What are Hooks?

React has become a popular choice for front end development over the past few years and is constantly evolving to increase stability, security and usability.  The framework has been around for about 7 years ago now so it's still being tweaked and expanded, and the release of [version 12.8](https://reactjs.org/blog/2019/02/06/react-v16.8.0.html) in 2019 came with the addition of [Hooks](https://reactjs.org/docs/hooks-intro.html).  Hooks are just functions that allow [Functional Components](https://www.robinwieruch.de/react-function-component) to access to "hook" into React state and lifecycle features that were only available to [Class Components](https://dev.to/folajomi__/building-react-components-ii-class-components-94p) previously.  

## List of Hooks

This is a list of the Hooks that are now available for use with React applications.

#### Basic Hooks

  - [useState](https://reactjs.org/docs/hooks-reference.html#usestate)
  - [useEffect](https://reactjs.org/docs/hooks-reference.html#useeffect)
  - [useContext](https://reactjs.org/docs/hooks-reference.html#usecontext)

#### Additional Hooks

  - [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer )
  - [useCallback](https://reactjs.org/docs/hooks-reference.html#usecallback)
  - [useMemo](https://reactjs.org/docs/hooks-reference.html#usememo)
  - [useRef](https://reactjs.org/docs/hooks-reference.html#useref)
  - [useImperativeHandle](https://reactjs.org/docs/hooks-reference.html#useimperativehandle)
  - [useLayoutEffect](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)
  - [useDebugValue](https://reactjs.org/docs/hooks-reference.html#usedebugvalue)


## Why Hooks?

React at it's core is supposed to allow for a more stable application, facilitate re-usability and simplify creating complex applications by deconstructing them into smaller, easily readable components.  In light of the last reason the developers of React decided that they needed to add hooks to give people the ability to use the same features that Class Components enjoyed without needing to convert components to Classes.  This is all in the mindset of making components a bit easier to work with, before if you had created a Functional component that you ended up needing to use state with down the road you would have to refactor it into a Class Component to get access to state.  This added some more complexity to jugle when developing components and it could cause some issues when needing to refactor complex applications in this way.  Now you can just add state to any Functional Component and simplify others components that where created as classes just use a single one of the features that hooks 'hook' into.

## Class Vs Functional with Hooks

Lets look at two components that do exactly the same thing but one is a Class Component and the other is a Functional Component using Hooks.  This is a very simple app that just taps into a API of dog images and displays an adjustable count that controls the number of dog images displayed on the page.  

#### Class Component
```
import React from "react";
import axios from "axios";
import "./styles.css";

export default class App extends React.Component {
  constructor() {
    super();
    this.state = {
      dogs: [],
      count: 1
    };
  }

  getDogs = () => {
    axios(`https://dog.ceo/api/breeds/image/random`).then(resp =>
      this.setState({
        dogs: [...this.state.dogs, resp.data.message]
      })
    );
  };

  componentDidMount() {
    this.getDogs();
  }

  componentDidUpdate(prevProps, prevState) {
    if (prevState.count !== this.state.count) {
      this.getDogs();
    }
  }

  render() {
    const { dogs, count } = this.state;
    return (
      <div className="App">
        <h2>Class Example</h2>
        <h3>Dogs Count: {count}</h3>
        <label>Use arrows to change number of dogs</label>
        <br />
        <button onClick={() => this.setState({ count: count + 1 })}>↑</button>
        <button onClick={() => this.setState({ count: count - 1 })}>↓</button>

        <div class="dogs-container">
          {dogs.map((dog, idx) =>
            idx + 1 <= count ? <img key={idx} src={dog} alt="A dog" /> : null
          )}
        </div>
      </div>
    );
  }
}
```
[Play with this code in codesandbox.io](https://codesandbox.io/s/class-example-pxby8)
<iframe
  src="https://codesandbox.io/embed/class-example-pxby8"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>


#### Functional Component with Hooks
```
import React, { useState, useEffect } from "react";
import axios from "axios";
import "./styles.css";

export default function App() {
  const [count, setCount] = useState(1);
  const [dogs, setDogs] = useState([]);

  useEffect(() => {
    if (dogs.length !== count) {
      getDogs();
    }
  });

  const getDogs = () => {
    axios("https://dog.ceo/api/breeds/image/random").then(resp =>
      setDogs([...dogs, resp.data.message])
    );
  };

  return (
    <div className="App">
      <h2>Hooks Example</h2>
      <h3>Dogs Count: {count}</h3>
      <label>Use arrows to change number of dogs</label>
      <br />
      <button onClick={() => setCount(count + 1)}>↑</button>
      <button onClick={() => setCount(count - 1)}>↓ </button>

      <div class="dogs-container">
        {dogs.map((dog, idx) =>
          idx + 1 <= count ? <img src={dog} alt="A dog" /> : null
        )}
      </div>
    </div>
  );
}
```
[Play with this code on codesandbox.io](https://codesandbox.io/s/hooks-example-2g6p3)

<iframe
  src="https://codesandbox.io/embed/hooks-example-2g6p3"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
  sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

As you can see the Class Component is a bit more congested with code than than the Functional Component making the Functional Component doing the same exact thing easier to understand and read.  There are two Hooks used in the Functional Component, the first is `useState` to give us access to component state and the second is `useEffect` which allows to do the same thing that `componentDidMount` and `componentDidUpdate` did for the Class Component.  Things are implemented slightly different with the Functional Component but in my opinion it's more straight forward.

## Wrapping up

To sum things up Hooks are not totally necessary when developing with React, you will find people who will argue each way that why you should or shouldn't use Hooks and you can go about developing without ever using them, but they seem to be showing up in more and more applications these days.

<div class='text-center'>
  <img class='' src='https://i.ytimg.com/vi/-9MvABBFdlw/hqdefault.jpg' alt='Classes are the past and Hooks are the future' />
</div>


I don't totally agree with the image above but as with most opinions on coding your going to find all sorts of differing views on the best way to do almost everything.  I personally lean more towards the side of you should be aware of them and know how to use them because it's just one more tool at your disposal when coding but some other reasons I have seen are the following.

 - Code is simplified and therefore easier to read and understand
 - Re-using stateful logic is easier without changing component hierarchy  
 - Hooks allow you to split up components that would otherwise be more complex
 - Less concepts to deal with when coding (like binding, correctly using `this`, etc)

There are allot of use cases Hooks and this is just a general introduction to them so you should do your own research and come to your own opinions as what works best.  Learn something new, try to refactor a old project to use Hooks or refactor one that uses Redux to instead use `useReducer`, in the end it's only going to make you a better coder.
