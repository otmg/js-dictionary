# 03. Program structure

## Expressions and Statements
-	A fragment of code that produces a value is called an expression.
-	An expression corresponds to a sentence fragment
-	A JavaScript statement corresponds to a full sentence.
-	A program is a list of statements.

## Binding / Variables
-	To catch and hold values, JavaScript provides a thing called a binding, or variable
```js
    let const   var

    var a = 1;
    var a = 2;  // this's okay
    let a = 3;  // Uncaught SyntaxError: Identifier 'a' has already been declared
    const c;    // Uncaught SyntaxError: Missing initializer in const declaration

    const b = 12;   // primitive value
    b = 5;  // Uncaught TypeError: Assignment to constant variable.
```

## Scope
Bindings declared with **let** and **const** are in fact **local** to the block that they are declared in, so if you create one of those inside of a loop, the code before and after the loop cannot “see” it.
```js
	Ex 1:
        const halve = function(n) {
            return n / 2;   //  local binding
        };
        let n = 10; // global binding
        console.log(halve(100));    // --> 50
        console.log(n); // --> 10

    Ex 2:
        let x = 10;
        if (true) {
            let y = 20;
            var z = 30;
            console.log(x + y + z); // --> 60
        }
        // y is not visible here
        console.log(x + z); // --> 40
```

## The environment
The collection of bindings and their values that exist at a given time is called the environment. When a program starts up, this environment is not empty. It always contains bindings that are part of the language standard, and most of the time,it also has bindings that provide ways to interact with the surrounding system. For example, in a browser, there are functions to interact with the currently loaded website and to read mouse and keyboard input.

## Flow of control
-	Loops
```js
	while (*condition*) {
        // code block to be executed
    }

    do {
        // code block to be executed
    } while (*condition*) 

    for (statement 1; statement 2; statement 3) {
        // code block to be executed
    }

    Note: break;    // Breaking out of loop
            continue;  // Continue looping
```

-	Condition execution
```js
    if (*condition*) {
        // code block to be executed // true
    } else {
        // code block to be executed // false
    }
```

-	Switch case
```js
	switch (*condition*) {
        case *value1*:
            // code block to be executed
            break;
        case *value2*:
            // code block to be executed
            break;
        default:
            // code block to be executed
            break;
    }
```

## Hoisting
```js
    var x = 5;
    function run() {
        console.log(x);   // output --> undefined
        //
        var x = 12;
    }
    run();

    var x = 5;
    function run() {
        console.log(x);
        //
        let x = 12;
    }
    run();  // Uncaught ReferenceError: x is not defined
```

## Side Effects
-	A side effect is any application state change that is observable outside the called function other than its return value. Side effects include:
    -	Modifying any external variable or object property (e.g., a global variable, or a variable in the parent function scope chain)
    -	Logging to the console
    -	Writing to the screen
    -	Writing to a file
    -	Writing to the network
    -	Triggering any external process
    -	Calling any other functions with side-effects

-	Side effects are mostly avoided in functional programming, which makes the effects of a program much easier to understand, and much easier to test.

-	Haskell and other functional languages frequently isolate and encapsulate side effects from pure functions using monads. The topic of monads is deep enough to write a book on, so we’ll save that for later.

-	What you do need to know right now is that side-effect actions need to be isolated from the rest of your software. If you keep your side effects separate from the rest of your program logic, your software will be much easier to extend, refactor, debug, test, and maintain.

-	This is the reason that most front-end frameworks encourage users to manage state and component rendering in separate, loosely coupled modules.

## References
https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0

https://eloquentjavascript.net/02_program_structure.html
