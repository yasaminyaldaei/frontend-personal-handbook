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
    


