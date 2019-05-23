# 02. Operator

  - [JavaScript Operator Precedence Values](#javascript-operator-precedence-values)
  - [Arithmetic](#arithmetic)
  - [Comparison operators](#comparison-operators)
  - [Unary & Binary & Ternary](#unary--binary--ternary)
  - [Logical operators](#logical-operators)
  - [Automatic type conversion](#automatic-type-conversion)
  - [Short-circuiting of logical operators](#short-circuiting-of-logical-operators)
  - [typeof operator](#typeof-operator)
  - [delete operator](#delete-operator)
  - [References](#references)

## JavaScript Operator Precedence Values
|	OPERATOR  |			NAME			|		EXAMPLE		|
|:-----------:|:---------------------------:|:-------------:|
|	( )    |  Expression grouping		|    (3 + 4)  |
|-----------|---------------------------|-------------|
|	.		|	Member					|	person.name		|
|	[]		|		Member				|	person["name"]	|
|	()		|		Function call		|	myFunction()	|
|	new		|		Create				|	new Date()		|
|-----------|---------------------------|-------------------|
|	<<	    |	Shift left				|	x << 2	|
|	>>	    |	Shift right				|	x >> 2	|
|	>>>	    |	Shift right (unsigned)	|	x >>> 2	|
|-----------|---------------------------|-----------|
|	in  		|	Property in Object			|	"PI" in Math		|
|	instanceof  |	Instance of Object			|	instanceof Array	|
|-----------|---------------------------|-------------------------------|
|	&	    |		Bitwise AND			|	x & y		|
|	^	    |		Bitwise XOR			|	x ^ y		|
|   &#124;  |		Bitwise OR			|	x &#124; y	|
|-----------|---------------------------|---------------|
|	+=	   |  Assignment			|	x += y		|
|	-=	   |  Assignment			|	x -= y		|
|	*=	   |  Assignment			|	x *= y		|
|	%=	   |  Assignment			|	x %= y		|
|	<<=	   |  Assignment			|	x <<= y		|
|	>>=	   |  Assignment			|	x >>= y		|
|	>>>=   |  Assignment			|	x >>>= y	|
|	&=	   |  Assignment			|	x &= y		|
|	^=	   |  Assignment			|	x ^= y		|
|  &#124;= |  Assignment			|	x &#124;= y	|
|-----------|---------------------------|-----------|
|	yield	|  Pause Function		|	yield x	|
|	,	   	|  Comma				|	5 , 6	|
|-----------|-----------------------|-----------|

## Arithmetic
```js
+ - * / --> easy
**  --> Exponentiation (ES6)
++  --> Postfix/Prefix Increment
--  --> Postfix/Prefix Decrement
%   --> Remainder
()  --> Expression grouping (Priority)
/*
Note: ++num   // increase before expressing
        num++ // express, then increasing
*/
```

## Comparison operators
```js
/*
<   >   ==  !=  <=  >=      // easy
===     // Strict equal (equal type and equal value)
!==     // Strict unequal
*/

1 > 3   // --> false
'Aardvark' < 'Zoroaster' // --> true
NaN == NaN  //	--> false
```


## Unary & Binary & Ternary
- Unary : 1 value
    `Ex: -(1)`
- Binary : 2 values
    `Ex: (1 + 2)`
- Ternary : 3 values
```js
1st ? 2nd : 3rd
condition ? true : false
Ex: true ? 1 : 0;   // --> 1
    false ? 1 : 0;  // --> 0
    console.log(true ? 'yes' : 2);  // --> yes
```

## Logical operators
```js
&& -- and
|| -- or
!  -- not

Ex: 1 + 1 == 2 && 10 * 10 > 50  // --> true
Note:   NaN == NaN  // false
```

## Automatic type conversion
-	JavaScript is a **loosely typed** language, which means that whenever an operator or statement is expecting a particular data-type, JavaScript will automatically convert the data to that type.
-	JavaScript values are often referred to as being `truthy` or `falsy`, according to what the result of such a conversion would be (i.e. true or false).
-	In fact there are only six falsy values:

	        false   undefined   null    0 (numeric zero)    "" (empty string)   NaN (Not A Number)

```js
8 * null   // --> 0
"5" - 1    // --> 4
"5" + 1    // --> 51
"five" * 2     // --> NaN
false == 0     // --> true
null == undefined     // --> true
null == 0     // --> false
0 == false  // --> true
"" == false // --> true
"" === false        // --> false
```

## Short-circuiting of logical operators
-	The **&&** and **||** operators **short-circuit**, meaning they don't evaluate the right hand side if it isn't necessary.
-	They will return either the original left-hand value or the right-hand value.

## typeof operator
```js
console.log(typeof "Hello");    // --> "string"
```

## delete operator
The [delete operator (See MDN)][delete-operator] removes a given property from an object.
On successful deletion, it will return true, else false will be returned.

-   If the property which you are trying to delete does **not exist**, return **true**
-   If a property with the same name exists on the object's prototype chain, then, after deletion, the object will use the **property from the prototype chain**.
-   Any property declared with var cannot be deleted from the global scope or from a function's scope.
    -   As such, delete cannot delete any functions in the global scope.
    -   Functions which are part of an object (apart from the global scope) can be deleted with delete.
-   Any property declared with let or const cannot be deleted from the scope within which they were defined.
-   **Non-configurable** properties cannot be removed. (see Object)

## References
https://www.sitepoint.com/automatic-type-conversion/

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete

[delete-operator]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete