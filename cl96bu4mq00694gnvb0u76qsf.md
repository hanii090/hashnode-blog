---
title: "Composing Closures and Callbacks In Javascript"
datePublished: Thu Oct 13 2022 00:32:45 GMT+0000 (Coordinated Universal Time)
cuid: cl96bu4mq00694gnvb0u76qsf
slug: composing-closures-and-callbacks-in-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1665698946434/IXmvlCY5f.png
tags: javascript, beginners, 2articles1week

---


- A closure is just a function that accesses things outside of it
  - It can capture anything outside of it and use that value each time the function is called

_questions:_

- shouldn't `i` increase to 1 when logging `i++`?

This is like a for loop, it wipes down the variable alignment.
# What is a callback in javascript
 

According to MDN: *"A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action."*

- A callback is when you pass a function as an argument to another function

 
![javascript-what-is-a-callback-in-javascript-callback (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665442481522/oSIUFt-re.png align="left")

**Callbacks:**
 
1. Function passed into another function as an argument
2. Invoked or "called-after" inside the outer function to complete an action/routine
3. Can be synchronous or asynchronous
 # Can a Function be a Closure and a Callback?
 

We can create a function that can be both

- A closure
- A callback

```javascript
let i = 0

let callback = (event) => {
  console.log(i++)
}

let anotherFunction = (fn) => {
  fn()
  fn()
  fn()
}

anotherFunction(callback)
```

We can see that `callback` is being passed to `anotherFunction` making it

- a callback because is being passed as an argument to another function
- a closure because it's capturing the variable `i` outside of it
# Compose closures and callbacks to create new functions
 
- Composition - a function passing one value to another function

instead of doing:

```javascript
let value = callback()
multiply(value)
```

we can pass callback inside multiply straight away:

```javascript
multiply(callback())
```

Cleaner ways to compose functions:

- using `pipe` from `lodash/fp`
- using `compose` from `lodash/fp`

**example**

```javascript
// multiply(callback()) becomes
pipe(callback, multiply)
```

```javascript
// multiply(callback()) becomes
compose(multiply, callback)
```

compose allows you to refactor the code by removing nested functions and instead list them out in order.
 
![javascript-what-is-a-callback-in-javascript-callback.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1665442372913/Ipkq9P-kQ.png align="left")

With pipe, each return is passed to the next function in line.

With compose the return is passed to the previous function in line - so the value returned to the callback will be passed to multiply.

## Using pipe vs compose

 
> We're going to stick to using pipe, which takes the functions in a different order, because when thinking asynchronously, it makes more sense to think, "Well, the callback is called, and then multiply is called." We're sticking to this.

# Defining the broadcaster and Listener Relationship
 

There are two concepts that will be used through this**listener** and **broadcaster**. This concepts holds a strong relationship.

This are common names used to define the behavior of this functions.
* **listener** refers to a callback function, a function meant to be passed as argument to another function
* **broadcaster** refers to a function that accepts a Listener (a callback) and triggers it to perform certain task.

In this scenario, the **broadcast** function is the one that initiate the data flow. The broadcaster starts the communication performing a task and calling the **listeners** asking them to perform certain task.
🔑 In general words, the **listeners** function are not meant to be used directly.

```javascript
let listener = value => {
    console.log(vale)
}

let broadcaster = callback => {
    callback(1)
    callback(2)
    callback(3)
}

broadcaster(listener)
```
 

# Time is a Hidden Variable in javascript

 
Another type of function that we will often use is call **operator** An operator is just a function that calls the broadcaster but in some particular way.

In the example, the first operator that we see is a timeout operator. A function that calls the broadcaster based on some time function, in this case it use the `setTimeout` call to orchestrate how the broadcaster will be executed.

The first example shows us how the time definition affect the behavior of the broadcaster but more important it shows how we can use a closure inside the operator to change this behavior

```javascript
let operator = (broadcaster) => (listener) => {
  let currentValue = 0
  broadcaster((value) => {
    currentValue += value
    setTimeout(() => {
      // this call closes over currentValue
      listener(currentValue)
    }, currentValue * 1000)
  })
}
```

In the example the line `currentValue += value` is run immediately and by the definition of the broadcaster function, that line was call 3 times, then the value of `currentValue` is _"trap"_ inside the `setTimeout` call as a closure.

🔑 The key of an operator is to give it a good name to represent what is the actual behavior captured by the function definition

# Solve Callback Hell with Composition
 
[Callback hell](http://callbackhell.com) is a concept used to describe a situation that happens when there are too many callbacks nested inside of each other, this results in _"spagetti code"_ a piece of code that is hard to read, maintain and difficult to reason about. The composition patterns that we saw here can help to avoid this situation by creating flexibility with how the callbacks are managed.

The first step to solve this callback hell is to figured out what can be extracted as a function, in this example the is easy to see that there are at least 3 main function that can be created:

- one for the `fetch` callback

```javascript
let getURL = () => {
  fetch(`https://api.github.com/user/36073`)
    .then((response) => response.json())
    .then((data) => {
      console.log(data)
    })
}
```

- one for `setTimeout` call

```javascript
let timeout = () => {
  setTimeout(() => {}, 1000)
}
```

- and one for the click event

```javascript
let click = () => {
  document.addEventListener('click', (event) => {})
}
```

🔑 At this step we just **captured the behaviors** described by the original code in different functions

The next step is to figured out how to wire up this functions.

First, we define the behavior that we need to capture, the relationship of nesting is essentially to invoke an outer function that accepts a callback that calls an inner function that also takes a callback, this behavior can be described by a function

```javascript
let nest = (inner) => (outer) => (listener) => {
  outer((value) => {
    inner(listener)
  })
}
```

So, by using the functions that we defined, this will look like:

```javascript
nest(getURL)(nest(timeout)(click))((data) => {
  console.log(data)
})
```

Is important to notice the pattern here, this code is hard to read and have multiple parenthesis that make it also hard to write and easy to mess, but this is a pattern that we already know and solve by composition in [lesson 4](4-compose-closures-and-callbacks-to-create-new-functions.md)

We can use `pipe` from `lodash/fp` package and refactor our code

```javascript
let timeoutURL = pipe(
  nest(timeout),
  nest(getURL)
)

timeoutURL(click)((data) => {
  console.log(data)
})
```

 # Passing Values Through Callback Hell
 
In the previous talk we avoid the situation of passing values through the callbacks, to do this we need to define a _mapping_ function that will accept the value and create the callback based on a configuration step that includes this value. To do that, each of the functions defined will now be wrapped by another function that accepts a `config` parameter like:

```javascript
let click = (config) => (listener) => {
  document.addEventListener(config, listener)
}
```

With this in place we need to change our mental model and think about the `mapInner` function as a function that creates or generates callbacks with the correct values.

```javascript
let nest = (mapInner) => (outer) => (listener) => {
  outer((config) => {
    let innerr = mapInner(config)
    inner(listener)
  })
}
```

🔑 So now, instead of thinking about nested callback, we can think of what value is the function receiving and how we want to configure the inner callback.
 
 
