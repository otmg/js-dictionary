# 08. Object

## Object constructor
- The Object constructor creates an object wrapper for the given value.
- If the value is null or undefined, it will create and return an empty object, otherwise, it will return an object of a Type that corresponds to the given value.
- If the value is an object already, it will return the value.
- To indicate a property a private put an underscore (_) character at the start of the property name.

- When called in a non-constructor context, Object behaves identically to `new Object()`.
```js
new Object();   // {}
new Object(undefined);  // {}
new Object(null);   // {}
new Object(true);   // Boolean {true}
new Object(1)   // Number  {1}
```

## Deleting a property
- The delete operator removes a given property from an object.
- On successful deletion, it will return true, else false will be returned.
```js
delete obj.propertyName
delete obj['propertyName']
```

## Property attributes
- At first glance, it might look like there is a one-to-one mapping between keys and values of an object. However, that’s not entirely true.
- Property Attributes :
    - Every key of an object contains a set of property attributes that define the characteristics of the value associated with the key.
    - There are six property attributes:
            [[Value]]
            [[Get]]
            [[Set]]
            [[Writable]]
            [[Enumerable]]
            [[Configurable]]

- Why have we wrapped the attribute names in [[]]? Double square brackets mark internal properties used by the ECMA specifications.
- These are properties that JS programmer cannot touch directly from within the code. 

## Flags
Data properties have three special attributes (so-called `flags`):
    - `writable` – if true, can be changed, otherwise it’s **read-only**.
    - `enumerable` – if true, then listed in loops, otherwise **not listed**.
    - `configurable` – if true, the property **can be deleted** and these attributes **can be modified**, **otherwise not**.

```js
const dog = {
    color: 'black'
};
console.log(Object.getOwnPropertyDescriptor(dog, 'color'));
    // expected output:
    // {
    //     value: "black",
    //     writable: true,
    //     enumerable: true,
    //     configurable: true
    // }
```

## Accessor Properties (getter & setter)
- Reading accessor properties do not need to use parentheses to invoke the function.
```js
var obj = {
    _name: 'Quang Dat', // private property
    get name() {
        return this._name;
    },
    set name(val) {
        this._name = val;
    }
};
```

## Iteration (Enumeration)
- for-in loop will return all string properties which is enumerable.
- all enumerable properties of prototype are also returned.
```js
for (let property in obj) {
    // Read the value 
    let value = obj[property];
}
```

- **Object.keys()** is similar to for-in loop except that it only returns the object’s own keys in an array.
- **Object.values()** returns an array of values.
- **Object.entries()** returns an array of [key, value] pairs.

## Define a property and Change its flags
- If the property exists, defineProperty updates its flags.
- Otherwise, it creates the property with the given value and flags;
- if a flag is not supplied, it is assumed false.
```js
var person = {
    age: 18
};

Object.defineProperty(person, 'name', {
    value: 'Dat',
    "writable": false,  // read-only
    "enumerable": false,    // non enumerable
    "configurable": false   // non configurable
});

// read-only  //
person.name = 'abc';    // Error: Cannot assign to read only property 'name'...

// non enumerable   //
for (let property in person) {
    console.log(property);  // age
}
console.log(Object.keys(person));   // age

// non configurable //
Object.defineProperty(person, 'name', {
    "writable": true,
}); // won't able to change
delete person.name; // won't able to delete

// Define many properties at once.
Object.defineProperties(user, {
    name: { value: "Dat", writable: false },
    surname: { value: "Pham", writable: false },
    // ...
});

// Together with Object.defineProperties it can be used as a “flags-aware” way of cloning an object:
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));

// Normally when we use an assignment to copy properties:
for (let key in user) {
    clone[key] = user[key]  // that does not copy flags
}
```

- Another difference is that **for..in ignores symbolic properties**,
- but **Object.getOwnPropertyDescriptors** returns all property descriptors **including symbolic ones**.

## Protecting objects
- `Preventing extension (non-extensible)` : prevents new properties from ever being added to an object.
- `Sealing (non-extensible, non-configurable)` : prevents extensions and makes all properties **“non-configurable”**
- `Freezing (non-writable, sealing)` : seals the object, prevents modifying any existing properties at all, prevents the descriptors from being changed

## Mutability
- When we have two numbers, 120 and 120, we can consider them precisely the same number, whether or not they refer to the same physical bits.
- With objects, there is a difference between having two references to the same object and having two different objects that contain the same properties.
```js
let object1 = {value: 10};
let object2 = object1;
let object3 = {value: 10};

console.log(object1 == object2);    // true
console.log(object1 == object3);    //  false

object1.value = 15;
console.log(object2.value); // 15
console.log(object3.value); // 10
```

- Bindings can also be changeable or constant
```js
const score = {visitors: 0, home: 0};
// This is okay
score.visitors = 1;
// This isn't allowed
score = {visitors: 1, home: 1};
```

## Clone a object
```js
var person = {
    name: 'Dat',
    gender: true,
    school: {
        tqk: 'Done',
        vtt: 'Done',
        fpt: 'Not yet'
    }
};
console.log(person);
    //    {name: "Dat", gender: true, school: {…}}
    //             gender: true
    //             name: "Dat"
    //             school:
    //                 fpt: "Not yet"
    //                 tqk: "Done"
    //                 vtt: "Done"
    //                 __proto__: Object   // "__proto__" of the "school" reference
    //             __proto__: Object       // "__proto__" of the "person" reference
```

- Spread operator (Shallow-cloning)
```js
var newObj = {...person};
person.name = '';
console.log(newObj.name);   // Dat
person.school.tqk = '';
console.log(newObj.school.tqk); //  ''
```

- Object.assign (Shallow-cloning)
```js
Object.assign({}, person);
```

- JSON (deep clone)
- JSON can not clone object method (function)
```js
var clone = JSON.stringify(person);
var newPerson = JSON.parse(clone);
```

- Deep clone
```js
function cloneDeep(obj) {
    var newObj = {};
    for (pro in obj) {
        newObj[pro] = 
            (typeof obj[pro]) === 'object' ?
                cloneDeep(obj[pro]) :
                obj[pro];
    }
    return newObj;
}
```

## The `Object` object
### I. Properties
**Object.length**
- Has a value of 1.

**Object.prototype**
- Allows the addition of properties to all objects of type Object.

### II. Methods
**Object.assign(*target*, *...objects*)**
- Copies the values of all enumerable own properties from one or more source objects to a target object.
- Both String and Symbol properties are copied.
- Getters are copied and turned into properties
```js
var o1 = {a: 1} // target
var o2 = {
    a: 12,
    b: {c: 5},  // referrence
    [Symbol('a')]: 10,  // property named symbol
    get d() { retrun 6; }   // getter
};    // source

var o3 = Object.assign(o1, o2);
console.log(o3);  // expected output: {a: 12, b: {c: 5}, d: 6, [Symbol('a')]: 10}
console.log(o1);  // expected output: {a: 12, b: {c: 5}, d: 6, [Symbol('a')]: 10}

o3.a = 0;
console.log(o2.a);    // expected output: 12
console.log(o3.a);    // expected output: 0

o2.b.c = 8;
console.log(o2.b.c);    // expected output: 8
console.log(o3.b.c);    // expected output: 8

// primitives wrapped
var v1 = 'abc';
var v2 = true;
var v3 = 10;
var v4 = Symbol('foo');
var obj = Object.assign({}, v1, null, v2, undefined, v3, v4); 
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```

**Object.create(*prototypedObject*, *...propertiesObject*)**
- Creates a new object with the specified prototype object and properties.
```js
Object.create(null);    // {}   // No properties // nothing inside
Object.create(Object.prototype);    // {}   // normal
Object.create({});  // {}   // proto: { proto: Object}   // look at your console

Object.create({}, {
    name: {
        value: 'Dat',
        writable: true,
        configurable: true
    },
    isBoy: {
        get() {
            return true;
        }
        set(value) {
            console.log('This property is non-configurable.');
        }
    }
});
```

**Object.defineProperty(*obj*, *propertyName*, *descriptor*) // see Define a property and Change its flags**
- Adds the named property described by a given descriptor to an object.
```js
// define Getter and Setter
const Person = (function() {
function Person() {
    this._name = '';
}
Object.defineProperty(Person.prototype, 'name', {
    get () {
    return this._name;
    },
    set (val) {
    this._name = val;
    },
    enumerable: false,
    configurable: false,
});
return Person;
})();
```

**Object.defineProperties(*obj*, *properties*)   // see Define a property and Change its flags**
- Adds the named properties described by the given descriptors to an object.

**Object.entries(*obj*)**
- Returns an array containing all of the [key, value] pairs of a given object's own enumerable string properties.
```js
// array like object with random key ordering
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]

// getFoo is property which isn't enumerable
const myObj = Object.create({}, { getFoo: { value() { return this.foo; } } });
myObj.foo = 'bar';
console.log(Object.entries(myObj)); // [ ['foo', 'bar'] ]

// non-object argument will be coerced to an object
console.log(Object.entries('foo')); // [ ['0', 'f'], ['1', 'o'], ['2', 'o'] ]
```

**Object.freeze(*obj*)**
- Freezes an object(array is also an object): other code can't delete or change any properties.
- Non-writable, non-configurable, non-extensible.
- Freeze is shallow (non-deep effect).
```js
var obj = {
    foo: 'foo',
    deep: {
        a: 5
    }
};
obj.bar = 'bar';
delete obj.foo; // true
var freeze = Object.freeze(obj);
freeze === obj; // true
obj.bar = 'foo';    // nothing happened
delete obj.bar; // false
obj.deep.a = 1; // a now is changed to 1
Object.isFreeze(obj);   //true
Object.defineProperty(obj, 'number', { value: 17 });  // throw a TypeError
Object.setPrototypeOf(obj, { x: 20 })   // throw a TypeError
```

**Object.fromEntries(*iterable*)**
- Returns a new object from an iterable of key-value pairs (reverses Object.entries).
```js
const map = new Map([ ['foo', 'bar'], ['baz', 42] ]);
Object.fromEntries(map);    // { foo: "bar", baz: 42 }
const arr = [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ];
Object.fromEntries(arr);    // { 0: "a", 1: "b", 2: "c" }
```

**Object.getOwnPropertyDescriptor(*obj*, *propertyName*)   // see Property flags**
- Returns a property descriptor for a named property on an object.
```js
// look up getter setter
var obj = {
    name: 'dat',
    get getName() {
        return this.name;
    }
};
Object.getOwnPropertyDescriptor(obj, "getName").get;
//  ƒ getName() {   return this.name;   }
```

**Object.getOwnPropertyDescriptors(*obj*)**
- Returns an object containing all own property descriptors for an object.

**Object.getOwnPropertyNames(*obj*)**
- Returns an array containing the names of all of the given object's own enumerable and non-enumerable properties.
```js
var obj = Object.create({}, {
    getName: {
        value() { return 'Dat';},
        enumerable: false
    }
});
obj.name = 1;
Object.getOwnPropertyNames(my_obj); // ["getName", "name"]

Object.getOwnPropertyNames('foo');  // ["0", "1", "2", "length"]  (ES2015 code)
```

**Object.getOwnPropertySymbols(*obj*)**
- Returns an array of all symbol properties found directly upon a given object.
```js
var obj = {
    [Symbol('a')]: 'a',
    [Symbol('b')]: 'b'
};
Object.getOwnPropertySymbols(obj);  // [Symbol(a), Symbol(b)]
```

**Object.getPrototypeOf(*obj*)**
- Returns the prototype of the specified object.
```js
var obj1 = {};
var obj2 = Object.create(obj1); // {} // proto: { proto: Object}
Object.getPrototypeOf(obj2) === obj1; // true
```

**Object.is(*value1*, *value2*)**
- Compares if two values are the same value. Equates all NaN values
- (which differs from both Abstract Equality Comparison and Strict Equality Comparison).
```js
Object.is(0, -0);   // false
Object.is(NaN, 0/0);    // true // 0/0 --> NaN
0/0 === NaN // false
```

**Object.isExtensible(*obj*)**
- Determines if extending of an object is allowed.
-  New objects are extensible.
```js
var obj = {};
var sealedObj = Object.seal({});
Object.isExtensible(obj);   // true
Object.isExtensible(sealedObj);    // false
Object.preventExtensions(obj);
Object.isExtensible(obj); // false
Object.isExtensible(10);    // false
```

**Object.isFrozen(*obj*)**
- Determines if an object was frozen.
```js
var obj = Object.preventExtensible({}); // empty obj
Object.isFrozen(obj);   // true

var oneProp = { p: 42 };    // obj has one property
Object.preventExtensions(oneProp);
Object.isFrozen(oneProp); // === false
delete oneProp.p;   // delete property
Object.isFrozen(oneProp); // === true

Object.isFrozen(2); // true
```

**Object.isSealed()**
- Determines if an object is sealed.
```js
var emptyObj = {};
Object.preventExtensions(emptyObj);
Object.isSealed(emptyObj);  // 

var obj = {name: 'cat'};
Object.seal(obj);
Object.isSealed(obj);   // true
Object.isExtensible(obj);   // false
```

**Object.keys(*obj*)**
- Returns an array containing the names of all of the given object's own enumerable string properties.
```js
var obj = {12: 'a', 8: 'b', 30: 'c'};
Object.keys(obj);   // ['8', '12', '30']
```

**Object.preventExtensions(*obj*)**
- Prevents any extensions of an object.
- Properties can be changed or deleted, but you can not defined new properties.
```js
var obj = {type: 'pet'};
var preExt = Object.preventExtensions(obj);
obj === preExt; // true
Object.definedProperty(obj, 'weight', {
    value: 24
}); // throw a TypeError
Object.preventExtensions(1);    // 1
```

**Object.seal(*obj*)**
- Prevents other code from deleting properties of an object.
- Non-extensible, non-configurable.
```js
var obj = {name: 'cat'};
Object.seal(obj);
delete obj.name;    // do nothing
obj.weight = 12;    // do nothing
obj.name = 'dog';   // name's value has changed to 'dog'
Object.isSealed(obj);   // true
Object.isExtensible(obj);   // false
Object.seal(1); // 1
```

**Object.setPrototypeOf(*obj*, *prototype*)**
- Sets the prototype (i.e., the internal [[Prototype]] property).
- Not recommended, use Object.create() instead.

**Object.values(*obj*)**
- Returns an array containing the values that correspond to all of a given object's own enumerable string properties.
```js
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']
Object.value('dat');    // ['d', 'a', 't']
```

## Object instances and Object prototype object
### I. Properties 
**Object.prototype.constructor**
- Specifies the function that creates an object's prototype.

**Object.prototype.\_\_proto\_\_**
- Points to the object which was used as prototype when the object was instantiated.
        https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto
    
### II. Methods
**Object.prototype.hasOwnProperty(*property*)**
- Returns a boolean indicating whether an object contains the specified property as a direct property of that object and not inherited through the prototype chain.
```js
var obj = {};
obj.empty = undefined;
obj.hasOwnProperty('empty');    // true
obj.hasOwnProperty('toString'); // false
```

**Object.prototype.isPrototypeOf(*obj*)**
- Returns a boolean indicating whether the object this method is called upon is in the prototype chain of the specified object.
```js
function Foo() {}
function Bar() {}
function Baz() {}

Bar.prototype = Object.create(Foo.prototype);
Baz.prototype = Object.create(Bar.prototype);

var baz = new Baz();

console.log(Baz.prototype.isPrototypeOf(baz)); // true
console.log(Bar.prototype.isPrototypeOf(baz)); // true
console.log(Foo.prototype.isPrototypeOf(baz)); // true
console.log(Object.prototype.isPrototypeOf(baz)); // true
```

**Object.prototype.propertyIsEnumerable(*property*)**
- Returns a boolean indicating if the internal ECMAScript [[Enumerable]] attribute is set.
```js
var obj = {};
obj.name = 'dat';
obj.propertyIsEnumerable('name');   // true
obj.propertyIsEnumerable('weight');   // false
obj.propertyIsEnumerable('constructor');   // false
```

**Object.prototype.toLocaleString()**
- Calls toString().

**Object.prototype.toString()**
- Returns a string representation of the object
```js
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
}

Person.prototype.toString = function () {
    return this.firstName + ' ' + this.lastName;
}

var p = new Person('Dat', 'Pham');
p.toString();   // 'Dat Pham'
```
**Object.prototype.valueOf()**
- Returns the primitive value of the specified object.
        
## References

https://javascript.info/property-descriptors#sealing-an-object-globally

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object

https://toddmotto.com/typescript-setters-getter
*/