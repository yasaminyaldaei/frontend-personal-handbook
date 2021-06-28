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

## Event Bubbling Through Portals
- A portal still behaves like any other React child.
    - As the portal still exists in the React tree regardless of position in DOM tree
- Including Event Bubbling
    - _An event fired in a portal will be propagated to its ancestor components in React tree_
        - Even if those ancestors, are not parents in DOM tree

```html
<div>
    <div id="root"></div>
    <div id="modal"></div>
</div>
```
```js
class Parent extends React.Component {
    state = { count: 0}
    handleClick = () => {
        this.setState(({ count}) => ({
            count: count + 1
        }))
    }

    render() {
        return (
            <div onClick={this.handleClick}>
                <p>{this.state.count}</p>
                // A standard modal with `createPortal` and appended to the "modal" div of the DOM
                <Modal>
                    // A child with a button without click handler
                    <Child />
                </Modal>
            </div>
        )
    }
}
```

- `Child` is not a dom node in `root` DOM tree.
    - Also the button inside `Child` is not child of the `div` with the click handler.
- The child doesn't have click handler
    - The event bubbles up to the `Parent` component thus.
    - The state of `Parent` will change
- This procedure makes the state management more flexible
    - The React (not DOM) parent of the modal children will capture the events regardless of the modal (or similar component) implementation
