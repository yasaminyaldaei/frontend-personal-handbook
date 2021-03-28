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

### Using dot notation for JSX type

- If a single module exports many React components, those can be referred using dot-notation from within JSX. Such as:

```
...
return <MyComponents.DatePicker color="blue" />;
```

### User-defined components must be capitalized

- When an element starts with lowercase letter, it refers to a built-in HTML tag

- Types that start with uppercase letters, refer to user-built and defined or imported components

    - Therefor upper-case letters should be used for components.

### Choosing the type at runtime

- A general expresssion can not be used as React element type.
- When we need to assign a dynamic component, first we need to assign it to a capitalized variable.

```
...
 // Wrong! JSX type can't be an expression.
  return <components[props.storyType] story={props.story} />;
// =>
 // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```