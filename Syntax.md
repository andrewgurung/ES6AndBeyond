## Syntax

Try out ES6 syntax with the following tools
- ES6Fiddle (http://www.es6fiddle.net/)
- Babel transpiler (http://babeljs.io/repl/)

## Block-Scoped Declarations
- Before ES6, the fundamental unit of variable scoping has been the `function`
- Other than function declaration, immediately invoked function expression were used to create block scope

Consider:
```js
var a = 2;

(function IIFE(){
  var a = 3;
  console.log( a ); // 3
})();
console.log( a ); // 2
```

### `let` Declarations
- Use `let` and a pair of `{..}` to create a scope
- Always put `let` declaration(s) at the very top of a dedicated `{..}` block
- `let` declarations are not initialized until they appear in the block
- `let` variables are not `hoisted`

```js
var a = 2;

{
  // 1. Block-Scoped `let` declaration
  let a = 3;
  console.log( a ); // 3
}

console.log( a ); // 2

// 2. Stylistic Choice
//    let in the same line as `{`
//    Multiple declaration with single `let`
{ let a = 2, b, c;
  //..
}

// 3. Temporal Dead Zone (TDZ) error
//    Accessing too-early `let` declared references
{
  console.log( foo ); // undefined
  console.log( bar ); // ReferenceError
  var foo;
  let bar;
}
```

#### `let` + `for`
  - The `let` in `for` loop header redeclares a new `i` for each iteration
  - Whereas `var` will only have a single `i` closed over in the outer scope

  Using `var`
  ```js
  var funCollection = [];
  for (var i = 0; i < 5; i++) {
    funCollection.push( function() {
      console.log( i );
    });
  }
  funCollection[3](); // 5
  ```

  Consider using `let`:
  ```js
  var funCollection = [];
  for (let i = 0; i < 5; i++) {
    funCollection.push( function() {
      console.log( i );
    });
  }
  funCollection[3](); // 3
  ```

### `const` Declarations
- A variable that is read-only after its initial value is set
- Assignment is Frozen, **NOT** the value itself
  Eg: Complex value such as object or array can still  be modified
- Block-scoped

```js
{
  const a = 2;
  console.log( a ); // 2
  // 1. Reassignment error
  a = 3; //TypeError

  // 2. Array as constant: modified
  const arr1 = [1, 2, 3];
  arr1.push( 4 );
  console.log( arr1 ); // [1,2,3,4]
  a = 42; //Type Error
}
```

### Block-scoped Functions
- Functions declared inside a `{..}` block is blocked-scoped there
- Unlike `let` declarations, block-scoped functions are `hoisted`

```js
{
  foo(); // Foo Printed

  function foo() {
    console.log( "Foo Printed!" );
  }
}

foo(); // ReferenceError
```

## Spread / Rest
- New `...` operator is referred as `spread` or `rest` operator depending on where/how it's used
- Solid alternative to deprecated so called `arguments` array

### Spread
- Used infront of an array to spread/expand out into its individual values

```js
function foo(x, y, z) {
  console.log( x, y, z );
}

// 1. Spread out individual values
foo(...[1,2,3]); // 1 2 3

// 2. Pre-ES6 syntax
foo.apply( null, [1,2,3] ); // 1 2 3

// 3. Spread out individual values
var a = [2,3,4];
var b = [ 1, ...a, 5 ];
console.log( b ); // 1,2,3,4,5
```

### Rest
- Collecting the rest of the parameters into an array

```js
// Gather rest of the arguments to `z` variable as an array
function foo(x, y, ...z) {
  console.log( x, y, z );
}

foo( 1, 2, 3, 4, 5 ); // 1 2 [3,4,5]
```

## Default Parameter Values
- Functions can have default parameter values
- Syntax: `fn(param = value, ..)`

Consider Pre-ES6
```js
function foo(x,y) {
  x = x || 11;
  y = y || 31;
  console.log( x + y );
}
foo();              // 42
foo( null, 6 );     // 17
foo( 0, 42 );       // 53 <-- Oops, not 42

function bar(x,y) {
    x = (x !== undefined) ? x : 11;
    y = (y !== undefined) ? y : 31;

    console.log( x + y );
}

bar( 0, 42 );           // 42
bar( undefined, 6 );    // 17
```

Using Default Parameter Values
```js
function foo(x = 11, y = 31) {
  console.log( x + y );
}
foo();               // 42
foo( null, 6 );      // 6 <-- Oops, not 17. null coerces to 0
foo( 0, 42 );        //42
foo( undefined, 6 );  // 17
```

### Default Value Expressions
- Functions can have default values other than simple primitives like `31`
- They can be any valid expression, even a function call
- Default Expressions are lazily evaluated only if parameter's argument is missing or is `undefined`
- Subtle detail: Default expressions first matches the formal parameter's scope `(..){..} bubble` before looking to an outer scope

```js
function bar(val) {
  console.log( "Bar Called!" );
  return y + val;
}

function foo(x = y + 3, z = bar( x )) {
  console.log( x, z );
}

var y = 5;
foo();  // Bar Called!
        // 8 13

foo( 10 ); // Bar Called!
          // 10 15

y = 6;
foo( undefined, 10 ); // Bar Called!
                      // 9 10
```

## Destructuring
- Decomposing array and objects into separate variable assignments
- `[a, b, c]` or `{ x: x, y: y, z: z }` on the left hand side of the `=` assignment
- Eliminates need of temporary variable
- Can be thought as `structured assignment`

Before ES6:

```js
function foo() {
  return [1, 2, 3];
}

// Temporary variable to hold array
var tempArray = foo();
var a = tempArray[0],
    b = tempArray[1],
    c = tempArray[2];
console.log( a, b, c ); // 1 2 3

function bar() {
  return {
    x: 4,
    y: 5,
    z: 6
  }
}
// Temporary variable to hold Object
var tempObject = bar();
var x = tempObject.x,
    y = tempObject.y,
    z = tempObject.z;

console.log( x, y, z );  // 4 5 6
```

ES6 Destructuring:
```js
function foo() {
  return [1, 2, 3];
}

// 1. Array Destructuring
var [a, b, c] = foo();
console.log( a, b, c ); // 1 2 3

function bar() {
  return {
    x: 4,
    y: 5,
    z: 6
  }
}
// 2. Object Destructuring
var {x: x, y: y, z: z} = bar();
console.log( x, y, z );  // 4 5 6
```

### Object Destructuring: Property Assignment Pattern
- Instead of `target <-- source`, the pattern for object destructing is `source --> target`
- Train your brain for a flipped syntactic pattern
- If the source and target property names matches, you can use short-hand technique

```js
function bar() {
  return {
    // Regular object assignment is always target <-- source
    x: 4,
    y: 5,
    z: 6
  }
}

// 1. Flipped pattern
//    Source --> Target. Example   x:bam
var {x: bam, y: baz, z: bap} = bar();
console.log( bam, baz, bap ); // 4 5 6
console.log( x, y , z );  // ReferenceError

// 2. Short-hand assignment
var {x, y, z} = bar();
console.log( x, y , z ); // 4 5 6
```


## Object Literal Extensions
## Template Literals
## Arrow Functions
## for..of Loops
## Regular Expression Extensions
## Number Literal Extensions
## Unicode
## Symbols
