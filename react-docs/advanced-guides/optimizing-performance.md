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
