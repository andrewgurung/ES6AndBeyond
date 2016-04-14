## Async Flow Control

### Promises
- Before ES6, the primary technique for async flow was the function `callback`
- Promises represent the future completion value from a potentially async task, normalizing behavior across sync and async boundaries
- Promise can only have one of two possible outcomes: fulfilled(Value: Fullfillment) or rejected(Value: Reason)
- Promise can only be resolved once making it an immutable value

### Making and Using Promises
- `Promise(..)` constructor is used to create a promise instance

```js
var p = new Promise( function pr(resolve, reject){
  //..
});
```
