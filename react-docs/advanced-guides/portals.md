# Portals
- A first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.
    - >A programming language is said to have First-class functions when functions in that language are treated like any other variable. For example, in such a language, a function can be passed as an argument to other functions, can be returned by another function and can be assigned as a value to a variable.
- ```js
    ReactDOM.createPortal(child, container)
  ```
    -  `child` is a renderable React child.
        - Element, string, fragment
    - `container` is a DOM element.

## Usage

- Insert a child in a different location in DOM.

```js
return ReactDOM.createPortal(this.props.children, domNode)
```
- React doesn't create a new div and render `this.props.children` as this div's children.
- React add the `this.props.children` as `domNode`'s children.
    - `domNode` can be anywhere in the hierarchy.

- We use portals when we need a child to "break out" of its container.
    - dialogs, hovercards, tooltips.

- Accessibility concerns
    - Managing keyboard focus in modals.
    - Following ARIA modal authoring practices.
