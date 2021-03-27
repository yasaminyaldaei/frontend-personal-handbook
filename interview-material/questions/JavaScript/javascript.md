## Table Of Contents:

- [Explain event delegation](#explain-event-delegation)
- [Explain how this works in JavaScript](#explain-how-this-works-in-javascript)
- [Can you give an example of one of the ways that working with this has changed in ES6?](#can-you-give-an-example-of-one-of-the-ways-that-working-with-this-has-changed-in-es6)

### Explain event delegation

- A technique to add event listeners to a parent element instead of adding them to the children
- The listener will fire whenever the event is triggered on the decendant elements due to **event bubbling up the DOM**
- Benefits:
  - Memory footprint goes down because of single handler being needed instead of multiple ones
  - No need to unbind handlers from the removed childrend and bind the event to new ones

### Explain how `this` works in JavaScript

`this` is determined based on these rules:

1. If `new` is used to call the function:
   - `this` will be an empty new object inside the function
2. If `apply`, `call` or `bind` are used to call/create a function:
   - `this` inside the function will be the object passed as an argument to those 3 functions.
3. If a function is called as a method property of an object:
   - `this` inside the function will be that object.
4. If a function call is a free function invocation, (just like: `fn()`):
   - `this` will be the global object
     - `window` in the case of browser
   - If in `strict mode`:
     - `this` will be `undefined`
5. If multiple of the above rules apply, the higher rule will set the `this`
6. If it is arrow function:
   - All the above rules are ignored and `this` will be determined at the time it is created.
     - We can check the line above the arrow function's creation, the `this` on that line will be the `this` in arrow function.

### Can you give an example of one of the ways that working with this has changed in ES6?

- Arrow function
    - Arrow function does not have its own bindings to `this` or `super`, and should not be used as methods.
    - arrow functions use enclosing scope
        - This prevents the caller from controlling context via `.call` or `.apply`

## Sources:

1. [h5b5/Front-end Developer Interview Questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)
2. [yangshun/front-end-interview-handbook](https://github.com/yangshun/front-end-interview-handbook)
