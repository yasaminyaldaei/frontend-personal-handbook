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