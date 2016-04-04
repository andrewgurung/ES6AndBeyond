## Classes
- `class` keyword identifies a block where the contents define the members of a function's prototype
- An ES6 `class` is just a meta concept that wraps around other concrete entities such as functions and properties

```js
// 1. Class Declaration
class Foo {
  constructor(a,b) {
    this.x = a;
    this.y = b;
  }

  gimmeXY() {
    return this.x * this.y;
  }
}

// 2. Class expression
var x = class Y { };
```

Notes:
- `class` Foo implies creating a special function of the name `foo`
- `constructor(..)` identifies the signature of that `Foo(..)` function
- Unlike object literals, there are no commas to separate members in a `class` body
- `class` expression is useful for passing a class definition as function argument

### `extends` and `super`
- `extends` is a syntactic sugar for creating [[Prototype]] delegation link between two function prototypes -- commonly mislabeled as "inheritance"
- `super` automatically refers to:
  - In the constructor --> the "parent constructor"
  - In the method --> the "parent object"
- In the following code snippet, `Bar extends Foo` is linking the [[Prototype]] of `Bar.prototype` to `Foo.prototype`

```js
class Foo {
  constructor(a,b) {
    this.x = a;
    this.y = b;
  }

  gimmeXY() {
    return this.x * this.y;
  }
}

class Bar extends Foo {
  constructor(a, b, c) {
    super(a,b);
    this.z = c;
  }

  gimmeXYZ() {
    return super.gimmeXY() * this.z;
  }
}

var b = new Bar( 5, 15, 25 );
b.gimmeXYZ(); // 1875
```
