# 11. Regular Expression

        In JavaScript, regular experssion are also objects.

## Creating regular expressions
- Literal — Syntax
```js
var regexp = /a+bcd/;   // forward slash (/)
```

- calling with "RegExp" object
```js
var regexp = new RegExp('a+bcd');
```

## Character groups
- Basics

|               |                                                     |
|:-----------:|---------------------------------------|
|  .	    | Any character except newline              |
|  a	    | The character a                                 |
|  ab	    | The string ab                                    |
|  a|b	    |    a or b  Alternation                         |
|  a*	    | 0 or more a's                                    |
|  \	    | Escapes a special character               |

- Quantifiers

|                       |                                       |
|:-----------:|---------------------------------------|
|*	   | 0 or more                      |
|+	   | 1 or more                      |
|?	   | 0 or 1                     |
|{2}	|    Exactly 2                      |
|{2, 5}|	    Between 2 and 5                     |
|{2,}	|    2 or more                      |

- Groups

|                       |                                       |
|:-----------:|---------------------------------------|
|(...)	 |   Capturing group        |
|(?:...)|	    Non-capturing group ( Matches and does not remember the match)      |
|\Y	   | Match the Y'th captured group      |

- Meta-Characters

|                       |                                       |
|:-----------:|---------------------------------------|
|[ab-d]	    |       One character of: a, b, c, d        |
|[^ab-d]	    |       One character except: a, b, c, d        |
|[\b]	    |    Backspace character        |
|\d	        |   One digit       |
|\D	        |   One non-digit       |
|\s	        |   One whitespace      |
|\S	        |   One non-whitespace      |
|\w	        |   One word character  [A-Za-z0-9_]        |
|\W	        |   One non-word character      |

- Assertions

|                       |                                       |
|:-----------:|---------------------------------------|
|^	        |   Start of string     |
|$	        |   End of string       |
|\b	    |    Word boundary      |
|\B	    |    Non-word boundary      |
|x(?=y)	    |       Matches 'x' only if 'x' is followed by 'y' (Positive lookahead)     |
|x(?!y)	    |       Matches 'x' only if 'x' is not followed by 'y' (Negative lookahead)     |

- Flags

|                       |                                       |
|:-----------:|---------------------------------------|
|   g   |	    Global Match        |
|   i   |	    Ignore case     |
|   m   |	    ^ and $ match start and end of line     |
|   y   |       sticky      |
|   u   |       Unicode     |

- Special characters

|                       |                                       |
|:-----------:|---------------------------------------|
|   \n  |	    Newline (Line feed LF)      |
|   \r  |	    Carriage return (CR)        |
|   \f  |      Form feed (FF)       |
|   \t  |	    Tab     |
|   \0  |	    Null character      |
|   \YYY    |	    Octal character YYY     |
|   \xYY    |	    Hexadecimal character YY        |
|   \uYYYY  |	    Hexadecimal character YYYY      |
|   \cY |	    Control character Y     |

- Replacement

|                       |                                       |
|:-----------:|---------------------------------------|
|   $$  |	    Inserts an  "$" character       |
|   $&  |  	Insert entire match     |
|   $`  |	    Insert preceding string     |
|   $'  |	    Insert following string     |
|   $Y  |	    Insert Y'th captured group      |

## RegExp methods
- `test`   
```js
let reTest = /quang?dat{1,4}/;
console.log(reTest.test("quangdat"));   // true
console.log(reTest.test("quandattt"));  // true
```
- `exec`    
```js
var regExp = /a/;
console.log(regExp.exec('quangdat'));
//  ["a", index: 2, input: "quangdat", groups: undefined]
```
- `source`    
    - The text of the pattern.
    - Updated at the time that the regular expression is created, not executed.

1. String methods
- `match`   
```js
var regExp = /dat/;
console.log('quangdat'.match(regExp));
//  ["dat", index: 5, input: "quangdat", groups: undefined]
```

- `search`  
```js
var regExp = /\D+/;
console.log('342dat14343dddsd43'.search(regExp));   // 3
``

- `replace` 
```js
console.log('quangdat'.replace(/dat/, 'DAT'));  // quangDAT
console.log('quangdat'.replace(/dat/, function(match) {
    return match.toUpperCase();
}));    // quangDAT
```

- `split`   
```js
var str = `quangdatpham
javascript
developer`;
console.log(str.split(/\r?\n/));
//  ["quangdatpham", "javascript", "developer"]
```

## Groups
```js
let quotedText = /'([^']*)'/;
console.log(quotedText.exec("she said 'hello'"));
// ["'hello'", "hello"]

console.log(/(\d)+/.exec("123"));
// ["123", "3"]
```

## Alternation
```js
let animalCount = /\b\d+ (pig|cow|chicken)s?\b/;
console.log(animalCount.test("15 pigs"));
// true
console.log(animalCount.test("15 pigchickens"));
// false
```

## Global and sticky
- When sticky is enabled, the match will succeed only if it starts directly at lastIndex, whereas with global, it will search ahead for a position where a match can start.
```js
var regExp = /a/g;
console.log('quangdat'.match(regExp));
// ["a", "a"]

//  RegExp.prototype.exec
var regExp = /a/g;
console.log(regExp.exec('quangdat'));   // first exec
// ["a", index: 2, input: "quangdat", groups: undefined]
console.log(regExp.lastIndex);  // 3

console.log(regExp.exec('quangdat'));   // second exec
// ["a", index: 6, input: "quangdat", groups: undefined]
console.log(regExp.lastIndex);  // 7

var sticky = /dat/y;
sticky.lastIndex = 2;
console.log(sticky.test('dddat'));    // true
```

## Replacement and groups
```js
var str = 'green-2, red-74, blue-34';
console.log(
    str.replace(/(\w+)-(\d+)/g, '$2 $1')
);
// 2 green, 74 red, 34 blue

str.replace(/(\w+)-(\d+)/g, function(wholeMatch, color, number) {
    return number + ' ' + color;
});
// 2 green, 74 red, 34 blue

console.log(/(\d)+/.exec("123"));
// ["123", "3", index: 0, input: "123", groups: undefined]

let quotedText = /'([^']*)'/;
console.log(quotedText.exec("she said 'hello'"));
// ["'hello'", "hello", index: 9, input: "she said 'hello'", groups: undefined]
```

## Word boundary
- A position between `\w ([a-zA-Z0-9])` and `\W` (non-word char), Or at the beginning or end of a string
```js
console.log(/\bdat\b/.test('dat')); // true
console.log(/\bdat\b/.test('++dat++')); // true
console.log(/\bdat\b/.test('dat++')); // true
console.log(/\bdat\b/.test('dat quang pham')); // true
console.log(/\b\d+\b/.test('abcd 12 ddd')); // true
```

## Greed
- If you put a question mark after them `(+?, *?, ??, {}?)`, they become **nongreedy** and start by matching as little as possible
```js
function stripComments(code) {
    return code.replace(/\/\/.*|\/\*[^]*\*\//g, "");
}
console.log(stripComments("1 /* a */+/* b */ 1"));
// 1  1

function stripComments(code) {
    return code.replace(/\/\/.*|\/\*[^]*?\*\//g, "");
}
console.log(stripComments("1 /* a */+/* b */ 1"));
    // 1 + 1
```

## Dynamically creating RegExp objects
```js
let name = "harry";
let text = "Harry is a suspicious character.";
let regexp = new RegExp("\\b(" + name + ")\\b", "gi");
console.log(text.replace(regexp, "_$1_"));
// _Harry_ is a suspicious character.
```

## International characters
- It is possible to use `\p` in a regular expression (that must have the Unicode option enabled) to match all characters to which the Unicode standard assigns a given property.
```js
console.log(/🍎{3}/.test("🍎🍎🍎"));    // false
console.log(/<.>/.test("<🌹>"));    // false
console.log(/<.>/u.test("<🌹>"));// true

console.log(/\p{Script=Greek}/u.test("α"));// true
console.log(/\p{Script=Arabic}/u.test("α"));// false
console.log(/\p{Alphabetic}/u.test("α"));// true
console.log(/\p{Alphabetic}/u.test("!"));// false
```

## Advanced
```js
//  \Y	    Match the Y'th captured group
var str = "blue+green+red=yellow."

var regexp1 = /blue(\W)green\1/;
console.log(regexp1.exec(str));
//  ["blue+green+", "+", index: 0, input: "blue+green+red=yellow.", groups: undefined]

var regexp2 = /green(\W)red\1/;
console.log(regexp2.exec(str));
//  null

// Assertions
/^dat$/.test('dat');    // true
/^dat$/.test('quangdat');   // false

/quang(?=dat)/.exec('quangdat');
    //    ["quang", index: 0, input: "quangdat", groups: undefined]
/quang(?=dat)/.exec('quangdaa');
    //  null
/(?:foo)bar\1/.test('foobarfoo');
    // false
/(?:foo)bar\1/.test('foobar\1');
    // true
    
//   \r  \n  \f (test in nodejs)
> console.log("stackoverflow\rnine");
//      ninekoverflow
> console.log("stackoverflow\fnine");
//      stackoverflow
//                          nine
> console.log("stackoverflow\nnine");
//      stackoverflow
//      nine
```
## References

https://eloquentjavascript.net/09_regexp.html

https://stackoverflow.com/questions/1324676/what-is-a-word-boundary-in-regexes

https://www.debuggex.com/cheatsheet/regex/javascript

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions