## Generators
- Locally pause/resume capable functions controlled by an iterator
- Can be used to programmatically (and interactively, through `yield`/`next`(..) message passing) generate values to be consumed via iteration

### Syntax
- The position of `*` is not functionally relevant

```js
function *foo(){
  //..
}

function *foo()  { .. }
function* foo()  { .. }
function * foo() { .. }
function*foo()   { .. }
```

### Executing a Generator
