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
