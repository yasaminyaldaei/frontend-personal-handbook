- **a HOC is a function that takes a component and returns a new component**
  - For reusing component logic
  - Not part of React API per se
  - HOC transforms a component into another component
    - **Whereas a component transforms props into UI**

```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

## Use HOC's For Cross-Cutting Concerns

- In some use cases, a particular pattern will occur over and over again
- We would want an abstraction that allows us to define this logic in a single place and share it across many components
- This is where HOCs excel
- We can write a function that creates components (A HOC)
  - This function does mutual logic
  - This function will accept as one of its arguments a child component that receives the result data as a prop
- **A HOC doesn't modify the input component, nor does it use inheritance to copy its behavior**
  - It composes the original component by wrapping it in a container component
  - HOC is a pure component with zero side-effects
  - The HOC isn't concerned with why and how the data is used, and the wrapped component isn't concerned with where the data came from
- Like the components, the contract between the HOC function and the wrapped component is entirely **prop-based**
  - This makes it easy to swap one HOC for a different one as long as they provide the same props

## Don't Mutate the Original Component

```
function logProps(InputComponent) {
  InputComponent.prototype.componentDidUpdate = function(prevProps) {
    console.log('Current props: ', this.props);
    console.log('Previous props: ', prevProps);
  };
  // The fact that we're returning the original input is a hint that it has
  // been mutated.
  return InputComponent;
}

// EnhancedComponent will log whenever props are received
const EnhancedComponent = logProps(InputComponent);
```

- The problems of the above code:
  - The input component cannot be resued separately from the enhanced component
  - If we apply another HOC to `EnhancedComponent`, that _also_ mutates the `componentDidUpdate`
    - Meaning the first HOC's functionality will be overridden
  - The HOC also won't work with function components
    - Because of the lack of lifecycle methods
- **Mutating HOCs are a leaky abstraction**
  - The consumer must know how they are implemented in order to avoid conflicts
- We should use **composition** in HOC
  - Avoiding the potential for clashes
  - Works equally well with class and function component
  - It's a pure function
    - Composable with other HOCs and with itself

## Convention: Pass unrelated props through to the wrapped component

- HOCs **add** features, they shouldn't alter the flow too much
  - It's expected that the input component has a **similar interface** to the wrapped component.

```
render() {
  // Filter out extra props that are specific to this HOC and shouldn't be
  // passed through
  const { extraProp, ...passThroughProps } = this.props;

  // Inject props into the wrapped component. These are usually state values or
  // instance methods.
  const injectedProp = someStateOrInstanceMethod;

  // Pass props to wrapped component
  return (
    <WrappedComponent
      injectedProp={injectedProp}
      {...passThroughProps}
    />
  );
}
```

## Convention: Composing single-argument HOCs

- Some HOCs accept only a single argument, **the wrapped component**
- The most common signature for HOC's looks like this:

  - `connect` is a higher-order function that returns another function
  - The returned function is a HOC, which returns a component that is connected to the Redux store

```
// React Redux's `connect`
const ConnectedComment = connect(commentSelector, commentActions)(CommentList);
```

- The benefit of this convention is the easier composition
  - Single-argument HOCs like the one returned by the `connect` have the signature `Component => Component`
  - Functions whose output type is the same as its input type are really easy to compose together
  - **This same property also allows enhancer-style HOCs to be used as decorators**

```
// Instead of doing this...
const EnhancedComponent = withRouter(connect(commentSelector)(WrappedComponent))

// ... you can use a function composition utility
// compose(f, g, h) is the same as (...args) => f(g(h(...args)))
const enhance = compose(
  // These are both single-argument HOCs
  withRouter,
  connect(commentSelector)
)
const EnhancedComponent = enhance(WrappedComponent)
```

## Convention: Display Name

- For easy debugging, it's common to choose a display name that communicates that it's the result of a HOC

```
WithSubscription.displayName = `WithSubscription(${getDisplayName(WrappedComponent)})`;
  return WithSubscription;
}

function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
```

## Caveats

### Using HOC inside render method

- React's diffing algorithm uses component identity to determine whether it should update the existing subtree or throw it away and mount new one
- If the component returned from render is identical (===) to the component from the previous render, React recursively updates the subtree by diffing it with the new one. If they’re not equal, the previous subtree is unmounted completely.

- Using HOC inside the render method causes the latter to happen

- This affects performance and state of that component and its children (they would be lost)

- Applying HOC outside the component definition makes it being created only once, thus its identity being consistent across renders

- When it's really need to apply HOC dynamically, it can be done inside the lifecycle methods or the constructor

```
render() {
  // A new version of EnhancedComponent is created on every render
  // EnhancedComponent1 !== EnhancedComponent2
  const EnhancedComponent = enhance(MyComponent);
  // That causes the entire subtree to unmount/remount each time!
  return <EnhancedComponent />;
}
```
