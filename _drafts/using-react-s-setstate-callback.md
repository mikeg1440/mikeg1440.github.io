---
layout: post
title: Using React\'s setState callback
---

<div class='text-center'>
  <img src='https://daqxzxzy8xq3u.cloudfront.net/wp-content/uploads/2020/01/20161948/react-setstate-usestate-async.jpg' style='width: 50rem;' alt='setState header with react logo above' />
</div>

When updating state in a component you may end up in a situation where your using `setState` to store the data but need to execute some block of code only after the state is updated and not before.  This can you send you into a infinite loop of sending requests, updating the state, reloading the component because of the state change and then starting the whole process over again or unwanted behavior like needing to perform the same action twice to get a desired result.  Timing can be everything in certain situations and this is why React's `setState` method has a convenient callback option built into it.

You have the option of adding a callback to any `setState` call by passing in a second argument to the function.  This is a example of a class component does not use the callback.  What happens is that unwanted behavior that I was talking about, can you guess what would happen when you entered in a age of 26 for the first time?

```
import React, { Component } from 'react';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      age: 0,
    };
  }

  updateAge = (value) => {
    this.setState({ age: value});
    this.checkAge;
  };

  checkAge = () => {
    const { age } = this.state;
    if (age !== 0 && age >= 21) {
      // Make API call
    } else {
      // Show message that your not old enough yet kiddo
    }
  };

  render() {
    const { age } = this.state;
    return (
      <div>
        <p>Beer Suggestor</p>
        <input
          type="number"
          value={age}
          onChange={e => this.updateAge(e.target.value)}
        />
      </div>
    );
  }

}

export default App;
```

This is a simple example where you are setting a new state then running a method to check the new state and make a api request depending on the state.  If you guessed that the API call would run when you put in a age of 26 the first time then you would be wrong.  This is because the `setState` method is asynchronous and it has not finished running by the time the `checkAge` function is checking the state so the first time you change the age it will see the value of `this.state.age` as equal to 0.  If you changed the age a second time it would then see the previously input value of 26 and not the second age input which should be set as the newly set state.  This is where the callback functionality comes in handy.  If you where to change the code like the example below then you would get the desired result where when you change the age value the first time it would make the API request (given the age is over 21);

```
import React, { Component } from 'react';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      age: 0,
    };
  }

  // this.checkAge is passed as the callback to setState
  updateAge = (value) => {
    this.setState({ age: value}, this.checkAge);
  };

  checkAge = () => {
    const { age } = this.state;
    if (age !== 0 && age >= 21) {
      // Make API call to /beer
    } else {
      // Show message that your not old enough yet kiddo
    }
  };

  render() {
    const { age } = this.state;
    return (
      <div>
        <p>Drinking Age Checker</p>
        <input
          type="number"
          value={age}
          onChange={e => this.updateAge(e.target.value)}
        />
      </div>
    );
  }

}

export default App;
```

This is only a simple example of one use case for the setState callback functionality, I have set up a codesandbox.io with code that makes a request to a news API and has a search component that will make an additional request to the API using a controlled component so it uses the callback to make the subsequent request after the search term is submitted.  Feel free to check it out [here](https://codesandbox.io/s/naughty-chandrasekhar-ctkfg?file=/src/App.js) and play around with the code.  I have included a API key you can use so you don't have to set up your own account.

** NOTE: For some reason the request to API is not working on codesandbox.io, possibly as some sort of security precaution but if you run the code locally on your system it should work just fine **

<iframe src='https://codesandbox.io/s/naughty-chandrasekhar-ctkfg?file=/src/App.js' width='100%' height='100%'></iframe>
