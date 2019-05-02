# 07. Array

`typeof []    // "object"`

## Clone an array
- Shallow-cloning
```js
var numbers = [1, 2, 3, 4, [10, 11, 12]];

var nums = numbers.slice();

var nums = [...numbers];

var nums = Array.from(numbers);

var nums = [].concat(numbers);
```

- Deep clone
    (See chapter object)

## The "Array" object
### I. Properties
**Array.length**
- The Array constructor's length property whose value is 1.
```js
var arr = [];
arr.length = 12;
```

**get Array[@@species]**
- The constructor function that is used to create derived objects.
```js
Array[Symbol.species]; // function Array()
```

**Array.prototype**
- Allows the addition of properties to all array objects.
```js
typeof Array.prototype; // "object"
```

### II. Methods
**Array.from(*arrayLike*, *mapFn(val, index)*)**
- Creates a new Array instance from an array-like or iterable object.
```js
Array.from('123');  // ['1', '2', '3']
const range = (start, stop, step) => 
    Array.from(
        { length: (stop - start) / step },
        function(_, i) {
            return start + (i * step);
        });
range(0, 5, 1); // [0, 1, 2, 3, 4]
range('A'.charCodeAt(0), 'Z'.charCodeAt(0) + 1, 1).map(x => String.fromCharCode(x));    // ["A", ..., "Z"]

Array.isArray()
Returns true if a variable is an array, if not false.
Array.isArray(Array.prototype); // true
Array.prototype instanceof Array; // false

Array.of(*...args*)
Creates a new Array instance with a variable number of arguments, regardless of number or type of the arguments.
Array.of(7);       // [7] 
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
```

## Array instances
### I. Properties
**Array.prototype.constructor**
- Specifies the function that creates an object's prototype.

**Array.prototype.length**
- Reflects the number of elements in an array.
- writable

**Array.prototype[@@unscopables]**
- A symbol containing property names to exclude from a with binding scope.

### 2. Methods
### `MUTATOR METHODS`
These methods `modify` the array and return it:

**Array.prototype.copyWithin(*target*, *start = 0*, *end = arr.length*)**
- Copies a sequence of array elements within the array.
```js
[1, 2, 3, 4, 5].copyWithin(-2); // [1, 2, 3, 1, 2]
[1, 2, 3, 4, 5].copyWithin(0, 3);   // [4, 5, 3, 4, 5]
[1, 2, 3, 4, 5].copyWithin(0, 3, 4);    // [4, 2, 3, 4, 5]
[1, 2, 3, 4, 5].copyWithin(-2, -3, -1); // [1, 2, 3, 3, 4]
[].copyWithin.call({length: 5, 3: 1}, 0, 3);    // {0: 1, 3: 1, length: 5}

// ES2015 Typed Arrays are subclasses of Array
var i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2);  // Int32Array [3, 4, 5, 4, 5]
// On platforms that are not yet ES2015 compliant: 
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]
```
**Array.prototype.fill(*value*, *start*, *end*)**
- Fills all the elements of an array from a start index to an end index with a static value.
```js
[1, 2, 3, 4, 5].fill(4);               // [4, 4, 4, 4, 4]
[1, 2, 3, 4, 5].fill(5, 2);            // [1, 2, 5, 5, 5]
[1, 2, 3, 4, 5].fill(4, 1, 2);         // [1, 4, 3, 4, 5]
[1, 2, 3, 4, 5].fill(6, 2, 2);         // [1, 2, 3, 4, 5]
[1, 2, 3, 4, 5].fill(4, -3, -2);       // [1 , 2, 4, 4, 5]
[1, 2, 3, 4, 5].fill(2, 5, 8);         // [1, 2, 3, 4, 5]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}
// Objects by reference.
var arr = Array(3).fill({}) // [{}, {}, {}];
arr[0].hi = "hi"; // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
```

**Array.prototype.pop()**
- Removes the last element from an array and returns that element.
```js
var plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato'];
console.log(plants.pop());  // expected output: "tomato"
console.log(plants);    // expected output: Array ["broccoli", "cauliflower", "cabbage", "kale"]
```

**Array.prototype.push(*...elements*)**
- Adds one or more elements to the end of an array and returns the new length of the array.
```js
var animals = ['pigs', 'goats', 'sheep'];
console.log(animals.push('cows'));  // expected output: 4
console.log(animals);   // expected output: Array ["pigs", "goats", "sheep", "cows"]
```

**Array.prototype.reverse()**
- Reverses the order of the elements of an array in place — the first becomes the last, and the last becomes the first.
- Careful: reverse is destructive. It also changes the original array
```js
const a = [1, 2, 3];
console.log(a.reverse());   // expected output: [1, 2, 3]
console.log(a); // expected output: [3, 2, 1]
```

**Array.prototype.shift()**
- Removes the first element from an array and returns that element.
```js
var arr = [1, 2, 3];
console.log(arr.shift());  // expected output: 1    // first element
console.log(arr);   // expected output: Array [2, 3]
```

**Array.prototype.sort(*compareFunction(a, b)*)**
- Sorts the elements of an array in place and returns the array.
```js
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
    return a - b;   // a > b ? 1 : -1
});
console.log(numbers);   // expected output [1, 2, 3, 4, 5]
```

**Array.prototype.splice(*start*, *count*, *...items*)**
- Adds and/or removes elements from an array.
```js
var myFish = ['angel', 'clown', 'trumpet', 'sturgeon'];
var removed = myFish.splice(0, 2, 'parrot', 'anemone', 'blue');
// myFish now is ["parrot", "anemone", "blue", "trumpet", "sturgeon"] 
// removed is ["angel", "clown"]
myFish.splice(-2, 1);
// myFish is ["parrot", "anemone", "blue", "sturgeon"] 
myFish(1);
// myFish is ["parrot"] 
```

**Array.prototype.unshift(*...elements*)**
- Adds one or more elements to the front of an array and returns the new length of the array.
```js
var array1 = [1, 2, 3];
console.log(array1.unshift(4, 5));  // expected output: 5
console.log(array1);    // expected output: Array [4, 5, 1, 2, 3]
```

### `ACCESSOR METHODS`
These methods do `not modify` the array and return some representation of the array.

**Array.prototype.concat(*...arrays*)**
- Returns a new array that is this array joined with other array(s) and/or value(s).
```js
const letters = ['a', 'b', 'c'];
const num1 = [[1]];
const alphaNumeric = letters.concat(1, [2, 3], num1);
console.log(alphaNumeric); 
// results in ['a', 'b', 'c', 1, 2, 3, [1]]
```

**Array.prototype.includes(*value*, *from*)**
- Determines whether an array contains a certain element, returning true or false as appropriate.
```js
[1, 2, 3].includes(3, -1); // true
[1, 2, NaN].includes(NaN); // true
arr.includes(1, -100); // true
[1, 2, 3].includes(3, 3);  // false
```

**Array.prototype.indexOf(*element*, *from*)**
- Returns the first (least) index of an element within the array equal to the specified value, or -1 if none is found.
```js
var array = [2, 9, 9];
array.indexOf(2);     // 0
array.indexOf(7);     // -1
array.indexOf(9, 2);  // 2
array.indexOf(2, -1); // -1
array.indexOf(2, -3); // 0
```

**Array.prototype.join(*separator*)   // default: a comma (",")**
- Joins all elements of an array into a string.
```js
var a = ['Wind', 'Rain', 'Fire'];
a.join();      // 'Wind,Rain,Fire'
a.join(', ');  // 'Wind, Rain, Fire'
a.join('');    // 'WindRainFire'
```

**Array.prototype.lastIndexOf(*search*, *from*)**
- Returns the last (greatest) index of an element within the array equal to the specified value, or -1 if none is found.
- BACK to FRONT
```js
var numbers = [2, 5, 9, 2];
numbers.lastIndexOf(2);     // 3
numbers.lastIndexOf(7);     // -1
numbers.lastIndexOf(2, 3);  // 3
numbers.lastIndexOf(2, 2);  // 0
numbers.lastIndexOf(2, -2); // 0
numbers.lastIndexOf(2, -1); // 3
```

**Array.prototype.slice(*begin*, *end*)**
- Extracts a section of an array and returns a new array.
```js
var arr = [1, 2, 3, 4, 5];
arr.slice(2, 4);    // [3, 4]
arr.slice(2);   // [3, 4, 5]
arr.slice(-3, 4);   // [3, 4]
```

**Array.prototype.toString()**
- Returns a string representing the array and its elements. Overrides the Object.prototype.toString() method.
```js
var array1 = [1, 2, 'a', '1a'];
console.log(array1.toString()); // expected output: "1,2,a,1a"
```

**Array.prototype.toLocaleString()**
- Returns a localized string representing the array and its elements. Overrides the Object.prototype.toLocaleString() method.

### `ITERATION METHODS`

**Array.prototype.entries()**
- Returns a new Array Iterator object that contains the key/value pairs for each index in the array.
```js
var array1 = ['a', 'b', 'c'];
var iterator1 = array1.entries();
console.log(iterator1.next());  // expected output: {value: Array(2), done: false}
console.log(iterator1.next().value);    // expected output: Array [1, "b"]
// Using for of loop
```

**Array.prototype.every(*callback(element, index, array)*, *thisArg*) // thisArg when executing callback**
- Returns true if every element in this array satisfies the provided testing function.
```js
['0', true, [], {}].every(function (e, i, a) {
    return e;
}); // true
```

**Array.prototype.filter(*callback(element, index, array)*, *thisArg*)**
- Creates a new array with all of the elements of this array for which the provided filtering function returns true.
```js
[2, 4, 5, 7, 8].filter(function (val) {
    return (val % 2);
}); // [5, 7]
```

**Array.prototype.find(*callback(element, index, array)*, *thisArg*)**
- Returns the found value in the array, if an element in the array satisfies the provided testing function or undefined if not found.
```js
const inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];
const result = inventory.find( fruit => fruit.name === 'cherries' );
console.log(result) // { name: 'cherries', quantity: 5 }
```

**Array.prototype.findIndex(*callback(element, index, array)*, *thisArg*)**
- Returns the found index in the array, if an element in the array satisfies the provided testing function or -1 if not found.
```js
const fruits = ["apple", "banana", "cantaloupe", "blueberries", "grapefruit"];
const index = fruits.findIndex(fruit => fruit === "blueberries");
console.log(index); // 3
console.log(fruits[index]); // blueberries
```

**Array.prototype.forEach(*callback(element, index, array)*)**
- Calls a function for each element in the array.
```js
const items = ['item1', 'item2', 'item3'];
const copy = [];
items.forEach(function(item){
    copy.push(item)
});
```

**Array.prototype.keys()**
- Returns a new Array Iterator object that contains the keys for each index in the array.
```js
var arr = ['a', , 'c'];
var sparseKeys = Object.keys(arr);
var denseKeys = arr.keys();
console.log(sparseKeys); // Array ['0', '2']
console.log(denseKeys);  // Array Iterator {}
console.log([...denseKeys]);    // Array [0, 1, 2]
```

**Array.prototype.map(*callback(element, index, array)*)**
- Creates a new array with the results of calling a provided function on every element in this array.
```js
var numbers = [1, 4, 9];
var doubles = numbers.map(function(num) {
    return num * 2;
});
// doubles is now [2, 8, 18]
// numbers is still [1, 4, 9]
['1.1', '2.2e2', '3e300'].map(Number); // [1.1, 220, 3e+300]
```

**Array.prototype.reduce(*callback(accumulator, currentValue, currentIndex, array)*, *initialValue*)**
- Apply a function against an accumulator and each value of the array (from left-to-right) as to reduce it to a single value.
- initialValue:   Value to use as the first argument to the first call of the callback.
```js
const arr = ['cat', 'mouse', 'cat', 'pig', 'buffalo', 'mouse',  'pig', 'cat'];
arr.reduce(function (count, item) {
    count[item] = (count[item] || 0) + 1;
    return count;
}, {}); // {cat: 3, mouse: 2, pig: 2, buffalo: 1}
```

**Array.prototype.reduceRight(*callback(accumulator, currentValue, currentIndex, array)*, *initialValue*)**
- Apply a function against an accumulator and each value of the array (from right-to-left) as to reduce it to a single value.
```js
var flattened = [[0, 1], [2, 3], [4, 5]].reduceRight(function(a, b) {
    return a.concat(b);
}, []); // flattened is [4, 5, 2, 3, 0, 1]
```

**Array.prototype.some(*callback(element, index, array)*, *thisArg*)**
- Returns true if at least one element in this array satisfies the provided testing function.
```js
[1, 2, 3, 4, 5, 6].some(function (val) {
    return val > 3;
}); // true
```

**Array.prototype.values()**
- Returns a new Array Iterator object that contains the values for each index in the array.
**Array.prototype.values === Array.prototype[Symbol.iterator]**
```js
const arr = ['a', 'b', 'c'];
const iterator = arr.values();
for (let val of iterator) {
    console.log(val); // expected output: "a" "b" "c"
}
```
**Array.prototype\[@@iterator\]\(\)**
- Returns a new Array Iterator object that contains the values for each index in the array.

## References
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array