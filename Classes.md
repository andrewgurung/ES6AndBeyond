## Classes
- `class` keyword identifies a block where the contents define the members of a function's prototype

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
```

Notes:
- `class` Foo implies creating a special function of the name `foo`
- `constructor(..)` identifies the signature of that `Foo(..)` function
- Unlike object literals, there are no commas to separate members in a `class` body
