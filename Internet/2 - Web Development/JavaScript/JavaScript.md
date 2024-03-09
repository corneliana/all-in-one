## What is JavaScript

A high-level and versatile language primarily known for its role in web development. It enables interactive and dynamic behavior on websites.

JS is a single-threaded language, which can only execute one line of code at a time. But it is possible to run 2 lines of code at the same time using asynchronous code.

JS runs in various environments, and each environment provides its own global objects that serve as the root context for JavaScript execution. In the web browser, the `window` object is the global context. The global variables and functions are properties of the `window` object. When define a global variable or function, you're actually defining a property of the `window` object.

## Data
variables:
hoisting: variables and function declarations are moved to the top of their scope before code execution.

### type
- string: template literals
- type casting: from one data type from another. Implicit conversion happens when the compiler (for compiled languages) or runtime (for script languages like `JavaScript`) automatically converts data types.

### objects
#### Core JS objects
- **Array**: Used to store multiple values in a single variable.
- **Date**: Represents dates and times.
- **RegExp**: Represents regular expressions.
- **Promise**: Represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

#### Web API objects
- **Document**: Represents any web page loaded in the browser and serves as an entry point into the web page's content (the DOM).
	- The Document Object Model is a programming interface for HTML and XML documents. It represents the document as nodes and objects so that programs can change the document structure, style, and content.
- **Console**: Provides access to the browser's debugging console.
- **LocalStorage** and **SessionStorage**: Provide mechanisms to store data on the client side.
- **XMLHttpRequest** and **Fetch API**: Enable making HTTP requests to servers from web pages.

### structure
- Map: A collection of key/value pairs and remembers the original insertion order of the keys.
- WeakMap: A collection of key/value pairs in which the keys are weakly referenced.
	- Key/value can be any data type in `Map` object while `WeakMap` can only use object as keys. `WeakMap` is not iterable whereas `Map` is, and holds the weak reference to the original object, so if there are no other references to an object stored in the `WeakMap`, those objects can be garbage collected.
- Set: Similar to `Array` but the values are unique. It is a collection of elements where each element is stored as a value without any keys.


## Operators
### logical operators
- OR ||
- AND &&
- NOT !
- Nullish Coalescing ??
	- `body.data.relatedParties ??= []`
	```Javascript
if (data === null || data === undefined) {
    data = [];
}
	```

### ternary operator
```js
condition ? true : false
```

### spread operator
Make the elements of an array or properties of an object expand or "spread" into individual elements or properties.


## Statements
- label statements: prefix a label to an identifier. It can be used with `break` and `continue` statement to control the flow more precisely.


## Functions
- **map**: creates a new array with the results of executing a provided function on every element in the calling array.
- **reduce**: results in a single value with the results of executing a reducer function on each element of the array.
	- feel it like an iterator method and remind me of useReducer? currying
```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const sum = numbers.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 0);

console.log(numbers); // [1, 2, 3, 4, 5, 6]
console.log(sum); // 21
```


## Async / await
### error-handling
try/catch statement


## ES6
[[ES6]], or ECMAScript 2015, represents a specific version of the ECMAScript standard that JavaScript follows.






## Class
- inheritance: create a new `Class` from an existing `Class`
- 