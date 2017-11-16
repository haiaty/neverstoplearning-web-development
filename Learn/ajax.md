
# AJAX

The A in Ajax stands for asynchronous . That means sending the request (or rather receiving the response) is taken out of the normal execution flow.

Here is an analogy which hopefully makes the difference between synchronous and asynchronous flow clearer:

#### Synchronous

Imagine you make a phone call to a friend and ask him to look something up for you. Although it might take a while, you wait on the phone and stare into space, until your friend gives you the answer that you needed.

The same is happening when you make a function call containing "normal" code:

```javascript
function findItem() {
    var item;
    while(item_not_found) {
        // search
    }
    return item;
}

var item = findItem();

// Do something with item
doSomethingElse();
```


Even though findItem might take a long time to execute, any code coming after var item = findItem(); has to wait until the function returns the result.

#### Asynchronous

You call your friend again for the same reason. But this time you tell him that you are in a hurry and he should call you back on your mobile phone. You hang up, leave the house and do whatever you planned to do. Once your friend calls you back, you are dealing with the information he gave to you.

That's exactly what's happening when you do an Ajax request.

```javascript
findItem(function(item) {
    // Do something with item
});
doSomethingElse();

```

Instead of waiting for the response, the execution continues immediately and the statement after the Ajax call is executed. To get the response eventually, you provide a function to be called once the response was received, a callback (notice something? call back ?). Any statement coming after that call is executed before the callback is called.


# SOURCES

[https://stackoverflow.com/questions/14220321/how-to-return-the-response-from-an-asynchronous-call](https://stackoverflow.com/questions/14220321/how-to-return-the-response-from-an-asynchronous-call)
