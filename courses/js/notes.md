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
    trendRate.toString(); // "0"
    trendRate === 0; // true
    trendRate > 0; // false
    trendRate < 0; // false
    ```
    - `Object.is(variable, -0)` is the right way to check
    - `Math.sign()` returns "0" and "-0" instead of usual "1" and "-1" return value
    - isNegativeZero polyfill
    ```js
    function isNegZero(v) {
      return v == 0 && 1 / v == -Infinity;
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
        3 > 2 > 1; // => false
        // (3 > 2) > 1
        // (3 > 2) => true
        // true > 1 => 1 > 1 => false
        ```

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
  - `==` is for comparisons with known types.
    - Optionally for cases when coercion is helpful.
  - When the types differ, using `===` is pointless since it would always be `false`.
    - We can say that when the types are equal `===` is unnecessary as well.
  - One `==` is faster and more efficient than two or more `===` when **we know the types** and the types are different.
    - The single `==` is more preferrable.
    - The two or more `===` is distracting and harder to read.
  - When we don't know the types
    - Shows not understanding the code.
    - Refactor is preferable
    - Here `===` can be a signal of uncertainty when uncertainty is unavoidable.
      - It makes it obvious to the reader that the types are uncertain.
      - `===` should be reserved for these kind of cases.
    - Not knowing the types is equivalent to assume that coercion will happen (the worst case).
      - Here again `===` is a safe option.
    - We need to use the `===` when we don't want or can't use known and obvious types.
  - Making types known and obvious leads to better code.
    - In this case `==` is the best.
    - Otherwise, `===` is the fallback.

- TypeScript & Flow
  - Pros
    - They make the types more obvious
    - Communicates _intent_
    - Popular and well-documented
    - Sophisticated and good at what they do
  - Cons
    - Non-JS-standard
      - External standard
      - Ecosystem locking
        - We're locked into a non-standard ecosystem
    - Requires a build process
      - Complexity and pipeline
      - Barrier to entry
    - Intimidating for those without prior formal types experience
    - They focus on static types rather than _value types_
      - JS is not meant to have static containers (variables, etc)
- Scope

  - JS is not interpreted line by line and some sort of compiling is done
    - JS is a lexically scoped language
    - There's a communication between the execution engine and the scope manager when a variable is being assigned to a value.
      - When the variable is in target position
      - Executing of a function, puts that function in the source position
        - The scope manager provides the officially declared function in the compilation phase
          - **If there isn't any `type error: undefined is not a function` will occur**
          - The identifier might not be in the current scope, but available in the global scope
            - Like `console`
      - These are the look up process
        - The engine asks for identifiers and the scope manager provides the looked up identifiers in the bottom to the top manner
      - Compilation is a plan or roadmap for execution
        - Stuff don't exist yet in the compiler phase
        - The compiler output is not reserved in memory
        - We discover information but we use it in run time
        - With each execution the plan will be regenerated
  - JS organizes scopes with functions and blocks
  - When a global scope variable is a target reference inside a function scope, it gets updated not shadowed (shadowing meaning re-assigned)
  - When a non formally declared variable inside a scope is created (target reference), it will be auto declared by global scope.
    - So the variables should always be formally declared
    - Or strict mode should be used
      - **This makes the above scenario a "reference error"**
    - When an undeclared source reference is called in program, it always throws `ReferenceError`
  - A parameter formally creates a identifier inside the function scope
    - The assignments happens on the function call before the function executes.
  - undefines vs undeclared
    - A variable is there but doesn't have value at the time: undefined
    - There isn't any formally declared reference: undeclared
  - When a function declaration gets assigned to a variable, it doesn't get attached to the global scope
    - This is called function expression
    - Calling the function expression independently will throw `ReferenceError`
    - Anonymous function expression vs named function expression
      - The latter is more preferred.
        - Name produces a reliable self-reference (recursion, etc)
        - More debuggable stack traces
        - More self-documenting code
  - Lexical scope
    - JS is lexically scoped
    - Dynamic scope
      - Bash script is an example
      - In dynamic scope it matters when a function is called
        - The variables inside the function will be looked up around the nearest scope
  - Function scoping
    - The minimum amount of variables should be exposed
      - Everything should be private by default
      - This decreases the name collisions
      - Also it makes it easier to refactor later - Since it's not exposed to be used directly by others
  - IIFE pattern
    - Used to create a temporary scope and throw it away after being finished
    - Some special use case:
      ```js
      var teacher = (function getTeacher(teacher) {
        try {
          return fetchTeacher(1);
        } catch {
          return "Fallback";
        }
      })();
      ```
  - Block scoping
    - Using `let` and `const` with `{}` makes an implicit block scope
      - Unlike `var` that attaches itself to the function scope
      - Also `var` can be used more than once inside of a scope for the same variable name
    - For semantic and behavior reasons, `let` and `var` should exist together
  - `const`
    - `const` allows mutating a constant array item!
    - It should be used with immutable primitive values
    - `Object.freeze` can be used with objects and arrays to achieve some kind of immutability
  - Hoisting
    - A convention to describe the parsing
      - The compile step
    - It's not an actual JS spec thing
    - `var` hoisting is usually not useful and good
      ```js
      teacher = "sth";
      var teacher;
      ```
    - On the other hand function hoisting can be useful in semantic ways
      ```js
          // use at top:
          getSth()
          ...
          // declare at the bottom:
          function getSth() {
              //Get sth
          }
      ```
    - `let` hoists as well
      - `let` doesn't get initialized so the `TDZ` error is caught
        ```js
        {
          teacher = "Kyle";
          let teacher;
        }
        ```
        - `var` initializes to `undefined` when it's declared
        - `TDZ` is invented for `const`
          - `const` cannot be initialized to `undefined` at the compile time and then assigned to its eventual value at the run time
          - But even though `TDZ` is used for `let` as well
        - _The variables are created when their containing Lexical Environment is instantiated...When the `LexicalBinding` is evaluated not when the variable is created_
          - _13.3.1 Let and Const declarations_

- Closure

  - When a function remembers its lexical scope
    - **Even when the function is executed outside that lexical scope**

        ```js
        function ask(question) {
            setTimeout(function waitASec() {
            console.log(question);
            }, 100);
        }

        ask("What is Closure?");
        ```

    - The `question` is closed over
        - That's called closure
        - The ask function is executed and gone, but `question` is still available
        - Variables are closed over, not values
        - Meaning closure doesn't capture a history of values assigned to a variable
            - Just closes over the latest value at the time execution
            ```js
                for(var i = 1; i <= 3; i++) {
                    setTimeout(function() {
                        console.log(`i: ${i}`)
                    })
                }
                /*
                i: 4
                i: 4
                i: 4
                */
            ```
            - To fix this:
            ```js
                for(var i = 1; i <= 3; i++) {
                    let j = i
                    setTimeout(function() {
                        console.log(`i: ${j}`)
                    })
                }
                /*
                i: 4
                i: 4
                i: 4
                */
            ```
            - Also we can define `i` with `let`
                - This creates a new block scope variable in each iteration
- Modules
    - It's about the private and public properties
    - Module needs encapsulation
        - Hiding data
    - Modules encapsulate data and behavior together using closure
    - The purpose of module is keeping a state and control it with minimum exposure
    - ES6 modules
        - Everything that is exported is public, otherwise private
        - With `.mjs`
        