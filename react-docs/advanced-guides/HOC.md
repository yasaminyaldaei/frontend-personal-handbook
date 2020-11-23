- **a HOC is a function that takes a component and returns a new component**
    - For reusing component logic
    - Not part of React API per se
    - HOC transforms a component into another component
        - **Whereas a component transforms props into UI**
```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```