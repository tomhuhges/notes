# design patterns

## good resources

- JavaScript Design Patterns - Addy Osmani
- Mastering JavaScript Design Patterns - Packt course

----

most of these notes were made while simultaneously referencing the 2 resources above.

- [getters and setters](#getters-and-setters)
- [facade pattern](#facade-pattern)
- [adapter pattern](#adapter-pattern)
- [namespacing](#namespacing)
- [amd/require.js](#amdrequirejs)
- [pubsub pattern](#pubsub-pattern)
- [prototypal inheritance](#prototypal-inheritance)
- [mixins pattern](#mixins-pattern)
- [mvc / mvp / mvvm](#mvc--mvp--mvvm)
- [factory pattern](#factory-pattern)
- [lazy initialisation](#lazy-initialisation)
- [generator/iterator pattern](#generatoriterator-pattern)
- [composite pattern](#composite-pattern)
- [simple node/express server](#simple-nodeexpress-server)
- [promises](#promises)

----

### getters and setters

when creating modules, you can expose variables with `get` and `set` without giving outside code/users access any other functionality contained within the module. those variable values can be retrieved or assigned without calling any other methods that are called to create them.

##### usage

module:  
```js
var module = (function(){
	var firstName = "Bob";
	var lastName = "Smith";

	return {
		get fullName(){
			return firstName + " " + lastName;
		},
		set fullName(value){
			value.split(" ");
			firstName = ( value[0] || "" );
			lastName = ( value[1] || "" );
		}
	}
})();
```

outside use:  
```js
console.log(module.fullName); // "Bob Smith"
module.fullName = "Jim Bloggs";
console.log(module.fullName); // "Jim Bloggs"
```

----

### facade pattern

facades abstract complicated code into a single function with a simple interface, for example a function that allows users to pass in an element, event, and callback to add an event listener. inside, the function checks to see which method is available depending on the browser version and calls the relevant one. the user doesn't have to update three separate function calls when maintaining the code. facades are typically created when dealing with multiple apis to do one job.

##### usage

```js
function addEvent(element, type, callback) {
	if ( window.addEventListener ) {
		element.addEventListener(type, callback);
	} else if ( window.attachEvent ) {
		element.attachEvent("on" + type, callback);
	} else {
		element["on" + type] = callback;
	}
}

addEvent(myelement, "click", alert("myelement was clicked!")); // calls the relevant function under the hood
```

----

### adapter pattern

similar, to `facade`, the adapter pattern wraps one function in another in order to make it more useable or suitable. Most often it's used to provide an interface to a function where the existing one is incompatible.

##### usage

```js
/* the adapter below lets you call a
setTimeout with seconds instead of milliseconds,
and places the timing as the first argument.
the below is probably not recommended, given
most developers will be familiar with standard
setTimeout but not your own implementation,
whether it's more pleasant to read or not. */

function setTimeoutAdapter( seconds, callback ) {
	return setTimeout(callback, seconds * 1000 );
}

setTimeoutAdapter( 5, alert("hello 5 seconds later!") );
```

----

### pubsub pattern

the pubsub pattern is a way to decouple modules through a module which acts as a middleman between the others. rather than having a module call lots of other module methods when a particular event occurs, it can simply call the pubsub module's 'trigger/emit' method. the pubsub module keeps a list of callbacks for each event, and fires them when the trigger is called. the pubsub module has 2 other methods, 'on' and 'off', which allow modules to subscribe or unsubscribe from events.

##### usage

```js
// simple pubsub module

var events = {}

var on = function( event, callback ) {
	if ( events.hasOwnProperty( event ) {
		events[event].push({callback: callback});
	} else {
		events[event] = [{callback: callback}];
	}
}

var off = function( event, callback ) {
	for ( var i=events.length-1; i >= 0; i-- ) {
		if ( events[event][i].callback === callback ) {
			events[event].splice(i,1);
		}
	}
}

var emit = function( event ) {
	if ( events.hasOwnProperty( event ) ) {
		for ( var i=0; i < events[event].length; i++ ) {
			events[event][i].callback();
		}
	}
}
```

----

### mixin pattern

a mixin is a class containing useful, reusable methods that can be added to other objects. the methods usually provide general-use functionality that can be applicable for multiple things, and added to multiple modules.

it can used as `Object.create(Mixin)`, with Mixin acting as a prototype, or with a helper function (like underscore's `_.extend`) that adds methods from the mixin directly to an existing prototype if the object is created from a constructor (this seems like bad practice, as it directly mutates the existing protype, but seems to be used by big frameworks/libraries?) backbone + ember both have mixins for adding pubsub functionality to multiple objects.


##### usage
```js
var Mixin = {
	sayHello: function(){
		console.log('hello');
	}
}

// Object.create()
var newObj = Object.create(Mixin);
newObj.sayHello() // 'hello'

// constructor
var Obj = function() {...}
_.extend( Obj.prototype, Mixin );
var newObj2 = new Obj();
newObj2.sayHello() // 'hello'
```

----

### mvc / mvp / mvvm

**mvc** - the controller reacts to events and updates the model when data is changed via the view  
**mvp** - some controller functionality is handed off to the view. the presenter just updates the model and view  
**mvvm** - the view doesn't have any functional code at all, it just displays data. the viewmodel instead handles all events, updating both the model and the view.

----

### factory pattern

factories are useful when we need to create lots of objects that are similar but may have a few differences.

##### usage

```js
// factory function
function ModelFactory(type, data) {
	var Model;

	Model = type === 'album'
	? AlbumModel

	: type === 'single'
	? SingleModel

	: type === 'track'
	? TrackModel

	: TrackModel

	return Object.create(Model).init(data)
}
```

the different models (`AlbumModel`, `SingleModel` and `TrackModel`) are defined in other modules and all have their own idiosyncracies. however, we can create any one of them with a single call:

```js
ModelFactory('album', { /* data */ })
```

----

### lazy initialisation

as a simple example, we tweaked the loading of our track data so that, rather than loading every time, it checked to see if the data was already loaded to save redoing an expensive process (creating a new array and pushing elements to it) if it wasn't required.

articles on lazy loading:  
[How to Speed Up Lo-Dash ×100? Introducing Lazy Evaluation.](http://filimanjaro.com/blog/2014/introducing-lazy-evaluation/)

i think you can also do this with callbacks as below

### usage
```js
function (thing, callback) {
	if (thing) callback()
}
```

----

### generator/iterator pattern

##### usage

```js
// generator function
function* generator() {
	var num;
	while ( num < someValue )
		yield num++
}

var iterator = generator()

// iterator
iterator.next().value // eg. 1
iterator.next().value // eg. 2
iterator.next().value // eg. 3
```

useful links:  
[MDN Iterators and Generators](https://developer.mozilla.org/en/docs/Web/JavaScript/Guide/Iterators_and_Generators)
[Exploring ES6: Generators](http://exploringjs.com/es6/ch_generators.html)

----

### composite pattern

----

### promises

----
