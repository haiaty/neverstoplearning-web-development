

# JAVASCRIPT

JavaScript runs in the UI thread of the browser and any long running process will lock the UI, making it unresponsive. Additionally, there is an upper limit on the execution time for JavaScript and the browser will ask the user whether to continue the execution or not.

 three available solutions for hanfling async operations in current browsers, and node 7+:
 
 
#### Promises with async/await (ES2017+, available in older browsers if you use a transpiler or regenerator)

The new ECMAScript version released in 2017 introduced syntax level support for asynchronous functions. With the help of async and await, you can write asynchronous in a "synchronous style". Make no mistake though: The code is still asynchronous, but it's easier to read/understand.

async/await builds on top of promises: an async function always returns a promise. await "unwraps" a promise and either result in the value the promise was resolved with or throws an error if the promise was rejected.

Important: You can only use await inside an async function. That means that at the very top level, you still have to work directly with the promise.


Here is an example that builds on top of delay above:

```javascript
// Using 'superagent' which will return a promise.
var superagent = require('superagent')

// This is isn't declared as `async` because it already returns a promise
function delay() {
  // `delay` returns a promise
  return new Promise(function(resolve, reject) {
    // Only `delay` is able to resolve or reject the promise
    setTimeout(function() {
      resolve(42); // After 3 seconds, resolve the promise with value 42
    }, 3000);
  });
}


async function getAllBooks() {
  try {
    // GET a list of book IDs of the current user
    var bookIDs = await superagent.get('/user/books');
    // wait for a second (just for the sake of this example)
    await delay(1000);
    // GET information about each book
    return await superagent.get('/books/ids='+JSON.stringify(bookIDs));
  } catch(error) {
    // If any of the awaited promises was rejected, this catch block
    // would catch the rejection reason
    return null;
  }
}

// Async functions always return a promise
getAllBooks()
  .then(function(books) {
    console.log(books);
  });

```
  
Newer browser and node versions support async/await. You can also support older environments by transforming your code to ES5 with the help of regenerator (or tools that use regenerator, such as Babel).
 
#### Callbacks (popular in node)

A callback is simply a function passed to another function. That other function can call the function passed whenever it is ready. In the context of an asynchronous process, the callback will be called whenever the asynchronous process is done. Usually, the result is passed to the callback.

In the example in the question, you can make foo accept a callback and use it as success callback. So this

```javascript
var result = foo();
// Code that depends on 'result'
becomes

foo(function(result) {
    // Code that depends on 'result'
});
Here we defined the function "inline" but you can pass any function reference:

function myCallback(result) {
    // Code that depends on 'result'
}

foo(myCallback);
foo itself is defined as follows:

function foo(callback) {
    $.ajax({
        // ...
        success: callback
    });
}
```

callback will refer to the function we pass to foo when we call it and we simply pass it on to success. I.e. once the Ajax request is successful, $.ajax will call callback and pass the response to the callback (which can be referred to with result, since this is how we defined the callback).

You can also process the response before passing it to the callback:

```javascript
function foo(callback) {
    $.ajax({
        // ...
        success: function(response) {
            // For example, filter the response
            callback(filtered_response);
        }
    });
}
```

It's easier to write code using callbacks than it may seem. After all, JavaScript in the browser is heavily event-driven (DOM events). Receiving the Ajax response is nothing else but an event.
Difficulties could arise when you have to work with third-party code, but most problems can be solved by just thinking through the application flow.


#### Promises with then() (ES2015+, available in older browsers if you use one of the many promise libraries)

The Promise API is a new feature of ECMAScript 6 (ES2015), but it has good browser support already. There are also many libraries which implement the standard Promises API and provide additional methods to ease the use and composition of asynchronous functions (e.g. bluebird).

Promises are containers for future values. When the promise receives the value (it is resolved) or when it is canceled (rejected), it notifies all of its "listeners" who want to access this value.

The advantage over plain callbacks is that they allow you to decouple your code and they are easier to compose.

Here is a simple example of using a promise:

```javascript
function delay() {
  // `delay` returns a promise
  return new Promise(function(resolve, reject) {
    // Only `delay` is able to resolve or reject the promise
    setTimeout(function() {
      resolve(42); // After 3 seconds, resolve the promise with value 42
    }, 3000);
  });
}

delay()
  .then(function(v) { // `delay` returns a promise
    console.log(v); // Log the value once it is resolved
  })
  .catch(function(v) {
    // Or do something else if it is rejected 
    // (it would not happen in this example, since `reject` is not called).
  });
Applied to our Ajax call we could use promises like this:

function ajax(url) {
  return new Promise(function(resolve, reject) {
    var xhr = new XMLHttpRequest();
    xhr.onload = function() {
      resolve(this.responseText);
    };
    xhr.onerror = reject;
    xhr.open('GET', url);
    xhr.send();
  });
}

ajax("/echo/json")
  .then(function(result) {
    // Code depending on result
  })
  .catch(function() {
    // An error occurred
  });
  ```
  
Describing all the advantages that promise offer is beyond the scope of this answer, but if you write new code, you should seriously consider them. They provide a great abstraction and separation of your code.


## SOURCES

[https://stackoverflow.com/questions/14220321/how-to-return-the-response-from-an-asynchronous-call](https://stackoverflow.com/questions/14220321/how-to-return-the-response-from-an-asynchronous-call)
