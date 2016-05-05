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

#### Endianness
- The `arr` is mapped using the endian-setting(big-endian or little-endian) of the platform where JS is running
- Endian means if the lower byte (8-bits) of a multi-byte number is on the right or the left
- Little-endian is the most common representation on web these days but doesn't guarantee in all browsers

Representing a base-10 number `3085`:
16-bits binary (Regardless of Endianness) / hexadecimal: 0000110000001101  / 0c0d
8-bits little-endian / hexadecimal: 0000110100001100 / 0d0c
8-bits big-endian / hexadecimal: 0000110000001101 / 0c0d

Test for Endianness:

```js
var littleEndian = (function() {
    var buffer = new ArrayBuffer( 2 );
    new DataView( buffer ).setInt16( 0, 256, true );
    return new Int16Array( buffer )[0] === 256;
})();
```

#### Multiple Views
A single buffer can have multiple views attached to it.

```js
var buf = new ArrayBuffer(2); // 2 bytes = 16 bits
var view8 = new Uint8Array(buf); // Array with 2 buckets
var view16 = new Uint16Array(buf); // Array with 1 bucket

view16[0] = 3085;
view8[0]; // 13
view8[1]; // 12
```


### Maps
### WeakMaps
### Sets
### WeakSets
