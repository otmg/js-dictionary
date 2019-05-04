# Higher order function

        Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.

## Taking functions as arguments
```js
function buildHouses(quantity, callback) {
    for (let i = 0; i < quantity; i++) {
        callback(i);
    }
}

function check(test, next) {
    if(test % 2 === 0) next();
}

buildHouses(7, function (n) {
    check(n, function () {
        console.log(n);
    });
});
    // expected output: 
    // 0
    // 2
    // 4
    // 6
```

## Returning functions as results
```js
function countDownFrom(from) {
    return function(check) {
        for (let i = from; i >= 0; i--) {
            if (check(i))
                console.log(i);
        }
    };
}

var countDownFrom10 = countDownFrom(10);
countDownFrom10(function(val) {
    return val % 2 == 0;
});

// expected output:
// 10
// 8
// 6
// 4
// 2
// 0
```
## References

https://eloquentjavascript.net/05_higher_order.html