## Table Of Contents:

- [Explain event delegation](#explain-event-delegation)
- [Explain how this works in JavaScript](#explain-how-this-works-in-javascript)
- [Can you give an example of one of the ways that working with this has changed in ES6?](#can-you-give-an-example-of-one-of-the-ways-that-working-with-this-has-changed-in-es6)
- [Explain how prototypal inheritance works
  ](#explain-how-prototypal-inheritance-works)

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

### Explain how prototypal inheritance works

- All Javascript objects have `__proto__` property
  - Which is a reference to another object
    - This object is called the initial object's "prototype
- When a property of object is called and is not present on that object, that property will be searched in `__proto__` object and the `__proto__` of `__proto__` and so on.

  - Until the property is found on one them or the Javascript engine reaches the end the of the chain
  - This may be called as "Inherihance" in Javascript, but it's more of a "Delegation" behavior.

- In ES5 `Object.create()` was introduced which creates a new object with the prototype of its first argument. This is how it works:

  ```
  if (typeof Object.create !== 'function') {
     Object.create = function(o) {
        function F() {}
        F.prototype = o
        return new F()
     }
  }
  ```

  - `Object.create` needs to be called in one of the following ways for the prototype methods to be inherited:
    - Object.create(Parent.prototype)
    - Object.create(new Parent(null))
    - Object.create(objLiteral)

- Example

```
const Parent = function () {
   this.name = "Parent"
}

Parent.prototype.greet = function () {
   console.log("Hello from parent")
}

const child = Object.create(Parent.prototype)

child.cry = function () {
   console.log("waaaaah!")
}

child.cry();
// Outputs: waaaaaahhhh!

child.greet();
// Outputs: hello from Parent

```

- In the above example, `child.constructor` is pointing to the `Parent`:

```
child.constructor
ƒ () {
  this.name = "Parent";
}
child.constructor.name
"Parent"
```

- One way to fix this is:

```
function Child () {
   Parent.call(this);
   this.name = "child";
}

Child.prototype = Parent.prototype;
Child.prototype.constructor = Child;

const c = new Child();

c.constructor.name // 'child'

```

## Sources:

1. [h5b5/Front-end Developer Interview Questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)
2. [yangshun/front-end-interview-handbook](https://github.com/yangshun/front-end-interview-handbook)
