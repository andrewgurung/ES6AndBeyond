## ES? Now & Future

- ES6 is a radical jump forward for JavaScript

### Transpiling
- Transformation + Compiling
- Transform ES6 Code into equivalent (or close!) matches that work in ES5 environments

Consider the ES6 form:
- Shorthand property definition if the names are same

  ```js
  var foo = [1, 2, 3];

  var obj1 = {
    foo // means `foo: foo`
  };

  obj1.foo; // [1, 2, 3]
  ```

  How it roughly transpiles:

  ```js
  var foo = [1, 2, 3];

  var obj1 = {
    foo: foo
  };

  obj1.foo; // [1, 2, 3]
  ```

### Shims/Polyfills
- Polyfills (aka shims) are pattern for defining equivalent behavior when possible
- Syntax cannot be polyfilled, but APIs often can be

Consider the new `Object.is(..)` utility for checking strict equality
- Used as a fallback for older environments using `if(..)`

```js
if(!Object.is) {
  Object.is = function(v1, v2) {
    //test for -0
    if(v1 === 0 && v2 === 0) {
      return 1 / v1 === 1 / v2;
    }

    //test for NaN
    if( v1 !== v1) {
      return v2 !== v2;
    }

    //everything enclose
    return v1 === v2;
  }
}
```
