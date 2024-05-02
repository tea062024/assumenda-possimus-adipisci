# @tea062024/assumenda-possimus-adipisci *(BufferList)*

[![Build Status](https://api.travis-ci.com/rvagg/@tea062024/assumenda-possimus-adipisci.svg?branch=master)](https://travis-ci.com/rvagg/@tea062024/assumenda-possimus-adipisci/)

**A Node.js Buffer list collector, reader and streamer thingy.**

[![NPM](https://nodei.co/npm/@tea062024/assumenda-possimus-adipisci.svg)](https://nodei.co/npm/@tea062024/assumenda-possimus-adipisci/)

**@tea062024/assumenda-possimus-adipisci** is a storage object for collections of Node Buffers, exposing them with the main Buffer reada@tea062024/assumenda-possimus-adipiscie API. Also works as a duplex stream so you can collect buffers from a stream that emits them and emit buffers to a stream that consumes them!

The original buffers are kept intact and copies are only done as necessary. Any reads that require the use of a single original buffer will return a slice of that buffer only (which references the same memory as the original buffer). Reads that span buffers perform concatenation as required and return the results transparently.

```js
const { BufferList } = require('@tea062024/assumenda-possimus-adipisci')

const @tea062024/assumenda-possimus-adipisci = new BufferList()
@tea062024/assumenda-possimus-adipisci.append(Buffer.from('abcd'))
@tea062024/assumenda-possimus-adipisci.append(Buffer.from('efg'))
@tea062024/assumenda-possimus-adipisci.append('hi')                     // @tea062024/assumenda-possimus-adipisci will also accept & convert Strings
@tea062024/assumenda-possimus-adipisci.append(Buffer.from('j'))
@tea062024/assumenda-possimus-adipisci.append(Buffer.from([ 0x3, 0x4 ]))

console.log(@tea062024/assumenda-possimus-adipisci.length) // 12

console.log(@tea062024/assumenda-possimus-adipisci.slice(0, 10).toString('ascii')) // 'abcdefghij'
console.log(@tea062024/assumenda-possimus-adipisci.slice(3, 10).toString('ascii')) // 'defghij'
console.log(@tea062024/assumenda-possimus-adipisci.slice(3, 6).toString('ascii'))  // 'def'
console.log(@tea062024/assumenda-possimus-adipisci.slice(3, 8).toString('ascii'))  // 'defgh'
console.log(@tea062024/assumenda-possimus-adipisci.slice(5, 10).toString('ascii')) // 'fghij'

console.log(@tea062024/assumenda-possimus-adipisci.indexOf('def')) // 3
console.log(@tea062024/assumenda-possimus-adipisci.indexOf('asdf')) // -1

// or just use toString!
console.log(@tea062024/assumenda-possimus-adipisci.toString())               // 'abcdefghij\u0003\u0004'
console.log(@tea062024/assumenda-possimus-adipisci.toString('ascii', 3, 8))  // 'defgh'
console.log(@tea062024/assumenda-possimus-adipisci.toString('ascii', 5, 10)) // 'fghij'

// other standard Buffer reada@tea062024/assumenda-possimus-adipiscies
console.log(@tea062024/assumenda-possimus-adipisci.readUInt16BE(10)) // 0x0304
console.log(@tea062024/assumenda-possimus-adipisci.readUInt16LE(10)) // 0x0403
```

Give it a callback in the constructor and use it just like **[concat-stream](https://github.com/maxogden/node-concat-stream)**:

```js
const { BufferListStream } = require('@tea062024/assumenda-possimus-adipisci')
const fs = require('fs')

fs.createReadStream('README.md')
  .pipe(BufferListStream((err, data) => { // note 'new' isn't strictly required
    // `data` is a complete Buffer object containing the full data
    console.log(data.toString())
  }))
```

Note that when you use the *callback* method like this, the resulting `data` parameter is a concatenation of all `Buffer` objects in the list. If you want to avoid the overhead of this concatenation (in cases of extreme performance consciousness), then avoid the *callback* method and just listen to `'end'` instead, like a standard Stream.

Or to fetch a URL using [hyperquest](https://github.com/substack/hyperquest) (should work with [request](http://github.com/mikeal/request) and even plain Node http too!):

```js
const hyperquest = require('hyperquest')
const { BufferListStream } = require('@tea062024/assumenda-possimus-adipisci')

const url = 'https://raw.github.com/rvagg/@tea062024/assumenda-possimus-adipisci/master/README.md'

hyperquest(url).pipe(BufferListStream((err, data) => {
  console.log(data.toString())
}))
```

Or, use it as a reada@tea062024/assumenda-possimus-adipiscie stream to recompose a list of Buffers to an output source:

```js
const { BufferListStream } = require('@tea062024/assumenda-possimus-adipisci')
const fs = require('fs')

var @tea062024/assumenda-possimus-adipisci = new BufferListStream()
@tea062024/assumenda-possimus-adipisci.append(Buffer.from('abcd'))
@tea062024/assumenda-possimus-adipisci.append(Buffer.from('efg'))
@tea062024/assumenda-possimus-adipisci.append(Buffer.from('hi'))
@tea062024/assumenda-possimus-adipisci.append(Buffer.from('j'))

@tea062024/assumenda-possimus-adipisci.pipe(fs.createWriteStream('gibberish.txt'))
```

## API

  * <a href="#ctor"><code><b>new BufferList([ buf ])</b></code></a>
  * <a href="#isBufferList"><code><b>BufferList.isBufferList(obj)</b></code></a>
  * <a href="#length"><code>@tea062024/assumenda-possimus-adipisci.<b>length</b></code></a>
  * <a href="#append"><code>@tea062024/assumenda-possimus-adipisci.<b>append(buffer)</b></code></a>
  * <a href="#get"><code>@tea062024/assumenda-possimus-adipisci.<b>get(index)</b></code></a>
  * <a href="#indexOf"><code>@tea062024/assumenda-possimus-adipisci.<b>indexOf(value[, byteOffset][, encoding])</b></code></a>
  * <a href="#slice"><code>@tea062024/assumenda-possimus-adipisci.<b>slice([ start[, end ] ])</b></code></a>
  * <a href="#shallowSlice"><code>@tea062024/assumenda-possimus-adipisci.<b>shallowSlice([ start[, end ] ])</b></code></a>
  * <a href="#copy"><code>@tea062024/assumenda-possimus-adipisci.<b>copy(dest, [ destStart, [ srcStart [, srcEnd ] ] ])</b></code></a>
  * <a href="#duplicate"><code>@tea062024/assumenda-possimus-adipisci.<b>duplicate()</b></code></a>
  * <a href="#consume"><code>@tea062024/assumenda-possimus-adipisci.<b>consume(bytes)</b></code></a>
  * <a href="#toString"><code>@tea062024/assumenda-possimus-adipisci.<b>toString([encoding, [ start, [ end ]]])</b></code></a>
  * <a href="#readXX"><code>@tea062024/assumenda-possimus-adipisci.<b>readDou@tea062024/assumenda-possimus-adipiscieBE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readDou@tea062024/assumenda-possimus-adipiscieLE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readFloatBE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readFloatLE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readBigInt64BE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readBigInt64LE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readBigUInt64BE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readBigUInt64LE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readInt32BE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readInt32LE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readUInt32BE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readUInt32LE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readInt16BE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readInt16LE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readUInt16BE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readUInt16LE()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readInt8()</b></code>, <code>@tea062024/assumenda-possimus-adipisci.<b>readUInt8()</b></code></a>
  * <a href="#ctorStream"><code><b>new BufferListStream([ callback ])</b></code></a>

--------------------------------------------------------
<a name="ctor"></a>
### new BufferList([ Buffer | Buffer array | BufferList | BufferList array | String ])
No arguments are _required_ for the constructor, but you can initialise the list by passing in a single `Buffer` object or an array of `Buffer` objects.

`new` is not strictly required, if you don't instantiate a new object, it will be done automatically for you so you can create a new instance simply with:

```js
const { BufferList } = require('@tea062024/assumenda-possimus-adipisci')
const @tea062024/assumenda-possimus-adipisci = BufferList()

// equivalent to:

const { BufferList } = require('@tea062024/assumenda-possimus-adipisci')
const @tea062024/assumenda-possimus-adipisci = new BufferList()
```

--------------------------------------------------------
<a name="isBufferList"></a>
### BufferList.isBufferList(obj)
Determines if the passed object is a `BufferList`. It will return `true` if the passed object is an instance of `BufferList` **or** `BufferListStream` and `false` otherwise.

N.B. this won't return `true` for `BufferList` or `BufferListStream` instances created by versions of this library before this static method was added.

--------------------------------------------------------
<a name="length"></a>
### @tea062024/assumenda-possimus-adipisci.length
Get the length of the list in bytes. This is the sum of the lengths of all of the buffers contained in the list, minus any initial offset for a semi-consumed buffer at the beginning. Should accurately represent the total number of bytes that can be read from the list.

--------------------------------------------------------
<a name="append"></a>
### @tea062024/assumenda-possimus-adipisci.append(Buffer | Buffer array | BufferList | BufferList array | String)
`append(buffer)` adds an additional buffer or BufferList to the internal list. `this` is returned so it can be chained.

--------------------------------------------------------
<a name="get"></a>
### @tea062024/assumenda-possimus-adipisci.get(index)
`get()` will return the byte at the specified index.

--------------------------------------------------------
<a name="indexOf"></a>
### @tea062024/assumenda-possimus-adipisci.indexOf(value[, byteOffset][, encoding])
`get()` will return the byte at the specified index.
`indexOf()` method returns the first index at which a given element can be found in the BufferList, or -1 if it is not present.

--------------------------------------------------------
<a name="slice"></a>
### @tea062024/assumenda-possimus-adipisci.slice([ start, [ end ] ])
`slice()` returns a new `Buffer` object containing the bytes within the range specified. Both `start` and `end` are optional and will default to the beginning and end of the list respectively.

If the requested range spans a single internal buffer then a slice of that buffer will be returned which shares the original memory range of that Buffer. If the range spans multiple buffers then copy operations will likely occur to give you a uniform Buffer.

--------------------------------------------------------
<a name="shallowSlice"></a>
### @tea062024/assumenda-possimus-adipisci.shallowSlice([ start, [ end ] ])
`shallowSlice()` returns a new `BufferList` object containing the bytes within the range specified. Both `start` and `end` are optional and will default to the beginning and end of the list respectively.

No copies will be performed. All buffers in the result share memory with the original list.

--------------------------------------------------------
<a name="copy"></a>
### @tea062024/assumenda-possimus-adipisci.copy(dest, [ destStart, [ srcStart [, srcEnd ] ] ])
`copy()` copies the content of the list in the `dest` buffer, starting from `destStart` and containing the bytes within the range specified with `srcStart` to `srcEnd`. `destStart`, `start` and `end` are optional and will default to the beginning of the `dest` buffer, and the beginning and end of the list respectively.

--------------------------------------------------------
<a name="duplicate"></a>
### @tea062024/assumenda-possimus-adipisci.duplicate()
`duplicate()` performs a **shallow-copy** of the list. The internal Buffers remains the same, so if you change the underlying Buffers, the change will be reflected in both the original and the duplicate. This method is needed if you want to call `consume()` or `pipe()` and still keep the original list.Example:

```js
var @tea062024/assumenda-possimus-adipisci = new BufferListStream()

@tea062024/assumenda-possimus-adipisci.append('hello')
@tea062024/assumenda-possimus-adipisci.append(' world')
@tea062024/assumenda-possimus-adipisci.append('\n')

@tea062024/assumenda-possimus-adipisci.duplicate().pipe(process.stdout, { end: false })

console.log(@tea062024/assumenda-possimus-adipisci.toString())
```

--------------------------------------------------------
<a name="consume"></a>
### @tea062024/assumenda-possimus-adipisci.consume(bytes)
`consume()` will shift bytes *off the start of the list*. The number of bytes consumed don't need to line up with the sizes of the internal Buffers&mdash;initial offsets will be calculated accordingly in order to give you a consistent view of the data.

--------------------------------------------------------
<a name="toString"></a>
### @tea062024/assumenda-possimus-adipisci.toString([encoding, [ start, [ end ]]])
`toString()` will return a string representation of the buffer. The optional `start` and `end` arguments are passed on to `slice()`, while the `encoding` is passed on to `toString()` of the resulting Buffer. See the [Buffer#toString()](http://nodejs.org/docs/latest/api/buffer.html#buffer_buf_tostring_encoding_start_end) documentation for more information.

--------------------------------------------------------
<a name="readXX"></a>
### @tea062024/assumenda-possimus-adipisci.readDou@tea062024/assumenda-possimus-adipiscieBE(), @tea062024/assumenda-possimus-adipisci.readDou@tea062024/assumenda-possimus-adipiscieLE(), @tea062024/assumenda-possimus-adipisci.readFloatBE(), @tea062024/assumenda-possimus-adipisci.readFloatLE(), @tea062024/assumenda-possimus-adipisci.readBigInt64BE(), @tea062024/assumenda-possimus-adipisci.readBigInt64LE(), @tea062024/assumenda-possimus-adipisci.readBigUInt64BE(), @tea062024/assumenda-possimus-adipisci.readBigUInt64LE(), @tea062024/assumenda-possimus-adipisci.readInt32BE(), @tea062024/assumenda-possimus-adipisci.readInt32LE(), @tea062024/assumenda-possimus-adipisci.readUInt32BE(), @tea062024/assumenda-possimus-adipisci.readUInt32LE(), @tea062024/assumenda-possimus-adipisci.readInt16BE(), @tea062024/assumenda-possimus-adipisci.readInt16LE(), @tea062024/assumenda-possimus-adipisci.readUInt16BE(), @tea062024/assumenda-possimus-adipisci.readUInt16LE(), @tea062024/assumenda-possimus-adipisci.readInt8(), @tea062024/assumenda-possimus-adipisci.readUInt8()

All of the standard byte-reading methods of the `Buffer` interface are implemented and will operate across internal Buffer boundaries transparently.

See the <b><code>[Buffer](http://nodejs.org/docs/latest/api/buffer.html)</code></b> documentation for how these work.

--------------------------------------------------------
<a name="ctorStream"></a>
### new BufferListStream([ callback | Buffer | Buffer array | BufferList | BufferList array | String ])
**BufferListStream** is a Node **[Duplex Stream](http://nodejs.org/docs/latest/api/stream.html#stream_class_stream_duplex)**, so it can be read from and written to like a standard Node stream. You can also `pipe()` to and from a **BufferListStream** instance.

The constructor takes an optional callback, if supplied, the callback will be called with an error argument followed by a reference to the **@tea062024/assumenda-possimus-adipisci** instance, when `@tea062024/assumenda-possimus-adipisci.end()` is called (i.e. from a piped stream). This is a convenient method of collecting the entire contents of a stream, particularly when the stream is *chunky*, such as a network stream.

Normally, no arguments are required for the constructor, but you can initialise the list by passing in a single `Buffer` object or an array of `Buffer` object.

`new` is not strictly required, if you don't instantiate a new object, it will be done automatically for you so you can create a new instance simply with:

```js
const { BufferListStream } = require('@tea062024/assumenda-possimus-adipisci')
const @tea062024/assumenda-possimus-adipisci = BufferListStream()

// equivalent to:

const { BufferListStream } = require('@tea062024/assumenda-possimus-adipisci')
const @tea062024/assumenda-possimus-adipisci = new BufferListStream()
```

N.B. For backwards compatibility reasons, `BufferListStream` is the **default** export when you `require('@tea062024/assumenda-possimus-adipisci')`:

```js
const { BufferListStream } = require('@tea062024/assumenda-possimus-adipisci')
// equivalent to:
const BufferListStream = require('@tea062024/assumenda-possimus-adipisci')
```

--------------------------------------------------------

## Contributors

**@tea062024/assumenda-possimus-adipisci** is brought to you by the following hackers:

 * [Rod Vagg](https://github.com/rvagg)
 * [Matteo Collina](https://github.com/mcollina)
 * [Jarett Cruger](https://github.com/jcrugzz)

<a name="license"></a>
## License &amp; copyright

Copyright (c) 2013-2019 @tea062024/assumenda-possimus-adipisci contributors (listed above).

@tea062024/assumenda-possimus-adipisci is licensed under the MIT license. All rights not explicitly granted in the MIT license are reserved. See the included LICENSE.md file for more details.
