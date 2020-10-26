---
id: cheatsheet
title: Cheatsheet
---

## Currying

- Currying is a fundamental tool in functional programming, a programming pattern that tries to **minimize the number of changes to a program’s state** (known as side effects) by using immutable data and pure (no side effects) functions.
- Currying is the process of taking a function with multiple arguments and returning a series of functions that take
  one argument and eventually resolve to a value.
- volume(2, 3, 4); // 24
- curried(2)(3)(4); // 24

## Hoisting

- Basically, when JavaScript compiles all of your code, all variable declarations using var are hoisted/lifted to the top of their functional/local scope (if declared inside a function) or to the top of their global scope (if declared outside of a function) regardless of where the actual declaration has been made. This is what we mean by “hoisting”
- Functions declarations are also hoisted, but these go to the very top, so will sit above all of the variable declaration`

## Call, Bind and Apply

- Call invokes the function and allows you to pass in arguments one by one.
- Apply invokes the function and allows you to pass in arguments as an array.
- Bind returns a new function, allowing you to pass in a this array and any number of arguments

## **new** Keyword

The new operator lets developers create an instance of a user-defined object type or of one of the built-in object types that has a constructor function. The new keyword does the following things:

1. Creates a blank, plain JavaScript object;
2. Links (sets the constructor of) this object to another object;
3. Passes the newly created object from Step 1 as the this context;
4. Returns this if the function doesn't return its own object.

## Worker

Web workers, service workers, and worklets are all scripts that run on a separate thread

- **Service workers** are a type of worker that serve the explicit purpose of being a **proxy between the browser and the
  network and/or cache**. navigator.serviceWorker.register('/service-worker.js');
- **Worklets** are hooks into the browser’s rendering pipeline, enabling us to have low-level access to the browser’s
  rendering processes such as styling and
- **Web workers** are general-purpose scripts that enable us to offload processor-intensive work from the main thread.

## Prototype and Inhertance

- **Prototype**: When a function is created in JavaScript, the JavaScript engine adds a prototype property to the function. This prototype property is an object (called as prototype object) which has a constructor property by default. The constructor property points back to the function on which prototype object is a property. We can access the function’s prototype property using functionName.prototype.

    <details>
      <summary>Example</summary>

  ```javascript
  function Animal(name, energy) {
    this.name = name;
    this.energy = energy;
  }

  Animal.prototype.eat = function (amount) {
    console.log(`${this.name} is eating.`);
    this.energy += amount;
  };

  Animal.prototype.sleep = function (length) {
    console.log(`${this.name} is sleeping.`);
    this.energy += length;
  };

  Animal.prototype.play = function (length) {
    console.log(`${this.name} is playing.`);
    this.energy -= length;
  };

  function Dog(name, energy, breed) {
    Animal.call(this, name, energy);

    this.breed = breed;
  }

  Dog.prototype = Object.create(Animal.prototype);

  Dog.prototype.bark = function () {
    console.log("Woof Woof!");
    this.energy -= 0.1;
  };

  Dog.prototype.constructor = Dog;
  ```

    </details>

- **Class Inheritance**: instances inherit from classes (like a blueprint — a description of the class), and create sub-class relationships: hierarchical class taxonomies. Instances are typically instantiated via constructor functions with the `new` keyword. Class inheritance may or may not use the `class` keyword from ES6.
- **Prototypal Inheritance**: A prototype is a working object instance. Objects inherit directly from other objects.
  Instances may be composed from many different source objects, allowing for easy selective inheritance and a flat [[Prototype]] delegation hierarchy.
- Instances are typically instantiated via factory functions, object literals, or Object.create().

## Memory Management

Low-level languages like C, have manual memory management primitives such as malloc() and free(). In contrast, JavaScript automatically allocates memory when objects are created and frees it when they are not used anymore (garbage collection). This automaticity is a potential source of confusion: it can give developers the false impression that they don't need to worry about memory management.

## Functional Programming

- **Pure functions**: A function is only pure if, given the same input, it will always produce the same output.
- **Function composition**: It is the process of combining two or more functions to produce a new function
- Avoid shared state
- Avoid mutating state
- Avoid side effects

## Closure

**Closure** is when a function “remembers” its lexical scope even when the function is executed outside that lexical scope.

```javascript
function Welcome(name) {
  var greetingInfo = function (message) {
    console.log(message + " " + name);
  };
  return greetingInfo;
}
var myFunction = Welcome("John");
myFunction("Welcome "); //Output: Welcome John
myFunction("Hello Mr."); //output: Hello Mr.John
```

## Cross-Origin Resource Sharing (CORS)

The Cross-Origin Resource Sharing standard works by adding new HTTP headers that let servers describe which origins are permitted to read that information from a web browser. Additionally, for HTTP request methods that can cause side-effects on server data (in particular, HTTP methods other than GET, or POST with certain MIME types), the specification mandates that browsers "preflight" the request, soliciting supported methods from the server with the HTTP OPTIONS request method, and then, upon "approval" from the server, sending the actual request. Servers can also inform clients whether "credentials" (such as Cookies and HTTP Authentication) should be sent with requests.

CORS failures result in errors, but for security reasons, specifics about the error are not available to JavaScript. All the code knows is that an error occurred. The only way to determine what specifically went wrong is to look at the browser's console for details.

## Storage

localStorage, sessionStorage, and cookies are all client storage solutions. Session data is held on the server where it remains under your direct control.

localStorage and sessionStorage are relatively new APIs (meaning, not all legacy browsers will support them) and are near identical (both in APIs and capabilities) with the sole exception of persistence. sessionStorage (as the name suggests) is only available for the duration of the browser session (and is deleted when the tab or window is closed) - it does, however, survive page reloads

Cookies are small strings of data that are stored directly in the browser. They are a part of HTTP protocol, defined by RFC 6265 specification.

Cookies are usually set by a web-server using response Set-Cookie HTTP-header. Then the browser automatically adds them to (almost) every request to the same domain using Cookie HTTP-header.

One of the most widespread use cases is authentication:

1. Upon sign in, the server uses Set-Cookie HTTP-header in the response to set a cookie with a unique “session identifier”.
2. Next time when the request is set to the same domain, the browser sends the cookie over the net using Cookie HTTP-header.
3. So the server knows who made the request

## Important Points

- In JavaScript, variables don't have types, values do.
- **Lexical scoping** (sometimes known as static scoping) is that sets the scope of a variable so that it may only be called (referenced) from within the block of code in which it is defined.

- **this**: A function's this references the execution context for that call, determined entirely by how the function was called.

- An **arrow function** doesn't define a this, so it's like any normal variable, and resolves lexically (aka "lexical this").

- **OLOO**: Objects Linked to Other Objects

- **Destructuring**: decomposing a structure into its individual parts.

- The **for...in** statement iterates over all non-Symbol, enumerable properties of an object.

- The **for...of** statement creates a loop iterating over iterable objects, including: built-in String, Array, Array-like objects (e.g., arguments or NodeList), TypedArray, Map, Set, and user-defined iterables.
- _Iterating_ means repeating some steps, while _enumerating_ means going through all values in a collection of values. So enumerating usually requires some form of iteration.

- **Partial application** produces functions of arbitrary number of arguments. The transformed function is different from the original — it needs less arguments.

- **Throttling** will delay executing a function. It will **reduce the notifications** of an event that fires multiple times.

- **Debouncing** will **bunch a series of sequential calls** to a function into a single call to that function. It ensures that one notification is made for an event that fires multiple times.

- A **promise** is an object that may produce a single value sometime in the future. A promise may be in one of 3 possible states: fulfilled, rejected, or pending.

- The **event loop** is a single-threaded loop that monitors the call stack and checks if there is any work to be done in the task queue. If the call stack is empty and there are callback functions in the task queue, a function is dequeued and pushed onto the call stack to be executed

- **Event Bubbling**: When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.

- **Event Capturing**: Reverse of Bubbling.

- **Event Delegation**: The idea is that if we have a lot of elements handled in a similar way, then instead of assigning a handler to each of them – we put a single handler on their common ancestor.

- The **Object.create()** method creates a new object, using an existing object as the prototype of the newly created object.

- `<script>` without any attributes does. The HTML file will be parsed until the script file is hit, at that point parsing will stop and a request will be made to fetch the file (if it’s external). The script will then be executed before parsing is resumed.

- `<script async>` downloads the file during HTML parsing and will pause the HTML parser to execute it when it has finished downloading.

- `<script defer>` downloads the file during HTML parsing and will only execute it after the parser has completed. defer
  scripts are also guaranteed to execute in the order that they appear in the document.

- A [**factory function**](https://medium.com/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1) is any function which is not a class or constructor that returns a (presumably new) object. `In JavaScript, any function can return an object. When it does so without the new keyword, it’s a factory function`

- The **Singleton Pattern** limits the number of instances of a particular object to just one.

- **Polyfill** A polyfill is a piece of JS code used to provide modern functionality on older browsers that do not natively support it.

- The **freeze()** method is used to freeze an object. Freezing an object does'nt allow adding new properties to an object,prevents from removing and prevents changing the enumerability, configurability, or writability of existing properties. i.e, It returns the passed object and does not create a frozen copy.

- The **Object.seal()** method is used seal an object, by preventing new properties from being added to it and marking all existing properties as non-configurable. But values of present properties can still be changed as long as they are writable.

- Javascript accessors: ECMAScript 5 introduced javascript object accessors or computed properties through getters and setters. Getters uses get keyword whereas Setters uses set keyword.

```javascript
var user = {
  firstName: "John",
  lastName : "Abraham",
  language : "en",
  get lang() {
    return this.language;
  }
  set lang(lang) {
  this.language = lang;
  }
};
console.log(user.lang); // getter access lang as en
user.lang = 'fr';
console.log(user.lang); // setter used to set lang as fr
```

- The **Object.preventExtensions()** method is used to prevent new properties from ever being added to an object. In other words, it prevents future extensions to the object

- Stopping bubbling
  -If an element has multiple event handlers on a single event, then even if one of them stops the bubbling, the other ones still execute.
  - In other words, event.stopPropagation() stops the move upwards, but on the current element all other handlers will run.
  - To stop the bubbling and prevent handlers on the current element from running, there’s a method event.stopImmediatePropagation(). After it no other handlers execute.
  - event.target – the deepest element that originated the event.
  - event.currentTarget (=this) – the current element that handles the event (the one that has the handler on it)
