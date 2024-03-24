## What is JavaScript

A high-level and versatile language that enables interactive and dynamic behavior on web pages.
- An interpreted/script language. JS engine interprets the code at runtime.
- A single-threaded language that one line of code is executed at a time. But it is possible to run 2 lines of code at the same time using asynchronous code.
- Runs in various environments, and each env provides its own global objects as the root context for JS execution, such as the the `window` object in web browser. When define a global variable or functioavan, it is defining a property of the `window` object.

## Data
`parse.JSON()`

### variables
- **declaration**
	- `var`: function-scoped, global variable
	- `let`: block-scoped, allows for reassignment
	- `const`: block-scoped, creates a read-only reference
- **hoisting**: variables and function declarations are moved to the top of their scope before code execution.
- **assignment**: 
	- for primitive types, reference is copied by value. A new variable creates a new value.
	- for non-primitive, reference is not copied by value. Variables all point to the same value.
- **binding:**
	- explicit binding: `this`
- `null` v.s. `undefined`: `null` is an assignment value, and can be assigned to a variable as a representation of no value. `undefined` is a primitive value that represents the absence of a value, or a variable that has not been assigned a value.

### type
#### primitive types
- `string`: template literals
- `type casting`: from one data type from another. 
	- Implicit: the compiler (for compiled languages) or runtime (for script languages like JS) automatically converts data types.
#### non-primitive types (objects)
make an object immutable: `Object.freeze()`
##### Core JS objects
- **Array**: Used to store multiple values in a single variable.
	- Find unique values in an array: `Set`, `filter`, `reduce`, `forEach`, `for...of`
- **Date**: Represents dates and times.
- **RegExp**: Represents regular expressions.
- **Promise**: a value that may not be available yet but will be at some point, or the eventual completion/failure of an asynchronous operation and its resulting value.
	- `Promise.all()`: rejects immediately if any of the promises reject.
	- `Promise.allSettled()`: waits for all of the promises to settle (either resolve or reject) and then returns the result.
	- error-handling
		- reject a Promise: `reject(new Error('Something went wrong'))`
		- catch method: `promise.then().catch((error) => console.log(error))`
		- second argument of the `then` method: `promise.then((result) => ..., error => ...)
	- `finally`: executed when the promise is resolved or rejected. `promise.then().catch().finally(() => console.log('Finally Promise has settled.))`
	- callback hell: 层层嵌套的callback function

##### Web API objects
- **Document**: Represents any web page loaded in browser and serves as an entry point into the web page's content (the DOM). The Document Object Model is a programming interface for HTML and XML documents. It represents the document as nodes and objects so that programs can change the document structure, style, and content.
	- create a new element: `createElement`, `setAttributes`
	- select elements: `querySelector`, `querySelectorAll
	- add new elements: `appendChild`, `insertBefore`
	- remove an element: `removeChild`
	- get the dimensions of element: `getBoundingClientRect` => `DOMRect { x: 8, y: 8, width: 784, height: 784, top: 8, right: 792, bottom: 792, left: 8 }
- **Console**: Provides access to the browser's debugging console.
- **LocalStorage** and **SessionStorage**: Provide mechanisms to store data on the client side.
- **XMLHttpRequest** and **Fetch API**: Enable making HTTP requests to servers from web pages.

##### Browser objects
- **global/window**: an open window in the browser and serves as the global execution environment for JS code running in that window. It's a part of the Browser Object Model (BOM).
	- `window.innerWidth/innerHeight`: get the viewpoint dimensions.
	- `window.scrollTo(0, 0)`: scroll to the top of the page.
	- `alert`: displays an alert box with a specified message and an OK button.
	- `prompt`: displays a dialog box that prompts the visitor for input.
	- `confirm`: displays a dialog box with a specified message, along with an OK and a Cancel button.

##### Node.js Env objects
 Node.js is a JavaScript runtime built on Chrome's V8 JS engine. => Run JS outside the browser
- **global**: Serves a similar purpose to the `window` object in browsers.
- **process**: Provides information about, and control over, the current Node.js process.
- **Buffer**: Used to handle binary data.

### prototype chain
The chain of objects linked by their prototypes. 
When access a property or method on an object, JS first checks the object itself. If find nothing, looks up the object's prototype. This process continues until the property or method is found or the end of the chain is reached (typically the prototype of the base object, which is `null`). The prototype chain allows objects to inherit properties and methods from other objects.

### garbage collection
JS automatically manages memory by freeing up space used by objects no longer needed. This algorithm is called Mark and Sweep, which is performed periodically by the JS engine.

### structure
- Map: A collection of key/value pairs and remembers the original insertion order of the keys.
- WeakMap: A collection of key/value pairs in which the keys are weakly referenced.
	- Key/value can be any data type in `Map` object while `WeakMap` can only use object as keys. `WeakMap` is not iterable whereas `Map` is, and holds the weak reference to the original object, so if there are no other references to an object stored in the `WeakMap`, those objects can be garbage collected.
- Set: Similar to `Array` but the values are unique. It is a collection of elements where each element is stored as a value without any keys.
- Heap: a large, mostly unstructured region of memory, store `objects`, `arrays`, and `functions`. 
- Stack: a small, organized region of memory, store primitive values, function calls, local variables, and references to the address of items stored in Heap, and follows a "Last In, First Out" (LIFO) order. 

## Scope
A scope is a set of variables, objects, and functions that you have access to. 
There are Global Scope, Function Scope (Local Scope), and Block Scope.

## Operators
- comma operator: evaluates each of its operands (from left to right) and returns the value of the last operand.
- equal operator: strict equality === only considers values equal that have the same type. == converts the operands if they are not of the same type, then applies strict comparison.
- logical operators
	- OR ||
	- AND &&
	- NOT 
	- Nullish Coalescing ??
		- `data ??= []`, means `if (data === null || data === undefined) { data = []; }`
- increment operator: pre-increment ++x, post-increment x++
### ternary operator
```js
condition ? true : false
```

### spread operator
Make the elements of an array or properties of an object expand or "spread" into individual elements or properties.


## Statements
- `switch` case: evaluates an expression, matching the expression's value to a `case` clause, and executes statements.
- loop
	- `do...while`: repeat the loop until the condition is `false`.
	- `while`
	- `for`
- label statements: prefix a label to an identifier. It can be used with `break` and `continue` statement to control the flow more precisely.
- strict mode: `use strict`

## Functions
- IIFE(Immediately Invoked Function Expression): JS function that runs as soon as it is defined. It is frequently used to create a new scope to avoid variable hoisting from within blocks.
- **closure**: a function that has access to its outer function scope even after the outer function has returned.
- arrow function: arrow functions do not have their own `this`. Instead, they inherit the `this` of the enclosing lexical scope.
- **map**: creates a new array with the results of executing a provided function on every element of the calling array.
- `forEach`: executes provided function once for each element, doesn't create a new array.
- **reduce**: results in a single value with the results of executing a reducer function on each element of the array.
	- feel it like an iterator method and remind me of useReducer? currying
- `setInterval`: accepts a function and a time interval in milliseconds, and returns a unique id for clearing the interval using `clearInterval`. `clearInterval(intervalId)`
- `setTimeout`: run a piece of code after a certain time. `clearTimeout(timeoutId)`

## Async / await
An `async` function always returns a promise, and within such a function, can use `await` to pause execution until a promise settles. `async` code does not block the execution of the program while `sync` code does.
- defer and async attribute
	- defer: 同时download，但仍然按顺序执行
	- async: 同时download，谁先download好，谁就先执行

### error-handling
try/catch statement


## Event handling
### event loop
a loop that constantly checks if there are any tasks to be executed. If yes, execute them. If no, wait for new tasks to arrive.
- Callback Stack: a data structure that stores the tasks that need to be executed. LIFO (Last In, First Out) data structure
- Callback Queue: a data structure that stores the tasks that have been completed and are ready to be executed. FIFO (First In, First Out) data structure.
- Workflow: 
	- Executes tasks from the Call Stack.
	- For an async task, such as a timer, it runs in the background. JS proceeds to the next task without waiting.
	- When the async task concludes, its callback function is added to the Callback Queue.
	- If the Call Stack is empty and there are tasks in the Callback Queue, the Event Loop transfers the first task from the Queue to the Call Stack for execution.

### **event bubbling**
events propagate/bubble up through the hierarchy of nested elements in the DOM.
- `stopPropagation`: prevent an event from bubbling up the DOM tree.

### event propagation: 
- event capturing/trickling: the first phase of event propagation. The event is captured by the outermost element and propagated to the inner elements. It is the opposite of event bubbling.
- `event.preventDefault()`: prevent the default action of an event.
### sub-sup model
### create a custom event
- create, listen, remove: `const event = new CustomEvent('roadmap-updated', { detail: { name: 'JavaScript' },})`, `addEventListner`, `removeEventListner`

## Debug
- console.log
- browser developer tools: source, console, network
- breakpoint
- `debugger` statement
- call stack and scope

## ES6
[[ES6]], or ECMAScript 2015, represents a specific version of the ECMAScript standard that JavaScript follows.

## Class
- inheritance: create a new `Class` from an existing `Class`