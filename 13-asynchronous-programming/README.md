# 13. Asynchronous programming
    There can only be one currently running execution context
        (Because JavaScript is single threaded language)

## Promise
- The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.
```js
function getProductPromise() {
    return new Promise(function (resolve, reject) {
        if (! anyError) {
            resolve('this is a product');   // fulfilled
        } else {
            reject('this is an error');     // rejected
        }
    });
}

getProductPromise() // pending
    .then(function (product) {
        console.log(product);
        return getAnotherProductPromise();  // Chaining
    })
    .then(function (anotherProduct) {
        console.log(anotherProduct);
    })
    .catch(failureCallback);    // Error propagation

// start operations in parallel
Promise.all([ fn1(), fn2(), fn3()])
    .then(function (results) {
        for (let i of results) {
            console.log(i);
        }
    });

// nesting to limit the scope of catch statements
doSomething()
    .then(function () {
        return doSomthingElse()
            .then(function () {
                return doMorething();
            })
            .catch(failureCallback);
    })
    .then(function () {
        return doStuff();
    })
    .catch(anotherFailureCallback);
```

## async / await
- The async/await pattern is a syntactic feature of many programming languages that allows an asynchronous, **non-blocking** function (non-blocking the main thread) to be structured in a way similar to an ordinary synchronous function.
- Async functions are built on top of promises.
```js
async function getProductWithAsync() {
    const product = await getProductPromise();
    // do somthing with product
}
```

- Another benefit of the "async" keyword is it declare that the function "getProductWithAsync" return a promise
- That mean you can call :
```js
getProductWithAsync().then();
// or 
await getProductWithAsync();
```

```js
// the correct way to use async/await in parallel
async getBooksAndAuthor(authorId) {
    const bookPromise = bookModel.fetchAll();
    const authorPromise = authorModel.fetch(authorId);
    const book = await bookPromise;
    const author = await authorPromise;
    return {
        author,
        books: books.filter(book => book.authorId === authorId),
    };
}

// fetch a list of items one by one
async getAuthors(authorIds) {
    // WRONG, this will cause sequential calls
    // const authors = authorIds.map(id => await authorModel.fetch(id));
    // CORRECT
    const promises = authorIds.map(id => authorModel.fetch(id));
    const authors = await Promise.all(promises);
}

// Error handling 
// try {...} catch() {...}
async function getProductWithAsync() {
    try {
        const product = await getProductPromise();
        // do somthing with product
    } catch (err) {
        console.error("ERROR :{}    ", err);
    }
}

// using catch
const product = await getProductPromise()
    .catch(function (err) {
        console.error(err);
    });
```

## References
https://en.wikipedia.org/wiki/Async/await

https://hackernoon.com/javascript-async-await-the-good-part-pitfalls-and-how-to-use-9b759ca21cda

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

https://blog.grossman.io/how-to-write-async-await-without-try-catch-blocks-in-javascript/