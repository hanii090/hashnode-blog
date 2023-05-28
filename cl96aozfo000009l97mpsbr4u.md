---
title: "Javascript Working with Primitive Types, Prototypal Inheritance also var, let, const, and block scopes"
datePublished: Thu Oct 13 2022 00:00:45 GMT+0000 (Coordinated Universal Time)
cuid: cl96aozfo000009l97mpsbr4u
slug: javascript-working-with-primitive-types-prototypal-inheritance-also-var-let-const-and-block-scopes-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1665698071935/PuS_KPA3W.png
tags: javascript, javascript-framework, beginners, 2articles1week

---


Within JavaScript, there's a concept of a **primitive value or a primitive data type**. You may have heard or might hear that everything in JavaScript is an object. As we look at these primitive values, we can see that this is not true. **There are in fact seven current types within JavaScript that are definitely not objects.**

## typeof operator

```js
// output of log shown as comment
console.log(typeof 'hello world') // string
console.log(typeof 1) // number
console.log(typeof false) // boolean
console.log(typeof 42n) // bigint
console.log(typeof Symbol()) // symbol
console.log(typeof null) // object
console.log(typeof undefined) // undefined
```

The difference between primitive types and Arrays or Objects is that they can't be mutated. The are immutable.

Primitive types don't have methods or properties on them either, they are not objects. They are the lowest level implementation in JavaScript.

_So why can we use methods on primitive types like string.toUpperCase?_

The `typeof` operator in JavaScript evaluates a statement to it's right and tells you what the type of that statement is. It will be a primitive or an object.

`"Hello World"` is a `string`
`1` is a `number`
`false` is a `boolean`
`42n` is how we define `bigint` in JavaScript.

There's also another literal form for creating a symbol in JavaScript so you use `Symbol()` which is it's constructor.

Because of a bug that hasn't been fixed in JavaScript early on, `null` will show as an object when passed in to the `typeof` operator.

`undefined` is the seventh and final primitive type.

Primitive types are treated differently than objects. First we'll see how an object can be successfully mutated.

## Mutate an Object

```jsx
let obj = { a : 1}

function addTwo(obj) {
  obj.a = 2
}
```

"Everything in JavaScript is an Object" is something that you've probably been told sometime in your career. As was discussed in the previous lesson,  , this is clearly not true. So what is really happening?

## Method used on Primitive Type

```js
const str = 'foo'

console.log(typeof str) // string
console.log(str.length) // 3
```

We should see an error when we try to use a 'dot' method on a string right? By rule, Primitive Types don't have any properties or methods on them. But as seen above, `str.length` returns a number, which means we just used a method on a string. 🧐

JavaScript uses a process called Autoboxing. **Autoboxing wraps Primitive Types in an object so that we have the convenience of objects when dealing with Primitive Types in JavaScript.**

When a Primitive Type is wrapped, it will connect that Type with a built-in object prototype that corresponds with the Primitive Type. This is where you get `string.length`, `string.includes`, or `string.toUpperCase`. ([Methods documented on MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/prototype)).

But because of autoboxing we treat primitive types like objects due to JavaScript wrapping those primitives into objects, now you can see why this misconception has started and is here today.


ProtoTypes are the way that JavaScript powers inheritance. This mimicks some behavior that you would expect out of a class in another language like C# or Java, but JavaScript does not have classes.

JavaScript adds a `__proto__` property on your object that links other methods and properties (which themselves can be objects).

Open up your console and create an empty object `let a = {}`. If you log that variable, you'll see that `__proto__` was automatically added even though the object is 'empty.'

**This **proto** key is also called Dunder proto.**

That Dunder proto property is what prototypes and inheritance is in JavaScript.

Every time you work with an object within JavaScript, as long as you don't mutate it later, will automatically be linked through this Dunder proto property to the global object prototype.

## toString an Empty Object

```js
const a = {}

a.toString()

console.log(a) // "[object Object]"
```

Even though this object is empty, when I call `toString` on `a`, JavaScript will first look on the object for the `toString` method. When it doesn't find that method there, it will step into `__proto__` and look for `toString` inside that object. In this case, it finds the `toString` method and calls it.

You could think of this method call as being called like this: `a.__proto__.toString()`.

## Nested Dunder Properties in Objects

```js
const a = {}

const b = Object.create(a)

console.log(b)
/*
{}
  __proto__:
    __proto__: Object
*/
```

`Object.create` will create an object and set the first argument that you pass it as the `__proto__` for that variable, in this case, the object `b` will have the empty object `a` set as its prototype.

You can see from above that prototypes can be nested. In this case, we have an empty object (`b`), it's prototype is also empty because it was set to `a`, and then `a`'s prototype is set to the Global Object ProtoType that all objects are given when no prototype is specified or mutated otherwise.

##  prototype returns true

```js
const a = {}

a.toString = function() { return true }

const b = Object.create(a)

console.log(b.toString()) // true
```

The nearest prototype property wins when JavaScript is looking for a function. In this case, `b` is looking for a `toString` property. It doesn't find it in it's own object so it starts looking up the chain. The object `a` is set as the next prototype and it has a `toString` function. That function gets called. It doesn't matter that the Global Object Prototype has a `toString` property because JavaScript quits looking up the chain as soon as it finds something that matches.

## prototype in the console

```js
console.log(b)
/*
{}
  __proto__:
    toString: f()
    __proto__:
      constructor: f Object()
      toString: f toString()
*/
```

A method call will return `undefined` if it never finds the method called after going all the way up the prototype chain.

All types of objects within JavaScript have their own global built-in prototype objects that get connected whenever a new instance is created.

**Plain Objects, Arrays, Maps, Sets, and Functions have their own Global Prototype Object.**

## Add property to function

```js
function foo() {}

foo.firstName = 'zac'

console.log(foo.firstName) // zac
```

**Functions are first class objects in JavaScript which means they can have their own properties and methods like any other plain object could.** You wouldn't typically find people adding properties to a function though, this is for demonstration purposes.

Every time a function is created, JavaScript will add a property to that function called `prototype`. This `prototype` property has two things on it. a constructor property which points back to the function itself and the `__proto__` property.

_Why does the `prototype` property exist if we already have **proto**?_
_What purpose does `prototype` serve?_

This `prototype` property is not used in the prototype chain look-up if we were to "dot" onto the `foo` function when looking up a property.

`__proto__` is also automatically created when a function is created and that is what JavaScript will use to look up methods it doesn't find immediately.

##

```js
function foo() {}

foo.prototype.test = 'hello world'

console.log(foo.prototype) // foo { test: 'hello world'}

const name = new foo()

console.log(name.test) // hello world
```

`name.test` is found on the `name` object because it is a new instance of the `foo()` function. Whatever `prototype` that currently lives on a function when a new instance is created with the `new` keyword will also be present on that new variable (`name`) as well.

_Is the `prototype` that's passed to the new instance passed by reference? In other words, can it be mutated?_

## Origininal prototype is immutable

```js
$ name
  foo {}
    __proto__: 
      test: "hello world"
      constructor: ƒ foo()
      __proto__: Object
$ name.test = "hey"
  "hey"
$ name
  foo {test: "hey"}

$ foo.prototype.test
  "hello world"
```

The keyword `Object` is actually a function in JavaScript. `Array`, `Map`, and `Set` also have global functions like `Object`.

These global functions are connected to the `__proto__` property to every instance of the object it corresoponds to. These functions are where many methods you use actually live.

In other words, you're using prototype inheritance every time you use any kind of object within your code.

## Global prototype function is equal to the instance method

```js
Array.prototype.map === [].map // true
```

Scope is what determines where/when you can access a variable or function in your code. Block scope is a scope area that is usually defined by `if`, `switch`, or loops (eg. `for` or `while`).
More on the different types of scope in Ming-Shiuan's [JavaScript: Introduction to Scope (function scope, block scope)](https://dev.to/sandy8111112004/javascript-introduction-to-scope-function-scope-block-scope-d11).

Block scope can also be defined by declaring variables within open and closed curly braces, `{ }`. This is an overload of the object literal syntax.

It's important to note, there will not be any block scope until an assignment is made within that block.

## Block scope

```js
var firstName = 'zac'
{
  var firstName = 'jones'
  console.log(firstName) // jones
}
console.log(firstName) // jones
```

Objects are not scoped, so simply looking for curlies will not completely help you when determining block scope.

Up above you can see an assignment and curlies which makes it a block scope

_Why doesn't the console logs respect the block scope then? We know it's valid._

**`var` only cares about function execution scopes, not block scopes.** The global namespace is the function execution scope for the example above so both assignments are mutating the same variable.

## Function Execution Scope

```js
var firstName = 'zac'
function foo() {
  var firstName = 'jones'
  console.log(firstName) // jones
}
console.log(firstName) // zac
foo()
```

To make the code work as we would expect it to, you can make the block scope a function and see that the `console.logs` will log out different variable values.

**`var` declarations are hoisted to the top of their execution context with an initial value of `undefined`.**

The code will look like this under the hood during execution time

## Function Execution Scope under the hood

```js
var firstName = undefined
firstName = 'zac'
function foo() {
  var firstName = undefined
  firstName = 'jones'
  console.log(firstName) // jones
}
console.log(firstName) // zac
foo()
```

Any vars declared in a global function context are placed on the `window` object.
So in the browser console, `window.firstName` will be `zac` for me.

## Block Scope with let

```js
let firstName = 'zac'
{
  let firstName = 'jones'
  console.log(firstName) // jones
}
console.log(firstName) // zac
```

**`let` scopes to block scope as well as function execution scope.**

`let` is only initialized when the parser evaluates it. It doesn't get hoisted to the top like `var` does. Finally, if we tried to re-declare let within the same scope, it would throw an error while using a var would not.

`const` has the same rules as `let` does.

The difference is that it `const` cannot be redefined to a different value. The thing to watch out for here is that `const` only does a shallow compare so if you change a value of a property in an object or an item in an array, `const` will not complain.

## Block Scope with const

```js
const firstName = 'zac'
{
  const firstName = 'jones'
  console.log(firstName) // jones
  firstName = 'not jones' // error!!
}
console.log(firstName) // zac
```