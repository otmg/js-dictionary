# 01. Value - Type
`EASY TO LEARN`

  - [Data types](#data-types)
  - [typeof operator](#typeof-operator)
  - [Primitive and Reference type](#primitive-and-reference-type)
  - [Primitive wrapper objects in JavaScript](#primitive-wrapper-objects-in-javascript)
  - [References](#references)

## Data types
-	Number
```js
    123456
    2.45    // fractional
    2.993e6 // --> 2.993 x 10^6   //exponent
```
-	String
```js
    '' // single quotes
    "" // double quotes
    `` // backticks

    /* Concatenate */
   'hello' + 'world' // --> 'helloworld'
```
-	Boolean
```js
    true
    false
```
-	Object `{}`
-	Array `[]`
-	Function
```js
    function fnName() {
    	// do something
    }
```
-	Special values
```js
    Infinity   -Infinity    NaN

    typeof NaN;  // "number"
    typeof Infinity;  // "number"
```

-	Empty values
```js
    null    undefined

    typeof null // "object"
    typeof undefined    // "undefined"
```

## typeof operator
```js
    typeof "Hello";    // --> "string"
```

## Primitive and Reference type
The differences between primitive types and reference types is how they are stored in memory.
-	Primitive
    -	A primitive (primitive value, primitive data type) is data that is **not an object**.
    -	Primitive values have **no properties**.
    -	In JavaScript, there are 6 primitive data types:
        		string, number, boolean, null, undefined, symbol

    -	Primitive values are **immutable**, but it **can be replaced**.
    ```js
        var str = 'dat';
        str.toUpperCase();
        console.log(str);       // dat
        str = 'Hello';
        console.log(str);       // Hello

        var num = 12;
        function minusOne(val) {
            val -= 1;
        }
        minusOne(num);
        console.log(num);       // 12
    ```

-	Reference
    -	Reference values are references to objects (addresses where they will be found).
    -	A property can be a reference (object) or a primitive.
    ```js
        var arr = [2, 4, 6];
        var refArr = arr;
        console.log(arr === refArr);    // true

        var arr = [2, 4, 6];
        var anotherArr = [2, 4, 6];
        console.log(arr === anotherArr);    // false

        var obj = {
            a: 2,
            c: {
                d:12
            }
        };

        var refObj = obj.c;
        refObj.d = 1;
        console.log(obj.c.d);   // 1
    ```

## Primitive wrapper objects in JavaScript
    String  Number  Boolean Symbol


## References
https://stackoverflow.com/questions/13266616/primitive-value-vs-reference-value

https://developer.mozilla.org/en-US/docs/Glossary/Primitive

https://medium.com/@ctrlalt_diljeet/primitive-types-vs-reference-types-in-javascript-8503259c30ca
