# 06. String

## Syntax
    '' single quotes
    "" double quotes
    `` backticks

## Escape notation
|                   |                                   |
|--------------|------------------------|
|\XXX	    |       an octal Latin-1 character.     |
|\'            |       	single  quote       |
|\"	           |       double  quote       |
|\\\\	          |       backslash       |
|\n 	    |       new line        |
|\r 	    |       carriage return     |
|\v 	    |       vertical tab        |
|\t 	    |       tab     |
|\b 	    |       backspace       |
|\f 	    |       form feed       |
|\uXXXX 	    |       unicode codepoint       |
|\u {X} ... \u{XXXXXX}	    |       unicode codepoint       |
|\xXX   |       the Latin-1 character       |

## Long literal strings
```js
let longString = "This is a very long string which needs " +
"to wrap across multiple lines because " +
"otherwise my code is unreadable.";

let longString = "This is a very long string which needs \
to wrap across multiple lines because \
otherwise my code is unreadable.";

// including linefeed
let longString = `This is a very long string which needs
to wrap across multiple lines because
otherwise my code is unreadable.`;
```

## String conversion
    String(*something*)

## Distinction between string primitives and String objects
```js
var s_prim = 'foo';
var s_obj = new String(s_prim);
console.log(typeof s_prim); // Logs "string"
console.log(typeof s_obj);
    //    String {"foo"}
    //             0: "f"
    //             1: "o"
    //             2: "o"
    //             length: 3
    //             __proto__: String
    //                 [[PrimitiveValue]]: "foo"
```

- String primitives and String objects also give different results when using **eval()**
- Primitives passed to eval are treated as source code
- String objects are treated as all other objects are, by returning the object

```js
var s1 = '2 + 2';             // creates a string primitive
var s2 = new String('2 + 2'); // creates a String object
console.log(eval(s1));        // returns the number 4
console.log(eval(s2));        // returns the string "2 + 2"
console.log(eval(s2.valueOf())); // returns the number 4
```

## Character access
```js
'master'.charAt(2);    // "s"
'master'[2];   // "s"
```

For character access using **bracket notation**, attempting to delete or assign a value to these properties will **not succeed**.

The properties involved are **neither writable nor configurable**. `Object.defineProperty()`

## String object
The **String global object** is a **constructor** for strings or a sequence of characters.
```js
    String(*thing*) // thing : Anything to be converted to a string.
```

### I. Properties
**String.prototype**
- Allows the addition of properties to a String object.
```js
var str = 'hello';
delete str.__proto__.charAt;
String.prototype.charAt;    // undefined
```

### II. Methods
**String.fromCharCode(*number*)**
- Returns a string created by using the specified sequence of Unicode values.
```js
String.fromCharCode(65);    // 'A'
```

**String.fromCodePoint(*number*)**   up to 21-bit
- Return such a pair and thus adequately represent these higher valued characters.
```js
String.fromCodePoint(9063, 1258, 3245, 1246);   // "⍧ӪಭӞ"
```

**String.raw()**
- Returns a string created from a raw template string.
        
## String instances
All String instances inherit from String.prototype.

Changes to the String prototype object are propagated to all String instances.

```js
var str = 'dat';
delete str.__proto__.charAt;
String.prototype.charAt();  // Uncaught TypeError: String.prototype.charAt is not a function
```

### I. Properties

These properties are read-only.

**String.prototype.constructor**
- Specifies the function that creates an object's prototype.

**String.prototype.length**
- Reflects the length of the string.

**[*N*]**
- Used to access the character in the Nth position where N is an integer between 0 and one less than the value of length.
        
### II. Methods 
**String.prototype.charAt(*index*)**
- Returns the character (exactly one UTF-16 code unit) at the specified index.

**String.prototype.charCodeAt(*index*)**
- Returns a number that is the UTF-16 code unit value at the given index.
- Note that charCodeAt() will always return a value that is less than 65536.
```js
'quangdatpham'.charCodeAt(3);   // 65
```

**String.prototype.codePointAt(*index*)**
- Returns a nonnegative integer Number that is the code point value of the UTF-16 encoded code point starting at the specified index.
```js
"⍧ӪಭӞ".codePointAt(2);  // 3245
```

**String.prototype.concat(*...otherStrings*)**
```js
'Hello'.concat(' ', 'World!');  // 'Hello World!'
```

**String.prototype.includes(*string*, *from*)
- *from* at which to begin searching for *string***
```js
'quangdatpham'.includes('atpha');   // true
```

**String.prototype.endsWith(*string*)**
```js
'quangdatpham'.endsWith('ham'); // true
```

**String.prototype.indexOf(*string*, *from*)
- return -1 if not found**
```js
'quangdatpham'.indexOf('at', 4)  // 6
```

**String.prototype.lastIndexOf(*string*, *from*)
- return -1 if not found**
```js
'quangdatpham'.lastIndexOf('a', 8); // 6
```

**String.prototype.localeCompare()**
- Returns a number indicating whether a reference string comes before or after or is the same as the given string in sort order.

**String.prototype.match(*RegExp*)**
- Used to match a regular expression against a string.
```js
'QuangDatPham'.match(/[A-Z]/g);  // Array ['Q', 'D', 'P']
```

**String.prototype.normalize()**
- Returns the Unicode Normalization Form of the calling string value.

**String.prototype.padEnd(*length*, *string*)**
```js
'quangdatpham'.padEnd(15, '=');  // 'quangdatpham==='
```

**String.prototype.padStart(*string*)**
```js
'quangdatpham'.padStart(15, '=');    // '===quangdatpham'
```

**String.prototype.repeat(*times*)
- RangeError: repeat count must be non-negative, less than infinity and not overflow maximum string size.**
```js
'dat'.repeat(3);    // 'datdatdat'
```

**String.prototype.replace(*RegExp*|*string*, *newString*|*function*)**
```js
'quangdatpham'.replace(/dat/gi, 'master');  // 'quangmasterpham'

'dat123!@#'.replace(/([^\d]*)([\d]*)([^\d]*)/, function(match, m1, m2, m3, offset, str) {
    return [m1, m2, m3].join('_');
});     // 'dat_123_!@#'
```

**String.prototype.search(*RegExp*|*string*, *newString*|*function*) 
- return -1 if not found**
```js
'quangdat#pham$'.search(/[\W]/);    // 8
```

**String.prototype.slice(*beginIndex*, *endIndex*)**
- Extracts a section of a string and returns a new string.
```js
var str = 'quangdatpham';
str.slice(2); // 'angdatpham'
str.slice(2, 5);    // 'ang'
str.slice(4, -3);   // 'gdatp'
str.slice(-3);  // 'ham'
str.slice(-5, -2);  // 'tph'
```

**String.prototype.split(*RegExp*|*string*, *limit*)**
- Splits a String object into an array of strings by separating the string into substrings.
```js
var str = 'quang 1 dat 2 pham';
str.split(/\d/);    // ["quang ", " dat ", " pham"]
str.split('a', 2);    // ["qu", "ng 1 d"]
```

**String.prototype.startsWith(*string*, *position*)**
```js
var str = 'quangdatpham';
str.startWith('qua'); // true
str.startWith('dat', 6);    // true
```

**String.prototype.substr(*start*, *length*)**
```js
var str = 'quangdatpham';
str.substring(2, 5);    // 'angda'
str.substring(5, 2);    // 'da'
```

**String.prototype.substring(*start*, *end*)**
```js
var str = 'quangdatpham';
str.substring(2, 5);    // 'ang'
str.substring(5, 2);    // 'ang'
```

**String.prototype.toLocaleLowerCase()**
- The characters within a string are converted to lower case while respecting the current locale. For most languages, this will return the same as toLowerCase().

**String.prototype.toLocaleUpperCase()**
- The characters within a string are converted to upper case while respecting the current locale. For most languages, this will return the same as toUpperCase().

**String.prototype.toLowerCase()**
```js
'QUANGDATPHAM'.toLowerCase()    // 'quangdatpham'
```

**String.prototype.toString()**
- Returns a string representing the specified object. Overrides the Object.prototype.toString() method.
```js
var str = new String('dat');
console.log(str);   // expected output: String { "dat" }
console.log(str.toString());    // expected output: "dat"
```

**String.prototype.toUpperCase()**
```js
'quangdatpham'.toUpperCase();   // 'QUANGDATPHAM'
```

**String.prototype.trim()**
- Trims whitespace from the beginning and end of the string. Part of the ECMAScript 5 standard.
```js
'   quangdatpham   '.trim();    // 'quangdatpham'
```

**String.prototype.trimStart()**

**String.prototype.trimLeft()**
 
**String.prototype.trimEnd()**

**String.prototype.trimRight()**

**String.prototype.valueOf()**
- Returns the primitive value of the specified object. Overrides the Object.prototype.valueOf() method.
```js
var stringObj = new String("foo");
console.log(stringObj); // expected output: String { "foo" }
console.log(stringObj.valueOf());   // expected output: "foo"
```
**String.prototype\[@@iterator\]\(\)**
- Returns a new Iterator object that iterates over the code points of a String value, returning each code point as a String value.
```js
const str = 'The quick red fox jumped over the lazy dog\'s back.';

let iterator = str[Symbol.iterator]();
let theChar = iterator.next();

while(!theChar.done && theChar.value !== ' ') {
    console.log(theChar.value);
    theChar = iterator.next();
    // expected output: "T"
    //                  "h"
    //                  "e"
}  
```
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String

*/