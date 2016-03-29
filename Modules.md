## Modules

### The Old Way
- Outer function with inner variables and functions
- Returned as "public API" with methods that have closure over inner data

```js
// 1. Hello(..) module can produce multiple instances
function Hello(name) {
  function greeting() {
    console.log( "Hi " + name );
  }

  return {
    greeting: greeting
  }
}

var me = Hello( "Andrew" );
me.greeting(); // Hi Andrew

var stark = Hello( "Stark" );
stark.greeting(); // Hi Stark
```

`Singleton` version of above snippet:
```js
var me = (function Hello(name) {
  function greeting() {
    console.log( "Hi " + name );
  }

  return {
    greeting: greeting
  }
})( "Andrew" );
me.greeting(); // Hi Andrew
```

### Moving Forward
- ES6 uses file-based modules
- API of an ES6 module is static
- ES6 modules are singletons shared by all other modules
- Properties and methods are like pointers that are actual bindings
- Importing a module is similar to statically request it to load (if it hasn't already)

### The New Way
The two main new keywords that enable ES6 modules are `import` and `export`

### `export`
1. `export` in front of declaration
2. `export` as an operator with list of bindings to export

Note:
- Anything not labeled as `export` stays private inside the scope of the module

```js
// 1. infront of declaration
export function foo() { }
export var awesome = 22;

// 2. as an operator
var bar = [1,2,3]; // Private scoped unless you export it
export { bar };
export { foo, awesome, bar }; // named exports

// 3. Rename aka alias
//    only `bar` will be available for import hiding `foo`
function foo() { }
export { foo as bar };
```

### Updated binding
- Binding is actually a reference to or a pointer to the variable itself
- Importing `awesome` will result in 100 instead of 40

Consider:
```js
var awesome = 40;
export { awesome }

// later
awesome = 100;
```

### `default` export
- Only one `default` export per definition is allowed
- Can be exported in two ways

Consider the two snippets
```js
function foo() { }

export default foo;

// Alternate
export default function foo() { }
```
Notes:
- Exporting a binding to the function expression `foo` **value** at the moment, ***not*** the identifier foo
- Later if `foo` is assigned to a different value, the module import still reveals the function originally exported

And:
```js
function foo(){ }
export { foo as default };
```
Notes:
- `foo` binding is actually a reference to or a pointer to the variable itself
- If you later change foo's value, the value seen on the import side will also be updated

### `import`
- `"foo"` is a module specifier
- bar and baz identifiers must match named exports on the module's API

```js
// 1. Importing certain specific named members of a module's API
import { bar, baz } from "foo";
bar();

// 2. Rename the bound identifiers
import { bar as theBarFunc } from "foo";
theBarFunc();
```
