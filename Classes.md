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

### More on `super`
- `super` works only with static class hierarchy with no cross pollination
- Borrowing `b.foo()` and use it in the context of `a` will result in an ugly surprise

```js
class ParentA {
  constructor() { this.id = "a"; }
  foo() { console.log( "ParentA: ", this.id ); }
}

class ParentB {
  constructor() { this.id = "b"; }
  foo() { console.log( "ParentB: ", this.id ); }
}

class ChildA extends ParentA {
  foo() {
    super.foo();
    console.log( "ChildA: ", this.id );
  }
}

class ChildB extends ParentB {
  foo() {
    super.foo();
    console.log( "ChildB: ", this.id );
  }
}

var a = new ChildA();
a.foo();

var b = new ChildB();
b.foo();

/*
-----------------
output
------------------
ParentA: a
ChildA: a
ParentB: b
ChildB: b
*/
```

Consider cross pollination:
- this.id reference was dynamically rebound so `: a` is reported in both cases instead of `: b`
- But `b.foo's` `super.foo()` reference wasn't dynamically rebound, so still reports as `ParentB` instead of `ParentA`
- `b.foo()` references `super` which is statically bound to the `ChildB`/`ParentB` hierarchy
```js
b.foo.call( a );
// Output
// -------------
// ParentB: a
// ChildB: a
```

### Subclass constructor
