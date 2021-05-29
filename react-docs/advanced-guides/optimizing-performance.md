# Optimizing Performance

React has several internal techniques to minimize costly DOM operations and usually no personal techniques are needed.
But these are the essential techniques that need to be used:

## Use The Production Build

- In production build the React warnings useful for development are gone, since they would have made it larger and slower.

### how-to

- CRA: `npm run build`
- Single-file builds: `.production.min.js` cdn files

## Use React Profiler

- `react-dom` 16.5+ provides enhanced profiling capabilities in DEV mode using the DevTools profiler plugin.
- This plugin uses React’s experimental Profiler API to collect timing information about each component that’s rendered in order to identify performance bottlenecks in React applications.

## Virtualized Long Lists

- For long list (hundreds or thousands of rows) **windowing** technique is recommended.
  - This technique only renders a small subset of rows at any given time.
  - Decreases the time it takes to rerender.
  - Decreases the DOM nodes created.
- Use `react-virtualized` and `react-window` libraries
  - The latter is a lite version of the former.

## Avoid Reconciliation

- React has internal representation of UI
  - This helps React avoid creating and accessing DOM nodes beyond necessity
  - Since these operations are slower than JS operations
  - This is called **Virtual DOM**
- When a state or prop changes, React decides whether an actual DOM change is needed or not using the virtual DOM
  - If the newly returned element is not equal to the previously rendered one, React will update the DOM.
- Even though only the changed DOM nodes are updated, the rerendering takes time.
  - This is usually alright.
  - If it causes noticable slowness, `shouldComponentUpdate` lifecycle method should be overwrited.
    - Is triggered before the rerendering process starts. - The default:
    ```js
     shouldComponentUpdate(nextProps, nextState) {
         return true;
     }
    ```
- We can `return false` to prevent the whole rendering process.
  - Including the `render()` method on this component and below.
- The same effect can be achieved using the `React.PureComponent`
  - It is equivalent to implementing `shouldComponentUpdate()` with a shallow comparison of current and previous props and state.

## Should Component Update in action
- Different scenarios may happen while updating:
  - React starts check top to bottom.
  - It finds out that `shouldComponentUpdate` is true for root.
  - It starts to go deeper.
  - If `SCU` is false for a subtree, it decides that subtree doesn't need reconciliation and moves on.
  - If `SCU` is true, then "is virtual DOM elements are equal" check starts.
  - Also the children of the subtree are also checked for the `SCU` and `vDOMEq`.
  - If both are true, that node and all the parents need reconciliation.
  - If `SCU` is true but `vDOMEq` is not, React doesn't change the DOM and only updates the component state. (thus no reconciliation is needed).
    - **Reconciliation only happens when `shouldComponentUpdate` is true and virtual DOM elements are not equal to the previous ones.**

### `shouldComponentUpdate` examples
- The manual way would be checking the `props` and `state` values one by one:
```js
shouldComponentUpdate(nextProps, nextState) {
  if (nextProps.someProp !== this.props.someProp) {
    return true;
  }
  if (nextState.someState !== this.state.someState) {
    return true;
  }

  return false;
}
```
- To achieve the same effect using a built-in feature, we can use `React.PureComponent`