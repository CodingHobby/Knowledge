# EcmaScript 6

ES6, or ES Next, or ES2015, or ES Harmony (whatever you want to call it), introduced some cool new concepts to the JavaScript world. It added Classes, Generators, Symbols, two new ways to declare variables, spread operators, deconstructing and so much more!

## Classes
One of the major concepts introduced by ES6 is the `class` syntax.

That's what it is: a syntax. In the end it doesn't compile into anything weird, just an Object, like everything in JavaScript.

A class looks something like this:

```javascript
class myClass {
  constructor(args) {
	  this.args = args
  }

  method() {
	  console.log(args)
  }
}
```
A class can have different methods and a constructor, just like in any other language. You can also set instance variables through the use of the `this`. syntax, which you should already be familiar with.

## Generators
Generators are basically just functions that can pause. That's it. They return a different value everythime the `next()` method is called on them:

```javascript
function* random() {
  while(true) {
    yield (Math.random()*10)
  }
}

console.log(random.next().value)
console.log(random.next().value)
console.log(random.next().value)
//...
```

## Symbols
Symbols are nothing but unique values with their own type.

This means that `new Sybmol('foo')` does not equal `new Symbol('foo')`, since each symbol is unique.

## let and const
Another concept introduced by ES2015 is the one of scoped variables: this means that each variable only exists in its own scope. Have you ever encountered a situation like this:

```javascript
var someValue = 'Hello'
if(true) {
  var someValue = 'World'
  console.log(someValue) // => World
}
console.log(someValue)  // => World
```

But, thanks to scoped variables, what happens is now this:

```javascript
let someValue = 'Hello'
if(true) {
	let someValue = 'World'
	console.log(someValue)  // => World
}
console.log(someValue)    // => Hello
```
There are two new keywords for variables: `let` and `const`. Well, technically `const` is not for variables: it's for constants, but the idea is still the same: the only difference is that once you declare a `const` you cannot change its value later in the code.

## Spread operators
Spread operators are really useful to make functions with a variable number of arguments: the way they work is they basically flatten an array:

```javascript
function say(...args) {
  args.forEach(arg) {
	  console.log(arg)
	}
}
```

This way we can call the function like `say('Hello', 'World')`, and both Hello and World will be printed to the console. Pretty cool, ay?

## Deconstructing
Deconstructing is extremely useful to get only certain properties out of an object: for example:

```javascript
let obj = {foo: 'bar'}
let {foo} = obj  // => let foo = 'bar'
```

This is also really useful for imports:

```javascript
import {Component} from 'react' // => const Component = require('react').Component
```

Also, that is the new ES6 import syntax, which looks a bit different.

## Babel
But how do you use ES6? Well, it's not yet completely standardized by all browsers, and even node doesn't yet support it fully.

To do that we need to transpile it into ES5 syntax, and we can do that through Babel.

Babel is just a transpiler which takes all of your ES6 code, bundles it and turns it into ES5 code.
