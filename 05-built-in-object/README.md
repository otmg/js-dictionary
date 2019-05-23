# 05. Built-in Object

  - [Value properties](#value-properties)
  - [Function properties](#function-properties)
  - [Fundamental objects](#fundamental-objects)
  - [Numbers and dates](#numbers-and-dates)
  - [Text processing](#text-processing)
  - [Indexed collections](#indexed-collections)
  - [Keyed collections](#keyed-collections)
  - [Structured data](#structured-data)
  - [Control abstraction objects](#control-abstraction-objects)
  - [Reflection](#reflection)
  - [Other](#other)
  - [Do this in your browser console](#do-this-in-your-browser-console)
  - [References](#references)

![Built-in Object][built-in-object]

## Value properties
These global properties return a simple value; they have no properties or methods.
```js
Infinity
NaN
undefined
null
globalThis, self, window, frames // browser
global // nodejs
```
## Function properties
These global functions—functions which are called globally rather than on an object—directly return their results to the caller.
```js
eval()
isFinite()
isNaN()
parseFloat()
parseInt()
decodeURI()
decodeURIComponent()
encodeURI()
encodeURIComponent()
```

## Fundamental objects
These are the fundamental, basic objects upon which all other objects are based. This includes objects that represent general objects, functions, and errors.
```js
Object
Function
Boolean
Symbol
Error
EvalError
InternalError
RangeError
ReferenceError
SyntaxError
TypeError
URIError
```

## Numbers and dates
These are the base objects representing numbers, dates, and mathematical calculations.
```js
Number
BigInt
Math
Date
```

## Text processing
These objects represent strings and support manipulating them.
```js
String
RegExp
```
## Indexed collections
These objects represent collections of data which are ordered by an index value. This includes (typed) arrays and array-like constructs.
```js
Array
Int8Array
Uint8Array
Uint8ClampedArray
Int16Array
Uint16Array
Int32Array
Uint32Array
Float32Array
Float64Array
```

## Keyed collections
These objects represent collections which use keys; these contain elements which are iterable in the order of insertion.
```js
Map
Set
WeakMap
WeakSet
```

## Structured data
These objects represent and interact with structured data buffers and data coded using JavaScript Object Notation (JSON).
```js
ArrayBuffer
DataView
JSON
```

## Control abstraction objects
```js
Promise
Generator
GeneratorFunction
AsyncFunction 
```

## Reflection
```js
Reflect
Proxy
```

## Other
```js
arguments
```

## Do this in your browser console
```js
Object.getOwnPropertyNames(Object);
// (24) ["length", "name", "prototype", "assign", "getOwnPropertyDescriptor", "getOwnPropertyDescriptors", "getOwnPropertyNames", "getOwnPropertySymbols", "is", "preventExtensions", "seal", "create", "defineProperties", "defineProperty", "freeze", "getPrototypeOf", "setPrototypeOf", "isExtensible", "isFrozen", "isSealed", "keys", "entries", "values", "fromEntries"]
Object.getOwnPropertyNames(Object.prototype);
// (12) ["constructor", "__defineGetter__", "__defineSetter__", "hasOwnProperty", "__lookupGetter__", "__lookupSetter__", "isPrototypeOf", "propertyIsEnumerable", "toString", "valueOf", "__proto__", "toLocaleString"]
Object.getOwnPropertyNames(Object.__proto__);
// (9) ["length", "name", "arguments", "caller", "constructor", "apply", "bind", "call", "toString"]

Object.getOwnPropertyNames(Function);
// (3) ["length", "name", "prototype"]
Object.getOwnPropertyNames(Function.prototype);
// (9) ["length", "name", "arguments", "caller", "constructor", "apply", "bind", "call", "toString"]
Object.getOwnPropertyNames(Function.__proto__);
// (9) ["length", "name", "arguments", "caller", "constructor", "apply", "bind", "call", "toString"]
Function.__proto__ === Function.prototype
// true

Function instanceof Object
// true
Object instanceof Function
// true

function fn() {}
fn instanceof Function
// true
fn.__proto__ === Function.prototype
// true
Object.getOwnPropertyNames(fn);
// (5) ["length", "name", "arguments", "caller", "prototype"]
```

## References
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects


[built-in-object]: /assets/images/built-in-object.png