# 12. The `this` keyword

        The simple but extremely important keyword in your JavaScript code.

  - [`this` in Global evironment](#this-in-global-evironment)
  - [`this` inside functions](#this-inside-functions)
  - [`this` inside constructors](#this-inside-constructors)
  - [`this` in Methods](#this-in-methods)
  - [call() and apply()](#call-and-apply)
  - [bind()](#bind)
  - [`this` inside arrow function](#this-inside-arrow-function)
  - [`this` in classes](#this-in-classes)
  - [References](#references)

## `this` in Global evironment
Open a command terminal and run the node command.

        > this === global
        true
    
Inside a JavaScript file

        console.log(this === global);
        // run this file using the node command:
        $ node index.js
        false

The reason for this is that inside a JavaScript file, this equates to module.exports and not global.

## `this` inside functions
```js
function rajat() {
    console.log(this === global);
}
rajat();    // true
```
- If we add **use strict** at the top of the file and run it again, we will get a false output because now the value of **this is undefined**.
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}

const person = Person('Master', 18);   // without the "new" keyword
console.log(person);    // undefined
```

- The reason behind this is that since the function is **not written in strict mode**, **this** refers to the **global** object.
- If we run this code in **strict mode**, we will get an error because JavaScript does not allow us to assign properties "name" and "age" to undefined. 
- To get the expected output we need to call that function as a constructor by using the "new" operator.

## `this` inside constructors
- We can convert a function call into a constructor call using **new** operator
- When a constructor call is made, a new object is created and set as the function’s this argument.
- The object is then implicitly returned from the function, unless we have another object that is being returned explicitly.

- Adding the "return" statement inside the function will overwrite the constructor call
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    
    return {name, age};     // return a literal object
}

const person = new Person('Master', 18);
console.log(person);    // {name: 'Master', age: 18}
console.log(person instanceof Hero);    // false
// Do not overwrite if return something that's not an object.
```

## `this` in Methods
```js
// The dialogue‘s "this" value then refers to hero itself.
const hero = {
    heroName: "Batman",
    dialogue() {
        console.log(`I am ${this.heroName}!`);
    }
};

hero.dialogue(); // I am Batman

const saying = hero.dialogue;
saying();   // I am undefined!
```

- When storing the reference to "dialogue" inside a variable and calling the variable as a function. The method has lost track of the "hero", "this" now refers to "global" instead of "hero".

- The loss of receiver usually happens when we are passing a method as a callback to another. We can either solve this by adding a wrapper function or by using **bind** method to tie our `this` to a specific object.

## call() and apply()
```js
function dialogue () {
    console.log (`I am ${this.heroName}`);
}

const hero = {heroName: 'Batman'};

dialogue.call(hero)
dialogue.apply(hero)
```

- But if you are using call or apply outside of strict mode, then passing null or undefined using call or apply will be ignored by the JavaScript engine.

## bind()
- When we pass a method as a callback to another function, there is always a risk of losing the intended receiver of the method, making the this argument set to the "global" object instead.
- The **bind()** method allows us to permanently tie a this argument to a value. 
```js
const hero = {
    heroName: "Batman",
    dialogue() {
        console.log(`I am ${this.heroName}`);
    }
};
setTimeOut(hero.dialogue.bind(hero), 1000);
```
- By doing so, our "this" cannot be changed by even "call" or "apply" methods.

## `this` inside arrow function 
- An arrow function uses the this value from its enclosing execution context, since it does have one of its own.
- Preventing apply, call and bind from changing it later on.
- An arrow function can also not be used as a constructor.
```js
const batman = this;
const bruce = () => {
    console.log(this === batman);
};
bruce();    // true

const counter = {
    count: 0,
    increase() {
        setInterval (() => {
            console.log (++this.count);
        }, 1000);
    },
};
counter.increase();    // it's working
```

## `this` in classes
- A class generally contains a constructor, where this would refer to any newly created object.
- And just like a method, classes can also lose track of the receiver.
```js
class Hero {
    constructor(heroName) {
        this.heroName = heroName;
    }
    dialogue() {
        console.log(`I am ${this.heroName}`)
    }
}
const batman = new Hero("Batman");

const say = batman.dialogue;
say();  // Uncaught TypeError: Cannot read property 'heroName' of undefined
```
- The reason for error is that JavaScript classes are implicitly in strict mode.
- We will need to manually bind() to tie this dialogue() function to "batman".

## References
https://blog.bitsrc.io/what-is-this-in-javascript-3b03480514a7