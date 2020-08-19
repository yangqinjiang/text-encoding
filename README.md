# text-encoding-gbk for gb18030

origin source : https://github.com/inexorabletash/text-encoding , and thank he ！

```html
yangqinjiang@qq.com
```
==============

This is a polyfill for the [Encoding Living
Standard](https://encoding.spec.whatwg.org/) API for the Web, allowing
encoding and decoding of textual data to and from Typed Array buffers
for binary data in JavaScript.

By default it adheres to the spec and does not support *encoding* to
legacy encodings, only *decoding*. It is also implemented to match the
specification's algorithms, rather than for performance. The intended
use is within Web pages, so it has no dependency on server frameworks
or particular module schemes.

Basic examples and tests are included.

### Install ###

There are a few ways you can get and use the `text-encoding` library.

### HTML Page Usage ###

Clone the repo and include the files directly:

```html
  <!-- Required for non-UTF encodings -->
  <script src="encoding-indexes.js"></script>
  <script src="encoding.js"></script>
```

This is the only use case the developer cares about. If you want those
fancy module and/or package manager things that are popular these days
you should probably use a different library.

#### Package Managers ####

The package is published to **npm** and **bower** as `text-encoding`.
Use through these is not really supported, since they aren't used by
the developer of the library. Using `require()` in interesting ways
probably breaks. Patches welcome, as long as they don't break the
basic use of the files via `<script>`.

### API Overview ###

Basic Usage

```js
  var uint8array = new TextEncoder().encode(string);
  var string = new TextDecoder(encoding).decode(uint8array);
```

Streaming Decode

```js
  var string = "", decoder = new TextDecoder(encoding), buffer;
  while (buffer = next_chunk()) {
    string += decoder.decode(buffer, {stream:true});
  }
  string += decoder.decode(); // finish the stream
```

### Encodings ###

All encodings from the Encoding specification are supported:

only **gb18030**

(Some encodings may be supported under other names, e.g. ascii,
iso-8859-1, etc. See [Encoding](https://encoding.spec.whatwg.org/) for
additional labels for each encoding.)

Encodings other than **utf-8**, **utf-16le** and **utf-16be** require
an additional `encoding-indexes.js` file to be included. It is rather
large (596kB uncompressed, 188kB gzipped); portions may be deleted if
support for some encodings is not required.

### Non-Standard Behavior ###

As required by the specification, only encoding to **utf-8** is
supported. If you want to try it out, you can force a non-standard
behavior by passing the `NONSTANDARD_allowLegacyEncoding` option to
TextEncoder and a label. For example:

```js
var uint8array = new TextEncoder(
  'gb18030', { NONSTANDARD_allowLegacyEncoding: true }).encode(text);
```

But note that the above won't work if you're using the polyfill in a
browser that natively supports the TextEncoder API natively, since the
polyfill won't be used!

You can force the polyfill to be used by using this before the polyfill:

```html
<script>
window.TextEncoder = window.TextDecoder = null;
</script>
```

To support the legacy encodings (which may be stateful), the
TextEncoder `encode()` method accepts an optional dictionary and
`stream` option, e.g. `encoder.encode(string, {stream: true});` This
is not needed for standard encoding since the input is always in
complete code points.
