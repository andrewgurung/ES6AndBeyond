## Async Flow Control

### Promises
- Before ES6, the primary technique for async flow was the function `callback`
- Promises represent the future completion value from a potentially async task, normalizing behavior across sync and async boundaries
- Promise can only have one of two possible outcomes: fulfilled(Value: Fullfillment) or rejected(Value: Reason)
- Promise can only be resolved once making it an immutable value

### Making and Using Promises
- `Promise(..)` constructor is used to create a promise instance
- `Promise(..)` constructor takes a single function `pr(..)` which is called immediately
- `pr(..)` function receives two control functions as arguments
  - `resolve(..)`
  - `reject(..)`

```js
var p = new Promise( function pr(resolve, reject){
  //..
});
```

### Code to be refactored

- `ajax(..)` utility (error-first style callback)

  ```js
  function ajax(url, cb) {
    // Make request to url
    // Eventually call the cb(..)
  }

  ajax( "http://random.url", function handler(err, contents) {
    if (err) {
      // handle ajax error
    }
    else {
      // handle `contents` success
    }
  });
  ```

### Convert to `promise`

  ```js
  function ajax(url) {
    return new Promise( function pr(resolve, reject){
      // make request, eventually call
      // either `resolve(..)` or `reject(..)`
    });
  }

  ajax("http://random.url")
  .then(
    function fullfilled(contents){
      // handle `contents` success
    },
    function rejected(reason) {
      // handle ajax errors
    }
  );
  ```

- Promises have a `then(..`) method that accepts one or two callbacks
    - First function (if present): Treated as the handler if promise is fulfilled successfully. Else use `null` as placeholder for empty success
    - Second function (if present): Treated as the handler if promise is rejected
- Both then(..) and catch(..) automatically construct and return another promise instance
- Short hand for calling `.then(null, handleRejection)` is `catch(handleRejection)`
```js
// Return promise through `ajax(..)` helper function
ajax("http://random.url")
.then(
  function fulfilled(contents) {
    return contents.toUpperCase();
  },
  function rejected(reason) {
    return "Default Value";
  }
)
.then( function fulfilled(data){
  // handle data from original promise's handlers
})

```

### Thenables
- Any object with a `then(..)` function on it
- `Thenables` are promise-like objects that can interoperate with the Promise mechanism
- Prior to ES6, there was never any special reservation for methods called `then(..)`. Use caution

Consider a misbehaving thenable:
- When normal Promises are supposed to only be resolved once, the following thenable fulfillment handler is called repeatedly

```js
var th = {
  then: function thener(fulfilled) {
    // call fulfilled(..) once every 100ms forever
    setInterval( fulfilled, 100 );
  }
}
```

### `Promise` API
Static Promise methods:

1. `Promise.resolve(..)`
  - Solution to the thenable trust issue
  - var p1 = Promise.resolve( 42 );

2. `Promise.reject(..)`
  - Creates an immediately rejected promise
  - var p1 = Promise.reject( "Oops" );

3. `Promise.all([ .. ])`
  - Accepts an array of one or more values
  - Waits for all fulfillments (or the first rejection)

4. `Promise.race([ .. ])`
  - Accepts an array of one or more values
  - Waits only for either the first fulfillment or rejection
