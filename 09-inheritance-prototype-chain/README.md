# 09. Inheritance - Prototype chain

## Object, Array, and Function
- JavaScript gives us access to three global functions: Object, Array, and Function. Yes, these are all functions.
- First-class objects in JavaScript.
```js
console.log(Object); // -> ƒ Object() { [native code] }
console.log(Array); // -> ƒ Array() { [native code] }
console.log(Function); // -> ƒ Function() { [native code] }
```

- You don’t know it, but every time you create an object literal, the JavaScript engine is effectively calling `new Object()`.
- An object literal is an object created by writing **{}**, as in `var obj = {};`.
- So an object literal is an **implicit** call to Object. Same goes for arrays and functions. We can think of an array as coming from the Array constructor and a function as coming from the Function constructor.

## Class notation
```js
class Square {
    constructor(a) {
        this.a = a;
    }

    // method
    calculateArea() {
        return this.a ** 2;
    }

    // getter
    get hypotenuse() {
        return this.a * Math.sqrt(2);
    }

    // setter
    set hypotenuse(val) {
        this.a = val / Math.sqrt(2);
    }

    // static
    static fromArea(val) {
        return new Square(Math.sqrt(val));
    }
}

var square = new Square(4);
console.log(square instanceof Square);  // true
console.log(square.calculateArea());    // 16

var square2 = Square.fromArea(36);
square2.hypotenuse = 12;
console.log(square2.a); // 8.48528137423857

console.log(square.__proto__ === Square.prototype);    // true
console.log(square.__proto__.constructor === Square);  // true
    // Square {a: 4}
    //             a: 4
    //             hypotenuse: (...)
    //             __proto__:
    //                 calculateArea: ƒ calculateArea()
    //                 constructor: class Square
    //                 hypotenuse: (...)
    //                 get hypotenuse: ƒ hypotenuse()
    //                 set hypotenuse: ƒ hypotenuse(val)
    //                 __proto__: Object

Object.getOwnPropertyNames(Square);
// ["length", "prototype", "fromArea", "name"]
```

## Function constructor
- When we invoke a function using the **new** keyword, the object bound to **this** in the constructor function is special.
- It sets the returned object’s **\_\_proto\_\_** property equal to the function’s **prototype** property. This is the **key** to **inheritance**.
```js
function Length(metre) {
    this.metre = metre;
}

Object.defineProperty(Length.prototype, 'kilometre', {
    get () {
        return this.metre / 1000;
    },
    set (val) {
        this.metre = val * 1000;
    }
});

// the same to static
Length.fromKilometre = function (val) {
    return new Length(val / 1000);
}

// Using prototypal inheritance // Recommended
Length.prototype.printLength = function () {
    console.log('Metre: ' + this.metre);
    console.log('Kilometre: ' + this.kilometre);
}

var length = new Length(1);
console.log(length.__proto__ === Length.prototype);  // true
console.log(length.__proto__ === Length);   // true
    // Length {metre: 1}
    //             metre: 1
    //             kilometre: (...)
    //             __proto__:
    //                 printLength: ƒ ()
    //                 constructor: ƒ Length(metre)
    //                 kilometre: (...)
    //                 get kilometre: ƒ ()
    //                 set kilometre: ƒ (val)
    //                 __proto__: Object
var obj = {};   // common object
obj.__proto__ = Length.prototype;   // assignment
obj instanceof Length;  // true

Object.getOwnPropertyNames(Length);
// ["length", "name", "arguments", "caller", "prototype", "fromKilometre"]
```

## Object literals (Manually)
```js
const Person = {  // not being a contructor
    run: function () {
        console.log('I\'m running.');
    },
    get sayHello() {
        console.log('Hello!');
    }
};

var person = Object.create(Person);
person.name = 'Dat';
person instanceof Person;   // Uncaught TypeError: Right-hand side of 'instanceof' is not callable
console.log(person);
    //  {name: "Dat"}
    //         name: "Dat"
    //         sayHello: (...)
    //         __proto__:
    //             run: ƒ ()
    //             sayHello: (...)
    //             get sayHello: ƒ sayHello()
    //             __proto__: Object
```

## Inheritance && prototype chain
- When trying to access a property of an object, the property will not only be sought on the object but on the prototype of the object, the prototype of the prototype, and so on until either a property with a matching name is found or the end of the prototype chain is reached.
- The [[Prototype]] is accessed using the accessors **Object.getPrototypeOf()** and **Object.setPrototypeOf()** or  **\_\_proto\_\_** (`non-standard`).
- It should not be confused with the "func.prototype" property of functions, which instead specifies the [[Prototype]] to be assigned to all instances of objects created by the given function when used as a constructor. The **Object.prototype** property represents the "Object" prototype object.

```js
// Consider this code
({}).__proto__ === Object.prototype;    // true
Object.prototype.__proto__; // null
[] instanceof Object;   // true
[].__proto__.__proto__ === Object.prototype;    // true
Array.prototype.__proto__ === Object.prototype; // true

class Mammal {
    constructor(weight) {
        this.weight = weight || 0;
    }

    run() {
        console.log('Running...');
    }
}

class Rabbit extends Mammal {
    constructor(color, weight) {
        super(weight);
        this.color = color || 'none';
    }

    jump() {
        console.log('Jumpping...');
    }
}

var redRabbit = new Rabbit('red', 12);
redRabbit.hasOwnProperty('jump');   // false
redRabbit.hasOwnProperty('weight');   // true
        // Rabbit {weight: 12, color: "red"}
        //         color: "red"
        //         weight: 12
        //         __proto__: Mammal
        //             constructor: class Rabbit
        //             jump: ƒ jump()
        //             __proto__:
        //                 constructor: class Mammal
        //                 run: ƒ run()
        //                 __proto__: Object

// the same with function constructor
function Mammal(weight) {
    this.weight = weight || 0;
}

Mammal.prototype.run = function () {
    console.log('Running...');
};

function Rabbit(color, weight) {
    this.color = color || 'none';
    Mammal.call(this, weight);  // calling Mammal constructor and specifing the "weight" property
}

Rabbit.prototype = Object.create(Mammal.prototype); // Rabbit.prototype = new Mammal();
Rabbit.prototype.constructor = Rabbit;
Rabbit.prototype.jump = function () {
    console.log('Jumpping...');
};
```

## Method overriding
```js
class Animal {
    constructor(color, weight) {
        this.color = color;
        this.weight = weight;
    }

    gainWeight(val) {
        this.weight += val;
    }
}

class Dog extends Animal {
    constructor(name, color, weight) {
        super(color, weight);
        this.name = name;
    }

    gainWeight(val, callback) {
        super.gainWeight(val);  // call the "gainWeight" method from the superclass
        callback();
    }
}

var dog = new Dog('abc', 'blue', 100);
dog.gainWeight(12, function () {
    console.log('Gain Weight.');
}); // Gain Weight
```

## References
https://community.risingstack.com/javascript-prototype-chain-inheritance/

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain#Inheritance_with_the_prototype_chain

https://codeburst.io/master-javascript-prototypes-inheritance-d0a9a5a75c4e

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model#Class-based_vs._prototype-based_languages