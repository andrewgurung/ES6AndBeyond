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
- Execute like a normal function but doesn't run the code in the Generator
- Instead produces an iterator that will control the generator to execute its code

```js
foo();
foo( 5, 10 ); // Passing arguments

// Executing a generator
var it = foo();
it.next();
```

### yield
- Keyword used inside generators to signal the pause point
- Can appear zero or more times
- `yield ..` can also be an expression that sends out a value  
  Eg: `yield Math.random();`

Consider:
In the following `*foo()` generator
- The first two lines (assignments) would run at the beginning
- `yield` would pause the generator
- When resumed, the last line would run

```js
function *foo(){
  var x = 10;
  var y = 20;

  yield;

  var z = x + y;
}
```

### yield *
- A very different mechanism, called `yield delegation`
- `yield * ..` requires an iterable
- Invokes that iterable's iterator, and delegates its own host generator's control to that iterator until it's exhausted

Consider:
- [1,2,3] value produces an iterator
- `*foo()` generator will yield those values out as it's consumed

```js
function *foo() {
    yield *[1,2,3];
}
```

### Iterator Control
- An iterator attached to a generator can be consumed with a `for..of` loop

```js
function *foo(){
  yield 1;
  yield 2;
  yield 3;
}

// 1. Manual iteration
var it = foo();
it.next();              // { value: 1, done: false }
it.next();              // { value: 2, done: false }
it.next();              // { value: 3, done: false }

it.next();              // { value: undefined, done: true }

// 2. Consume iterator attached to a generator
for(var v of foo()) {
  console.log( v );
}
// 1 2 3
```
