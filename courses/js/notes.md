- The difference between `x++` and `++x` 
    - JS first type casts and then operates the `++`
        - plus plus uses `Number` method at the begining
    
- Everything is not objects in JS
    - Most primitive types are not objects
- In JS as a dynamic lang, the variables don't have types, the values have types
- `typeof` returns a string containing the type
    - Object subtypes still return "object"
    - `null` variable returns `object`
        - Historically `null` has been used to unset the objects (`undefined` was used to unset other types)
        ```
        var v = null;
        typeof v; // "object"
        ```
        - `function` is not primitive type, but it has it's own type in `typeof` response
        - `array` returns `object`
            - Other tools should be used, like `isArray`