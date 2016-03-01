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

## Spread / Rest
## Default Parameter Values
## Destructuring
## Object Literal Extensions
## Template Literals
## Arrow Functions
## for..of Loops
## Regular Expression Extensions
## Number Literal Extensions
## Unicode
## Symbols
