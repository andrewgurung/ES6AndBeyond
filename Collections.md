## Collections
- Array and object were the primary mechanism for creating data structures from the beginning
- ES6 introduces some new data structure abstractions as a native component

New ES6 collections listed below:

### TypedArrays
- Typed Arrays doesn't mean an array of specific type of values like `number` and `string`
- Instead `typed arrays` are collections that provide structured access to binary data
- `Type` acts as a bucket of bits which maps bits into an array of buckets namely 8-bit, 16-bit signed integers and so on
- Such bit-buckets are called `buffer`

```js
// 1. 32-bytes (256 bits) long buffer
var buf = new ArrayBuffer(32);
console.log(buf.byteLength);  // 32 [32 bytes]

var arr16 = new Uint16Array(buf);
console.log(arr16.length);  // 16  [256/16 bucket]

var arr8 = new Uint8Array(buf);
console.log(arr8.length);  // 32  [256/8 bucket]
```

### Maps
### WeakMaps
### Sets
### WeakSets
