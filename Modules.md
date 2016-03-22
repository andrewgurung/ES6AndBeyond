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
