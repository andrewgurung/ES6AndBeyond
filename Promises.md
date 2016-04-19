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

### Code to refactored

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

- Convert to `promise`

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
