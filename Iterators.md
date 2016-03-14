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

### Custom Iterators
- Return an iterator object with `next()` and `return()` methods on it

Consider an iterator that produces infinite series of Fibonacci sequence

```js
var Fib = {
  [Symbol.iterator]() {
    var n1 = 1, n2 = 1;

    return {
      // make the iterator an iterable
      [Symbol.iterator]() { return this;  },

      next() {
        var current = n2;
        n2 = n1;
        n1 = n1 + current;
        return { value: current, done: false };
      },

      return(v) {
        console.log( "Fib seq completed" );
        return { value: v, done: true };
      }
    };
  }
};

for (var v of Fib) {
  console.log( v );
  if (v > 50) break;
}
// 1 1 2 3 5 8 13 21 34 55
// Fib seq completed
```

### Iterator Consumption
- ES6 has structures other than `for..of` loop to consume Iterator

```js
var a = [1,2,3,4,5];

// 1. ... spread operator fully exhausts an iterator
function foo(x,y,z,w,p) {
  console.log(x + y + z + w + p);
}
foo( ...a ); // 15

// 2. spread an iterator inside an array
var b = [ 0, ...a, 6 ];
b; // [0,1,2,3,4,5,6]

// 3. Array destructuring
var it = a[Symbol.iterator]();

var [x,y] = it;
var [z, ...w] = it;

// is `it` fully exhausted? YES
it.next(); // { value: undefined, done: true }

console.log( x ); // 1
console.log( y ); // 2
console.log( z ); // 3
console.log( w ); // [4,5]
```
