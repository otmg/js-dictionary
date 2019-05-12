# 04. Function

 * [Definition](#definition)
 * [Defining](#defining)
 * [Arguments](#arguments)
 * [Lexical scope](#lexical-scope)
 * [Execution context](#execution-context)
 * [Lexical Environment](#lexical-environment)
 * [Closure](#closure)
 * [Recursion](#recursion)
 * [References](#references)

## Definition
A function is a piece of program wrapped in a value.
Executing a function is called **invoking**, **calling**, or **applying** it.
- Values given to functions are called `arguments`.
- Functions may also produce values. (Expression)

In JavaScript, functions are **first-class objects**, which means they can be:
-   stored in a variable, object, or array
-   passed as an argument to a function
-   returned from a function

## Defining
```js
function fn_name(*...args*) {
    // code block to be executed
}

const fn_name = function (*...args*) {
    // code block to be executed
}

// Arrow function
const fn_name = (*...args*) => {
    // code block to be executed
}
```

## Arguments
-   Optional arguments
  ```js
function power(base, exponent = 2) {
    let result = 1;
    for (let count = 0; count < exponent; count++) {
        result *= base;
    }
    return result;
}
console.log(power(4));  // --> 16
console.log(power(2, 6));   // --> 64
```

-   Rest parameters
```js
function max(...numbers) {
    let result = -Infinity;
    for (let number of numbers) {
        if (number > result) result = number;
    }
    return result;
}
console.log(max(4, 1, 9, -2));  // --> 9
```

- An Array-like object (arguments)
```js
function f() {
    return arguments;
}
console.log(f(1, 2, 3));
//      Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
//                     0: 1
//                     1: 2
//                     2: 3
//                     callee: ƒ f()
//                     length: 3
//                     Symbol(Symbol.iterator): ƒ values()
//                     __proto__: Object
```
## Lexical scope
A lexical scope or static scope in JavaScript refers to the accessibility of the variables, functions, and objects based on their physical location in the source code.
```js
let a = 'global';
function outer() {
    let b = 'outer';
    function inner() {
        let c = 'inner'
        console.log(c);   // prints 'inner'
        console.log(b);   // prints 'outer'
        console.log(a);   // prints 'global'
    }
    console.log(a);     // prints 'global'
    console.log(b);     // prints 'outer'
    inner();
}
outer();
console.log(a);         // prints 'global'
```

## Execution context
- An execution context is an abstract environment where the JavaScript code is evaluated and executed.
- When the global code is executed, it’s executed inside the global execution context,
and the function code is executed inside the function execution context.
- There can only be one currently running execution context (Because JavaScript is single threaded language)
- The call stack is a collection of execution contexts.

## Lexical Environment
- Every time the JavaScript engine creates an execution context to execute the function or global code,
it also creates a new lexical environment to store the variable defined in that function during the execution of that function.

- A lexical environment is a data structure that holds identifier-variable mapping.
(here identifier refers to the name of variables/functions,
and the variable is the reference to actual object [including function type object] or primitive value).

- A Lexical Environment has two components: 
    - The environment record is the actual place where the variable and function declarations are stored.
    - The reference to the outer environment means it has access to its outer (parent) lexical environment.
    This component is the most important in order to understand how closures work.
```js
// make sure you write it in global scope
let a = 'Hello World!';
function first() {
    let b = 25;  
    console.log('Inside first function');
}
first();    // execute the "first" function
console.log('Inside global execution context');

// //   the lexical environment for the global scope
//     globalLexicalEnvironment = {
//         environmentRecord: {
//             a     : 'Hello World!',
//             first : < reference to function object >
//         }
//         outer: null
//     }

// // the lexical environment for "first" function when executed
//     functionLexicalEnvironment = {
//         environmentRecord: {
//             b    : 25,
//         }
//         outer: <globalLexicalEnvironment>
//     }
```

## Closure
- This feature—being able to reference a specific instance of a local binding in an enclosing scope—is called closure.
- A closure is a function that has access to its outer function scope even after the outer function has returned. This means a closure can remember and access variables and arguments of its outer function even after the function has finished.
- When called, the function body sees the environment in which it was created, not the environment in which it is called.

```js
function multiplier(factor) {
    return function(number) {
        return number * factor;
    };
}
let twice = multiplier(2);
console.log(twice(5));  // --> 10
```

## Recursion
A function that calls itself is called recursive.
```js
function power(base, exponent) {
    if (exponent == 0) {
        return 1;
    } else {
        return base * power(base, exponent - 1);
    }
}

console.log(power(2, 3));   // 8
```

## References
https://blog.bitsrc.io/a-beginners-guide-to-closures-in-javascript-97d372284dda

https://medium.freecodecamp.org/discover-the-power-of-first-class-functions-fd0d7b599b69
