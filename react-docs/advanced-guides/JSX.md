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

## Props in JSX

### Javascript expressions as Props

- Any kind of Javascript expressions can be used as props inside `{}` and the value would evaluated base on that

- Statements such as `if` and `for` can not be used inside JSX tags and should be put in the surrounding code.

### String Literals

- String literals can be used without brackets
- The value is HTML-unescaped

### Props default to "true"

- A prop without a value is considered as `true` 
- It can be confused with ES6 object shorthand (`{foo} // -> {foo: foo}`), therefor it's not recommended.

### Spread attributes

- If `props` object is already available it can be passed as `{...props}` to the inner tag as a whole

- Specific props can be extracted using object destructing:

```
const {specific, ...rest} = this.props;

... return(
  <tag {...rest} />
)
```
- Unnecessary props may be passed using this 
- Invalid HTML attributes may be passed to the DOM

## Children in JSX

- The content between the opening and closing tags are passed as a special prop called `children`

### String Literals
- Strings can be passed as children
- The `props.children` will be that string
- HTML is unescaped
- Whitespaces, blank lines and new lines are removed by JSX. Thus these are the same:
  ```
  <div>Hello World</div>

<div>
  Hello World
</div>

<div>
  Hello
  World
</div>

<div>

  Hello World
</div>
```

### Components and tags
- Other components can be used as children between opening and closing tags
- Different types of children (string literals, tags, components, etc) can be mixed together like in HTML
- **A React component can return an array of elements**:
```
return [
    // Don't forget the keys :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
```

### Expressions
- Any JavaScript expression can be passed as JSX children, enclosed in `{}`
  - Mainly useful for rendering a list of elements:
```
  return (
    <>
    {array.map(item => <Item item={item}/>)}
    </>
  )
```
- Expressions can be mixed with other types of children as well.
  - String templates alternative

### Functions

- Expressions inside JSX usually evaluate into strings and/or React elements.
- But they can also be functions or any other kind of output in the case of custom components.
  - This callback function can be called inside the component as `props.children(args)`
- This approach will work as long as they will eventually change into sth React understands inside that component.

### Booleans, null, and undefined are all ignored

- These values are valid JSX children
- But They won't render anything

```
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

- This functionality is useful for **conditional rendering**

```
  {showSth && <Some Component>}
```

- **Be aware of falsy values which aren't any of listed above**
  - Because `0 && anything` will be evaluated into 0 in JS, since 0 is falsy
    > When the value to `&&` operator's left is something that converts to false, it returns that value, and otherwise it returns the value on its right.
    _Eloquent JS_
  - To solve this we should make sure that the left side is always a Boolean
    - Or just abandon this method and use ternaries! ([read more](https://kentcdodds.com/blog/use-ternaries-rather-than-and-and-in-jsx))

- If we need to display one of the above values, we need to convert it to string (`String(variable)`)


  



