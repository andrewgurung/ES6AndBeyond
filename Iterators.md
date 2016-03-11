## Iterators
- ES6 has several important features that help significantly improve properly organize JavaScript code, including:
  - Iterators
  - Generators
  - Modules
  - Classes
- ES6 has introduced an implicit standardized interface for iterators
- Many built-in data structures will expose an iterator

> Organization: Iterators are a way of organizing ordered, sequential, pull-based consumption of data

### Interfaces

1. `Iterator` Interface
```
Iterator [required]
    next() {method}: retrieves next IteratorResult
```

2. `IteratorResult` Interface
  - value {property}: current iteration value or final return value  (optional if `undefined`)
  - done {property}: boolean, indicates completion status

  ```
  { value: .. , done: true / false }
  ```

### next() Iteration
- Each time `Symbol.iterator` is invoked, a new fresh iterator is produced
- `it` iterator object doesn't return `done: true` until it goes beyond the array/collection
  - Considered a best practice to return `{ value: undefined, done: true}`
- Primitive string value is boxed implicitly to String object wrapper form which is an `iterable`
- New data structures, called collections eg: `Map` provides API method(s) to generate an iterator
  - `.entries()`

```js
// 1. Iterating an array
var arr = [1,2,3];
var it = arr[Symbol.iterator]();
it.next(); // {value: 1, done: false}
it.next(); // {value: 2, done: false}
it.next(); // {value: 3, done: false}
// Out of bound
it.next(); // {value: undefined, done: true}

// 2. Iterating a string Primitive
var str = "Hel";
var it1 = str[Symbol.iterator]();
it1.next(); // {value: "Y", done: false}
it1.next(); // {value: "a", done: false}
it1.next(); // {value: undefined, done: true}

// 3. Map Collection
var map = new Map();
map.set( "foo", 42 );
map.set ( { cool: true }, "hello world" );
var it2 = map[Symbol.iterator]();
var it3 = map.entries();
console.log(it2.next()); // { value:[ "foo", 42 ],  done:false}
console.log(it3.next()); // { value:[ "foo", 42 ],  done:false}
```
