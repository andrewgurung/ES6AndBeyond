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
  return yji + val;
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

### Assignment, Mappings/Transformations and Swapping
- Destructuring is a general operation, not just declaration
- The purpose of destructuring is not just less typing, but more declarative readability
- Object to Array Mapping
- Array to Object Mapping
- Swapping without temporary variable

```js
function foo() {
  return [1, 2, 3];
}

function bar() {
  return {
    // Regular object assignment is always target <-- source
    x: 4,
    y: 5,
    z: 6
  }
}

// 1. Declarations
var a, b, c, x, y, z;

// 2. Array assignment
[a,b,c] = foo();
console.log( a, b, c ); // 1 2 3

// 3. Object assignment
// Must include `(..)` so that the inner `{..}` isn't taken as a block statement instead of an object
({x, y, z} = bar());
console.log( x, y, z); // 4 5 6

// 4. Object to Array Mapping
var obj1 = {x: 4, y: 5, z: 6};
var arr1 = [];
// source --> target
({x: arr1[0], y: arr1[1], z: arr1[2]} = obj1);
console.log(arr1); // [4, 5, 6]

// 5. Array to Object Mapping
var arr1 = [4, 5, 6];
var obj2 = {};

[obj2.x, obj2.y, obj2.z] = arr1;
console.log(obj2); //  {x: 4, y: 5, z: 6}

// 6. Swapping
var five = 5;
var ten = 10;
[ten, five] = [five, ten];
console.log(five); // 10
console.log(ten); // 5

// 7. Spreading
var spreadTest = [2,3,4];
var [b, ...c] = spreadTest;
console.log( b );     // 2
console.log( c );     // [3,4]
```

### Destructuring Assignment Expression
- Assignment expression with object or array destructuring value is the full righthand object/array value
- Helps in chain destructuring assignment expressions
- Default value assignment

```js
var o = { a:1, b:2, c:3 },
    a, b, c, p;

p = {a, b, c} = o;
// `p` and `o` points to the same reference
console.log( a, b, c );     // 1 2 3
console.log( p === o );     // true

// Chain destructuring assignment
var arr1 = [4, 5, 6],
    x, y, z,
    o = { a:1, b:2, c:3 };
( {a} = {b,c} = o );
[x,y] = [z] = arr1;

console.log( a, b, c );         // 1 2 3
console.log( x, y, z );         // 4 5 4

// Default value assignment
var [ a = 3, b = 6, c = 9, d = 12 ] = arr1;
console.log( a, b, c, d );    // 4 5 6 12

// Flatten out object namespace
var App = {
  model: {
    User: function() {}
  }
}

// instead of: var User = App.model.User;
var { model: { User } } = App;
```

### Destructuring Parameters
```js
// 1. Array Destructuring for parameters including default value
function foo( [ x, y, z = 10 ] ) {
  console.log( x, y, z);
}

foo( [1, 2, 3] ); // 1 2 3
foo( [1] ); // 1 undefined 10

// 2. Object Destructuring for parameters
function bar( { x, y, z = 10 } ) {
  console.log( x, y, z);
}

bar( { x: 1, y: 2, z: 3} ); // 1 2 3
bar( { x: 1 } ); // 1 undefined 10

// 3. Possible variation with spread
function f3([ x, y, ...z ], ...w) {
  console.log( x, y, z, w );
}

f3( [] ); // undefined undefined [] []
f3( [1,2,3,4], 5, 6 ); // 1 2 [3, 4] [5, 6]
```

## Object Literal Extensions
- ES6 adds a bunch of extensions to its `{..}` object Literal

### Concise Properties
The old way:

```js
var x = 2, y = 3,
    o = {
      x: x,
      y: y
    };
```

The ES6 way:

```js
var x = 2, y = 3,
    o = {
      x,
      y
    };
```

### Concise Methods
The old way:

```js
var o = {
  x: function() {

  },
  y: function() {

  }
};
```

The ES6 way:

```js
var o = {
  x() {

  },
  y() {

  }
}
```

### Concise Methods: Anonymous Function
- Concise methods are anonymous functions
- Best Practice: Use them if you never need recursion or event binding

Traditional way
```js
// Subtract the smaller from the bigger number
function runSomething(o) {
    var x = Math.random(),
        y = Math.random();

    return o.something( x, y );
}

var x = runSomething( {
    something: function something(x,y) {
        if (x > y) {
            // recursively call with `x`
            // and `y` swapped
            return something( y, x );
        }

        return y - x;
    }
} );

console.log(x); // 0.2334036862011999
```

Concise Method:
```js
function runSomething(o) {
    var x = Math.random(),
        y = Math.random();

    return o.something( x, y );
}

var x = runSomething( {
    something(x,y) {
        if (x > y) {
            // recursively call with `x` and `y` swapped
            return something( y, x );
        }

        return y - x;
    }
} );

console.log(x); // Reference Error
```

The above ES6 snippet is interpreted as:
- The recursive call `return something( y, x )` cannot find the `something` due to anonymous function name

```js
runSomething( {
    something: function(x,y){
        if (x > y) {
            return something( y, x );
        }

        return y - x;
    }
} );
```

### Computed Property Names
- Specify an expression whose computed value is the property name

```js
var prefix = "user_";

var o = {
  baz: function() {},
  [ prefix + "foo" ]: function() { console.log( "USER FOO" ) },
  [ prefix + "bar" ]: function() {}
};

o.user_foo(); // USER FOO
```

### Setting `[[Prototype]]`
- Setting `[[Prototype]]` of an object as a part of object literal declaration
- Alternate Approach: Use `Object.setPrototypeOf(..)`
- Warning: Do not use o.__proto__ since that form is both a getter and setter

```js
var o1 = { };

var o2 = {
  __proto__: o1
};
```

### Object `super`
- `super` is only allowed in concise methods
- Must be of `super.xxx` form (property/method access), not `super()` form

```js
var o1 = {
  foo() {
    console.log( "o1:foo" );
  }
};

var o2 = {
  foo() {
    super.foo();
    console.log( "o2:foo" );
  }
};

Object.setPrototypeOf( o2, o1 );
o2.foo();     // o1:foo
              // o2:foo
```

## Template Literals
- New type of string literal with the `` ` `` backtick as delimiter
- Instead of templating, we are interpolating (parsing and evaluating)
- `` `..` `` string literal is automatically evaluated inline

```js
var name = "Jon";

// 1. String literal
var greeting = `Hello ${name}!`;
console.log( greeting );        // Hello Jon!
console.log( typeof greeting ); // string

// 2. Multiple lines: Preserving line breaks
var text =
`Now is the time for all good men
to come to the aid of their
country!`;

console.log( text );
/* Output
--------------------------------
Now is the time for all good men
to come to the aid of their
country!
*/
```

### Interpolated Expression
- Any valid expression is allowed inside `${..}` in an interpolated string literal including nested interpolated string literals
- Interpolated string literal is just lexically scoped where it appears

```js
function upper(s) {
  return s.toUpperCase();
}

var who = "reader";

var text =
`A very ${upper( "warm" )} welcome
to all of you ${upper(`${who}s`)}!
`;

console.log( text );
/* Output
--------------------------------
A very WARM welcome
to all of you READERS!
*/
```

## Arrow Functions
- Frustrations associated with `this`-based programming is the motivation for new ES6 `=>` arrow function
- Arrow function definition consists of
  - A parameter list `(..)` with zero or more parameters
  - Followed by the `=>` marker
  - Followed by a function body
    - Body only needs to be enclosed by `{..}` if there's more than one expression, or consists of non-expression
    - If there is only one expression and `{..}` is **omitted**, it implies `return` in front of the expression
- Arrow functions are always **anonymous** function expressions
- Capabilities such as default values, destructing, rest parameters etc applies
- Best Practice: The longer the function, the less `=>` helps; the shorter the function, the more `=>` shines

Consider a simple arrow function where:
- The arrow function is `(x,y) => x + y`
- Referenced by the variable `foo`

```js
function foo(x,y) {
  return x + y;
}

// Arrow function
var foo = (x,y) => x + y;

// More Arrow function variations
var f1 = () => 12;
var f2 = x => x * 2;
var f3 = (x,y) => {
  var z = x * 2 + y;
  y++;
  x *= 3;
  return (x + y + z) / 2;
};

var numbers = [1, 2, 3, 4, 5];
var timesTwo = numbers.map((item) => item * 2); // Extra parens around `(item)` is optional but recommended
console.log( timesTwo ); // [2, 4, 6, 8, 10]
```
### Not Just Shorter Syntax, But this
- `=>` arrow functions are popular for terser code by dropping `function`, `return` and `{..}`
- But `=>` were primarily designed to solve a common `this` problem
- Arrow functions use the `lexical this` whose value can be determined at compile time based on its position in the code
- Arrow-functions use lexical scope for `this` binding
- Inherits `this` binding from its enclosing function call
- Syntactic replacement of `self = this` or `.bind(..)`
- Lexical binding of arrow-functions cannot be overriden even with `new` operator

Reference: https://www.nczonline.net/blog/2013/09/10/understanding-ecmascript-6-arrow-functions/
Consider:
- `PageHandler` object that handles interactions on the page
- `init()` set up the interactions

Problem:
-  `this.doSomething(..)` points to global object instead of PageHandler

```js
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", function(event) {
            this.doSomething(event.type);     // error
        }, false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

`.bind()` solution:

```js
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", (function(event) {
            this.doSomething(event.type);
        }).bind(this), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

`that` Solution:

```js
var PageHandler = {
    that: this,

    id: "123456",

    init: function() {
        document.addEventListener("click", function(event) {
            that.doSomething(event.type);     // error
        }, false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

`arrow` Solution:

```js
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click",
                event => this.doSomething(event.type), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

## for..of Loops
- `for..of` loops over set of `iterable` values produced by an `iterator` or values which can be boxed to an iterable object
- `for..in` loops over `keys/indexes` in an array
- `for..of` loops over `values` in an array
- Default iterables in JavaScript: Arrays, Strings, Generators, Collections
- Warning: Plain objects are not by default suitable for `for..loop` looping

```js
var a = ["a","b","c","d","e"];
for(var indx in a) {
  console.log( indx ); // 0 1 2 3 4
}

for(var val of a) {
  console.log( val ); // "a" "b" "c" "d" "e"
}

// Iterating a string
for (var c of "hello") {
    console.log( c );
}
// "h" "e" "l" "l" "o"
```

## Regular Expression Extensions

ES6 introduces two new flags for regular expressions:

1. `u` enables various Unicode-related features
2. `y` enables sticky matching

### Unicode Flag
- Setting the `u` flag on a regular expression enables the use of ES6 Unicode code point escapes `(\u{…})` in the pattern
- Prior to ES6, regular expressions could only match based on Basic Multilingual Plane (BMP) characters
- Extended characters had to be represented as two separate characters

Consider: `/^.-clef/` returns `true` only if there is a single character in front of `-clef`

```js
// 1. Using regular expression
/^.-clef/ .test( "𝄞-clef" );     // false

// 2. Using unicode `u` flag
/^.-clef/u.test( "𝄞-clef" );     // true
```

### Sticky Flag
- Sticky means to search in strings only from the index indicated by the `lastIndex` property of the regular expression
- `y` implies a virtual anchor that is relative to exactly the `lastIndex` position

```js
var re2 = /foo/y,       // <-- notice the `y` sticky flag
    str = "++foo++";

re2.lastIndex;          // 0
re2.test( str );        // false -- "foo" not found at `0`
re2.lastIndex;          // 0

re2.lastIndex = 2;
re2.test( str );        // true
re2.lastIndex;          // 5 -- updated to after previous match

re2.test( str );        // false
re2.lastIndex;          // 0 -- reset after previous match failure
```

#### Sticky Versus Global
- `g` global pattern matches with `exec(..)` starting from `lastIndex`'s current value
- `g` matches are free to move ahead in their matching. Doesn't have to match exactly beginning from `lastIndex`'s current position
- `sticky` mode expression would fail because it would not be allowed to move ahead
- `g` global `match(..)` returns all the matches at once
- `sticky` `match(..)` returns one-at-a-time progressive matching. Just make sure `lastIndex` is always in the right position

#### Regular Expression flags
- The expression's flags is returned in this order: "gimuy" regardless of the original pattern

Prior to ES6:

```js
var re = /foo/ig;

re.toString();          // "/foo/ig"

var flags = re.toString().match( /\/([gim]*)$/ )[1];

flags;                  // "ig"
```

ES6 `flag` property:

```js
var re = /foo/ig;

re.flags;               // "gi"
```

## Number Literal Extensions
- Apart from decimal and hexadecimal form, ES6 adds some new forms
- An official octal form
- A brand new binary form
- An amended hexadecimal form

```js
// 1. The new ES6 numbers
var dec = 42,
    oct = 0o52,         // or `0O52`
    hex = 0x2a,         // or `0X2a`
    bin = 0b101010;     // or `0B101010`

// 2. String representation form are converted to number equivalent
Number( "42" );         // 42
Number( "0o52" );       // 42
Number( "0x2a" );       // 42
Number( "0b101010" );   // 42

// 3. Converting a number to different forms
var a = 42;

a.toString();           // "42" -- also `a.toString( 10 )`
a.toString( 8 );        // "52"
a.toString( 16 );       // "2a"
a.toString( 2 );        // "101010"
```

## Unicode
- Basic Multilingual Plane (BMP): Unicode characters that range from `0x0000` to `0xFFFF`[ie. 4 hexadecimal chars] contain all standard printed characters
  - Syntax: "\uhexadecimal"
- Extended BMP ranges up to `0x10FFFF` [i.e highest 6 hexadecimal chars] which includes `astral` symbols like 💩 U+1F4A9 poo
  - Syntax: "\u{hexadecimal}"

```js
// 1. Snowman Prior ES6
var snowman = "\u2603";
console.log( snowman );  // ☃

// 2. Surrogate pair: Two unicode-escaped chars side by side which JS interprets together as a single astral char
var surrogatePoo = '\uD83D\uDCA9';
console.log( surrogatePoo );  // 💩

// 3. ES6 expression
var poo = '\u{1F4A9}';
console.log( poo );  // 💩

// 4. JavaScript string operation still treats astral characters as two individual BMP characters
var poo1 = "💩";
console.log( poo1.length ); // 2

// 5. Temp workaround: Spread array literal to array of string symbols
console.log( [...poo1].length ); // 1

// 6. Combining Diacritical Marks: `length` trick doesn't work
var s1 = "\xE9",
    s2 = "e\u0301";

console.log( s1 );              // "é"
console.log( s2 );              // "é"

console.log( [...s1].length ); // 1
console.log( [...s2].length ); // 2

// 7. String#normalize(..) utility
s1.normalize().length;          // 1
s2.normalize().length;          // 1

s1 === s2;                      // false
s1 === s2.normalize();          // true
```

### Character Positioning
- Pre-ES6 provides charAt(..) to return a character at a given position but doesn't take into account of astral character and combining marks

```js
var s1 = "ab\u{1d49e}d";
console.log( s1 );      // "ab𝒞d"
s1.charAt( 3 );         // "" <-- unprintable surrogate

// ES6 hack
[...s1.normalize()][2]; // "𝒞"

// codePointAt(..)
s1.normalize().codePointAt( 2 ).toString( 16 ); // 1d49e

// fromCodePoint(..)
String.fromCodePoint( 0x1d49e );    // "𝒞"

// Combination
String.fromCodePoint( s1.normalize().codePointAt( 2 ) ); // "𝒞"
```

### Unicode Identifier Names
```js
// Pre-ES6
var \u03A9 = 42; // same as: var Ω = 42;
console.log(\u03A9); // 42

// ES6
var \u{2B400} = 42;
console.log(\u{2B400});
```

## Symbols
