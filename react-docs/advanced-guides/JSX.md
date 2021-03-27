# JSX In Depth

- JSX just provides syntactic sugar for the:
`React.createElement(component, props, chilren)`

## Specifying the React element type

- The first part of a JSX tag determines the type of the React element
    - Capitalized types indicate that the JSX tag is referring to a React component.
        - These tags get compiled into a direct reference to the named variable.
        - **So the variable should be in scope**

### React must be in scope

- **Since JSX compiles into calls to `React.createElement`, the `React` library must also always be in scope from our JSX code.**
- If a Javascript bundler is not uses and React is loaded via a `<script>` tag, it is already in scope as the `React` global.