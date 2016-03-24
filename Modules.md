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

### `import`
