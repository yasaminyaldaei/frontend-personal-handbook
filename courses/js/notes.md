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

    - `BigInt` has its own type: "bigint"
        - It's not IEEE number
        - Doesn't work well with numbers, completely different

- `undeclared` vs `undefined` vs `uninitialized`
    - Synonym in human lang but not in programming
    - Problem is JS:
        - Getting `undefined` for a variable that doesn't even exist
        - Should've returned `undeclared`
    - `undeclared` never been created, is not there at all at any scope
    - `undefined` is there (created) but doesn't have value at the moment 
    - `uninitialized` is there but it's not initialized and should'nt be touched
        - It would cause error which is `TDZ`
            - Temporal dead zone

- `===` lies in two cases!

    - `NaN`
        - Invalid number rather than "not a number"
        - `NaN` === `NaN` : `false`
        - `isNaN("string")` => `true`
            - because of coercion
        - `Number.isNaN("string")` : `false`
        - `typeof NaN` => "number"
        - isNan polyfill
        ```js
        function isNan(v) {
            return v != v;
        }
        ```

    - Negative Zero
        ```js
            var trendRate = -0;
            trendRate.toString() // "0"
            trendRate === 0 // true
            trendRate > 0 // false
            trendRate < 0 // false
        ```
        - `Object.is(variable, -0)` is the right way to check
        - `Math.sign()` returns "0" and "-0" instead of usual "1" and "-1" return value
        - isNegativeZero polyfill
        ```js
        function isNegZero(v) {
            return v == 0 && (1 / v) == -Infinity
        }
        ```
- `String()`, `Boolean()`, `Number()` are not supposed to be used with `new` 
    - The output already exists as a type and these utilities are great to output the value directly

    