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

- `ToNumber()` and `ToString()` are abstract operations done internally to achieve `ToPrimitive()`, alognside `ToBoolean`
    - These are procedures used in different methods (explicitly or implicitly), not methods themselves
    - These can happen while coercion
    - `ToString(-0)` => 0
    - `ToString(object)` => **`[object object]`**
    - `ToNumber("")` => 0
        - Root of all coercion evil!
        - Empty string with white spaces is still 0 because in the process the leading and trailing white spaces are eliminated first
    - `ToNumber(-0)` => -0
    - `ToNumber(null)` => 0
    - `ToNumber(undefined)` => `Nan`
    - `ToBoolean` only checks the falsy values and decided if the value is in there or not
        - `ToBoolean(true)` => 1
            - Bad idea!
                ```js
                3 > 2 > 1 // => false
                // (3 > 2) > 1 
                // (3 > 2) => true
                // true > 1 => 1 > 1 => false
                

- `Boolean(object)`
    `Boolean(null)` => false
    `Boolean(undefined)` => false
    `Boolean({})` => true
    - Useful kind of implicit coercion and condition
        - `if (myObject) { // Do sth}`

- A string (or any primitive type) has access to object prototype methods
    - This is a form of implicit coercion
    - This is called **Boxing**

- Both `==` and `===` check the type
    - **When the types match `==` does the `===` operation.**
    - When the types mismatch, `===` always returns `false`.
    - spec explitly says that `===` returns `false` for `NaN` and `true` for `-0` and `+0` comparison.
    - `==` allows coercion when types differ but `===` doesn't.
        - We should decide whether we want coercion or not.
        - When we choose `===` all the time, it shows that we're not certain about our code and result.
    - **`null == undefined //true`**
    - `null === undefined //false`
        - In this case `==` is more convenient.
        - `===` would require lots of strict checks for no proper benefit.
    - **`==` prefers `ToNumber`**
        - If we have two inputs, either number or string, and we're sure one of them is absolutely number, `==` is more convenient.
            - `===` would require `Number` to check.
            - If one both of them strings, they should be absolutely the same.
    - **`==` tries `ToPrimitive` and if doesn't work, it would be `false`.**
        - `ToPrimitive` on array stringifies it.
            - When array has one element, coercion is bad.
                - `42 == [42] //true`
                    - The answer is not `===`, but it is better structuring and sensible comparison.
    - `[] == ![] //true`
        - Corner case
        - Not sensible comparison
        - **`[]` is truthy** => `![]` would become `false`
        - `[] == false` => Non-primitive comparions => Toprimitive.
            - `"" == false`. => Number preference.
                - `0 == false`
                - `0 == 0` => `true`
    - `[]` is truthy but `[] == false` 
        - The coercion corner cases makes this.
            - **[] => Non-primitive => ToPrimitive => Stringify => "" => 0 => 0 is falsy**
    - When to avoid `==`
        - when either side might be 0 or ""
        - when either side is non-primitive
        - when either side is `true` or `false`
    - `==` is great for coercion-based comparison between primitive values.





    