---
layout: post
title: JavaScript Closures
---

<div class='text-center'>
  <img src='https://i.imgur.com/VwfKlqu.png' alt='Example of a closure and its scope' />
</div>

Closures are very useful if you are a JavaScript developer and it definitely is something you should be aware of as you advance in your developer career.  It's sometimes misunderstood and ignored because it may seem confusing at first when learning the language early on so some people ignore it even though it seems to be a pretty common interview question used to determine a Frontend coders understanding of the language.  In this post we are going to take a look at what closures are and why you may want to use them.

## What is a Closure?

To understand closures you first have to understand scope.  Scope is the what determines the accessibility of variables in code and in JavaScript there can be function scopes or block scopes.  

A closure is essentially a lexically scoped name binding that carries with it the all of the variables it depends on.  Essentially it is just a function within a function, and so along with access to the global scope you have access to variables within its own scope(from the inner function), as well as the variables from the outer enclosing function.  This may seem alien to some so I'll it explain it further with some examples.  

```
function add(){
  let num1 = 50;

  return (num2) => num1 + num2
}

let example = add();

console.log(example(10));
```
In this example we have a simple function defined in the return statement using ES6 shorthand, the function `add` takes no arguments, sets the variable `num1` to 50 and then returns a function `(num2) => num1 + num2` which accepts a argument called `num2` and adds this to the `num1` value.

When we initialize the `example` variable here it actually returns the function `(num2) => num1 + num2` and if we wanted to verify this we could run `typeof example` and it would return `function`.  Along with the function that is returned is also the chain of variables that accompany the scope of the function that returned the closure function so we have access to the `num1` variable so when we call `example(10)` and it adds 10 to 50 and returns 60.

If you know a little bit about JavaScript then you may be aware that variables are created and stored in memory when a function is invoked and destroyed when that function finishes.  So you may be asking how is it that one function can return another function and have the inner function use a variable set in the outer function?  JavaScript actually implements a scoping mechanism called lexical scoping (or static scoping) meaning that the accesibility of variables is determined by their position within nested functions, and when you return another function it actually is returned with that store of information which allows you to access that `num1` variable in the example given.

Before ES6 this was hugely helpful for programming in a Object Oriented manner with JS and because there where no classes yet you could kind of get around it by creating something like this.

```
function person(){
  let first,
    last,
    age;

  const setFirstName = (name) => first = name;

  const setLastName = (name) => last = name;

  const setAge = (num) => age = num;

  const getName = () => `${first} ${last}`;

  const getAge = () => age;

  const displayInfo = () => console.log(`${first} ${last} is ${age} years old!`)

  return {
    setFirstName,
    setLastName,
    setAge,
    getName,
    getAge,
    displayInfo
  }
}
```  

This way you had a person method that acts similar to a class and you could create multiple instances of it with variables such as `first, last` and `age` which are enclosed in the outer function so you can only set and display them with the specific functions returned from calling `person()`.  Since there is no way to make a variable private in JS this also helped keep some variables enclosed so you couldn't directly alter them, instead you had to use the defined functions like `setFirstName` to set the `first` variable of the `person` object that was created.

Of course ever since ES6 you can now do the same thing with a class like this.

```
class person{
  constructor(){
    let first,
      last,
      age,
      height;
  }

  setFirstName = (name) => this.first = name;

  setLastName = (name) => this.last = name;

  setAge = (num) => this.age = num;

  getName = () => `${this.first} ${this.last}`;

  getAge = () => this.age;

  displayInfo = () => console.log(`${this.first} ${this.last} is ${this.age} years old!`);
}
```

So in summary a closure is a function defined within a function that has access to the outer variables, even when the inner function is executed outside of the scope of the outer function.  This happens because of lexical scoping and the inner function is a closure because it closes over the variables of the outer function. A closure is just a function that captures the variables from the lexical scope, no matter where it is executed.
