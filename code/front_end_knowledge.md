# Front-end Job Interview Questions

## Table of Contents

  1. [General Questions](#general-questions)
  1. [HTML Questions](#html-questions)
  1. [CSS Questions](#css-questions)
  1. [JS Questions](#js-questions)
  1. [React Questions](#react-questions)
  1. [Testing Questions](#testing-questions)
  1. [Performance Questions](#performance-questions)
  1. [Network Questions](#network-questions)
  1. [Coding Questions](#coding-questions)
  1. [Fun Questions](#fun-questions)

#### General Questions:

**What did you learn yesterday/this week?**

**What excites or interests you about coding?**

**What is a recent technical challenge you experienced and how did you solve it?**

**Can you describe the difference between progressive enhancement and graceful degradation?**

**How would you optimize a website's assets/resources?**

- minify + concatenate HTML + CSS  
- remove unused/duplicate CSS/JS  
- tinyPNG/JPEGmini images  
- data URIs for small images
- gzip + cdn everything  

**How many resources will a browser download from a given domain at a time? What are the exceptions?**

browsers have different limits - the typical limit is 6  
not sure about http2

**What is Flash of Unstyled Content? How do you avoid FOUC?**

**Explain some of the pros and cons for CSS animations versus JavaScript animations.**

**What does CORS stand for and what issue does it address?**

----

#### HTML Questions:

* What does a `doctype` do?
* What's the difference between full standards mode, almost standards mode and quirks mode?
* What's the difference between HTML and XHTML?
* Are there any problems with serving pages as `application/xhtml+xml`?
* How do you serve a page with content in multiple languages?
* What kind of things must you be wary of when design or developing for multilingual sites?
* What are `data-` attributes good for?
* Consider HTML5 as an open web platform. What are the building blocks of HTML5?
* Describe the difference between a `cookie`, `sessionStorage` and `localStorage`.
* Describe the difference between `<script>`, `<script async>` and `<script defer>`.
* Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?
* What is progressive rendering?
* Have you used different HTML templating languages before?

----

#### CSS Questions:

* What is the difference between classes and IDs in CSS?
* What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?
* Describe Floats and how they work.
* Describe z-index and how stacking context is formed.
* Describe BFC(Block Formatting Context) and how it works.
* What are the various clearing techniques and which is appropriate for what context?
* Explain CSS sprites, and how you would implement them on a page or site.
* What are your favourite image replacement techniques and which do you use when?
* How would you approach fixing browser-specific styling issues?
* How do you serve your pages for feature-constrained browsers?
  * What techniques/processes do you use?
* What are the different ways to visually hide content (and make it available only for screen readers)?
* Have you ever used a grid system, and if so, what do you prefer?
* Have you used or implemented media queries or mobile specific layouts/CSS?
* Are you familiar with styling SVG?
* How do you optimize your webpages for print?
* What are some of the "gotchas" for writing efficient CSS?
* What are the advantages/disadvantages of using CSS preprocessors?
  * Describe what you like and dislike about the CSS preprocessors you have used.
* How would you implement a web design comp that uses non-standard fonts?
* Explain how a browser determines what elements match a CSS selector.
* Describe pseudo-elements and discuss what they are used for.
* Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
* What does ```* { box-sizing: border-box; }``` do? What are its advantages?
* List as many values for the display property that you can remember.
* What's the difference between inline and inline-block?
* What's the difference between a relative, fixed, absolute and statically positioned element?
* The 'C' in CSS stands for Cascading.  How is priority determined in assigning styles (a few examples)?  How can you use this system to your advantage?
* What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
* Have you played around with the new CSS Flexbox or Grid specs?
* How is responsive design different from adaptive design?
* Have you ever worked with retina graphics? If so, when and what techniques did you use?
* Is there any reason you'd want to use `translate()` instead of *absolute positioning*, or vice-versa? And why?

----

#### JS Questions:

**Explain event delegation**

event delegation is adding a single event listener to a top level DOM element and checking to see which element triggered it within the event listener callback through e.target.value.

this works because events bubble up from the element they emitted from. it's useful because it avoids having to add/remove event listeners on lots of individual elements.

**Explain how `this` works in JavaScript**

`this` provides a pointer to the scope/context the current code is running within at runtime. this can refer to 4 different things depending on the context within which it's used:

1. the global scope
2. the parent object (if it's a method)
3. the call/bind/apply this argument
4. the `new` object

explained:

1\. the global scope

this is the default. if there is a variable `a` with the value 2 in the global scope, `this.a` will be 2. in the browser, the global object is `window`.

2\. the parent object

if the function is a method of an object, this refers to that object. if there is a property `a` on the object with the value 3, `this.a` will be 3 even if there is a global variable `a` whose value is 2.

3\. the call/bind/apply this argument

```js
let f = () => console.log(this.a)
const obj1 = { a: 1 }
const obj2 = { a: 2 }
const obj3 = { a: 3 }

f.call(obj1)      // 1
f.apply(obj2)     // 2

f = f.bind(obj3)
f()               // 3
```

4\. the `new` object

when a function is called with the `new` keyword, it creates a new object and returns that (unless the function returns its own object)

```js
const Thing = (a, b) => {
  this.a = a
  this.b = b
  this.getValues = () => console.log(this.a, this.b)
  return this
}

const thing = new Thing(1, 2)
thing.getValues() // 1, 2
```

**Explain how prototypal inheritance works**

everything is an object in javascript. every object has a prototype which it inherited properties from. for example, a new array inherits from the Array object. the Array object has lots of useful methods and properties which are passed down to the new array. when you attempt to access a property/method on the array it will first look to see if the property/method is attached to that array. if not, it will go up the prototype chain to check if the parent object has it. it keeps going until it finds the property/method until it hits the Object object. it will always stop at the first instance so inherited properties/methods can be overridden with own properties.

```js
const array = [] // array inherits from Array
array.length
// checks if `array` owns property `length` - no
// checks if Array object owns property `length` - yes
// returns 0
array.length = 5
array.length
// checks if `array` owns property `length` - yes
// returns 5
```

you can find a variable's prototype through `varname.prototype`

you can create an object with a prototype using `Object.create(prototypeObj)`

**What do you think of AMD vs CommonJS?**

amd is a bit more ugly and requires external libraries like require.js to use.

common js has been adopted by node and requires a transpiler like babel to use in the browser.

both have been superseded by es6 import/export in the browser.

**Explain why the following doesn't work as an IIFE: `function foo(){ }();`**

when using the function keyword, the parser recognizes you're defining a function not calling one, so it treats the calling parentheses at the end as a separate statement.

wrapping the entire thing in parentheses causes the parser to evaluate everything inside, so it defines the function, runs it and immediately closes the execution context.

**What's the difference between a variable that is: `null`, `undefined` or undeclared?**

An undeclared variable is not declared anywhere in the code.
An undefined variable is one that has been declared but not assigned a value.
A null variable is one that has been assigned the value `null`.

`undefined` and `null` are values in javascript, but undeclared is not. By default, all attempts to access an undeclared variable return `undefined`.

**What is a closure, and how/why would you use one?**

a closure is a function defined and returned inside another function whose execution context remains available in the global scope after it's been run.

```js
const outer = () => {
  const a = 2
  const inner = () => console.log(a)
  return inner
}

const inner = outer()
a       // undefined
inner() // 2
```

even though `a` is defined inside outer and therefore should not be available in the global scope, we can access the value through `inner`. this is useful in the case we want to have private variables that shouldn't be changed by global code, or if we have an async callback that should be able to hold onto values until it's ready to be called.

```js
const wait = (message) => {
  const callback = () => console.log(message)
  setTimeout(callback, 1000)
}
wait('hello')

// `callback` is a closure that still has access to `message` when it's called even though `wait` was called 1000 milliseconds ago
```

**What's a typical use case for anonymous functions?**

as a callback or an IIFE where defining the function separately is not particularly necessary for code clarity. however, anonymous functions may be difficult to debug in the stack trace.

**Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?**

`function Person(){}` is a function definition

`var person = Person()` assigns the return value of Person to the person variable (`undefined`)

`var person = new Person()` assigns the return value of Person as a constructor function to the person variable (`{}`)

**What's the difference between `.call` and `.apply`?**

`.call` takes context as the first argument and **passes all subsequent arguments** as the arguments to the function being called.

`.apply` takes context as the first argument and **an array of arguments** to be passed as the arguments to the function being called.

**Explain `Function.prototype.bind`.**

`Function.prototype.bind` is a method that lets you set the values of some arguments for a function so when it is called those arguments are already preset.

**When would you use `document.write()`?**

never

**What's the difference between feature detection, feature inference, and using the UA string?**

**Explain Ajax in as much detail as possible.**

**What are the advantages and disadvantages of using Ajax?**

**Explain how JSONP works (and how it's not really Ajax).**

**Have you ever used JavaScript templating? If so, what libraries have you used?**

**Explain "hoisting".**

**Describe event bubbling.**

**What's the difference between an "attribute" and a "property"?**

**What is the difference between `==` and `===`?**

`===` checks for type equality as well as value equality.

```js
2 == '2'  // true
2 === '2' // false
```

**Explain the same-origin policy with regards to JavaScript.**

**Make this work: `duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]`**

```js
const duplicate = (array) =>
  array.reduce((a, b) => {
    a.push(b)
    return a
  }, array)
```

**Why is it called a Ternary expression, what does the word "Ternary" indicate?**

'ternary' means third and a ternary operation requires 3 arguments: a statement to evaluate, a result if true and a result if false

**What is `"use strict";`? what are the advantages and disadvantages to using it?**

a statement that tells the interpreter to run the code block in strict mode, which throws errors for things like defining variables without `var`/`const`/`let`

**Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?**

**Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?**

**Explain what a single page app is and how to make one SEO-friendly.**

**What is the extent of your experience with Promises and/or their polyfills?**

**What are the pros and cons of using Promises instead of callbacks?**

**What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?**

**What tools and techniques do you use debugging JavaScript code?**

**What language constructions do you use for iterating over object properties and array items?**

arrays - use forEach/map/reduce  
objects - use Object.keys(...).forEach/map/reduce

**Explain the difference between mutable and immutable objects.**
  * What is an example of an immutable object in JavaScript?
  * What are the pros and cons of immutability?
  * How can you achieve immutability in your own code?

**Explain the difference between synchronous and asynchronous functions.**
**What is event loop?**
  * What is the difference between call stack and task queue?

**Explain the differences on the usage of `foo` between `function foo() {}` and `var foo = function() {}`**

`function foo() {}` is a function statement. `var foo = function() {}` is a function expression.

function expressions are not hoisted so you must use them after defining them

----

#### React Questions

**What is React? How is it different from other JS frameworks?**

**What happens during the lifecycle of a React component?**

components have various lifecycle methods, these are generally setup (constructor, componentWillMount, componentDidMount), updating (componentWillUpdate, componentWillReceiveProps) and destruction (componentWillUnmount)

render is first called inbetween componentWillMount and componentDidMount. it is then called every time state updates.

**Explain what JSX is and why it's useful**

JSX is an extension to javascript that allows you to create DOM elements in something closely resembling HTML. it is transpiled into javascript that calls standard JS functions like React.createElement.

it's useful because writing endless javascript functions to create and modify and combine elements is extremely tedious and writing tedious code usually means more errors. it's very clean to read.

**What are pure functional components? When would you use them**

they are pure functions which simply take props and render the UI.

they are used for presentational components that don't have state or use lifecycle methods.

**When would you use keys in React?**

keys are needed for things like mapped lists where more than 1 of the same component is rendered. keys let react know the order they're meant to go in and quickly determine if the list has changed when re-rendering.

**When would you use refs in React?**

refs allow you to work with actual DOM elements from inside React. once the view has been rendered in the DOM, the callback function added to the ref attribute saves a reference to the actual DOM node, so it can be manipulated. this is useful for things like focusing form inputs etc

**What happens when you call setState?**

the object passed to set state is merged with the previous state object.  
then React will begin constructing a new virtual DOM tree and diffing it against the previous one to determine which components need re-rendering.  
the render function on those components will then be called, so long as shouldComponentUpdate returns true.

**What’s the difference between an Element and a Component in React?**

an element is what is rendered. a component is what renders the element

**In which lifecycle event do you make AJAX requests and why?**

componentDidMount - the component might not be mounted by the time the ajax request is done so calling setState won't work. also React Fiber is implementing a more flexible version of componentWillMount that stops and starts based on performance requirements, so ajax requests may not get called, or called more than once.

**What does shouldComponentUpdate do and why is it important?**

it allows you to skip re-renders if the ui will never change. this is better for performance.

**Describe how events are handled in React.**

React has its own SyntheticEvent object which is a facade that normalizes browser events across different browsers. it has a limited set of properties/methods, but the actual browser event can be accessed via event.nativeEvent

----

#### Redux Questions:

**How is it different from MVC and Flux?**

In MVC applications, any part of the codebase can update the model and changes in the model can affect any part of the code.

The Flux pattern ensures that dataflow is unidirectional - user input causes only the relevant stores to change, and these changes are then emitted as events which can trigger a state change in the rest of the code.

**What are “actions” in Redux?**

actions are event-like objects that contain information about the type of action that has happened and the relevant data to that action. they are dispatched to the store when an event occurs.

**What is the role of reducers in Redux?**

reducers are pure functions that describe how state should change dependent on the action that has been dispatched. they take the current state and return the new state based on the action payload.

**What is ‘Store’ in Redux?**

The store is an object that contains the entire app's state. it is the single source of truth for the app. it has a dispatch method that allows actions to be emitted.

----

#### Testing Questions:

* What are some advantages/disadvantages to testing your code?
* What tools would you use to test your code's functionality?
* What is the difference between a unit test and a functional/integration test?
* What is the purpose of a code style linting tool?

#### Performance Questions:

* What tools would you use to find a performance bug in your code?
* What are some ways you may improve your website's scrolling performance?
* Explain the difference between layout, painting and compositing.

#### Network Questions:

* Traditionally, why has it been better to serve site assets from multiple domains?
* Do your best to describe the process from the time you type in a website's URL to it finishing loading on your screen.
* What are the differences between Long-Polling, Websockets and Server-Sent Events?
* Explain the following request and response headers:
  * Diff. between Expires, Date, Age and If-Modified-...
  * Do Not Track
  * Cache-Control
  * Transfer-Encoding
  * ETag
  * X-Frame-Options
* What are HTTP methods? List all HTTP methods that you know, and explain them.

#### Coding Questions:

*Question: What is the value of `foo`?*
```javascript
var foo = 10 + '20';
```

30

*Question: How would you make this work?*
```javascript
add(2, 5); // 7
add(2)(5); // 7
```

*Question: What value is returned from the following statement?*
```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

"goh angasal a m'i"

*Question: What is the value of `window.foo`?*
```javascript
( window.foo || ( window.foo = "bar" ) );
```

"bar"

*Question: What is the outcome of the two alerts below?*
```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```

"Hello World"
"Hello"

*Question: What is the value of `foo.length`?*
```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

2

*Question: What is the value of `foo.x`?*
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```

{n: 2}

*Question: What does the following code print?*
```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```

"one"
"three"
"two"

#### Fun Questions:

* What's a cool project that you've recently worked on?
* What are some things you like about the developer tools you use?
* Who inspires you in the front-end community?
* Do you have any pet projects? What kind?
* What's your favorite feature of Internet Explorer?
* How do you like your coffee?


#### Contributors:

This document started in 2009 as a collaboration of [@paul_irish](https://twitter.com/paul_irish) [@bentruyman](https://twitter.com/bentruyman) [@cowboy](https://twitter.com/cowboy) [@ajpiano](https://twitter.com/ajpiano)  [@SlexAxton](https://twitter.com/slexaxton) [@boazsender](https://twitter.com/boazsender) [@miketaylr](https://twitter.com/miketaylr) [@vladikoff](https://twitter.com/vladikoff) [@gf3](https://twitter.com/gf3) [@jon_neal](https://twitter.com/jon_neal) [@sambreed](https://twitter.com/sambreed) and [@iansym](https://twitter.com/iansym).

It has since received contributions from over [100 developers](https://github.com/h5bp/Front-end-Developer-Interview-Questions/graphs/contributors).
