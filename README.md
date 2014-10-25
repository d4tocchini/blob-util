blob-util
=====

[![Build Status](https://travis-ci.org/nolanlawson/blob-util.svg)](https://travis-ci.org/nolanlawson/blob-util)

`blob-util` is a [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob?redirectlocale=en-US&redirectslug=DOM%2FBlob) library for busy people.

If you want an easy way to work with binary data in the browser, or you don't even know what a Blob is, then this is the library for you.

`blob-util` offers cross-browser utilities for translating Blobs to and from different formats:

* `<img/>` tags
* base 64 strings
* binary strings
* ArrayBuffers
* data URLs

It's also a good pairing with the attachment API in [PouchDB](http://pouchdb.com).

`blob-util` is a browser library. If you need to work with binary data in Node.js, see [Buffers](http://nodejs.org/api/buffer.html).

Install
------

Grab it from the `dist/` folder above, or use NPM:

```
npm install blob-util
```

Or Bower:

```
bower install blob-util
```

Then stick it in your HTML:

```html
<script src="blob-util.js"></script>
```

Now you have a `window.blobUtil` object that contains the API. Or if you don't like
globals, you can use Browserify.


Quick Start
--------

Here's Kirby. He's a famous little Blob.

<img id="kirby" alt="Kirby" src="./test/kirby.gif"/>

So let's fulfill his manifest destiny, and convert him to a real `Blob` object.

```js
var img = document.getElementById('kirby');

blobUtil.imgSrcToBlob(img.src).then(function (blob) {
  // ladies and gents, we have a blob
}).catch(function (err) {
  // image failed to load
})
```

(Don't worry, this won't download the image twice, because browsers are smart.)

Now that we have a `Blob`, we can convert it to a URL and use that as the source for another `<img/>` tag:

```js
var blobURL = blobUtil.createObjectURL(blob);

var newImg = document.createElement('img');

newImg.src = blobURL;
```

So now we have to Kirbys - one with a normal URL, and the other with a blob URL. Neato!

API
-------

Warning: this API uses [Promises](https://promisesaplus.com/), because it's not 2009 anymore.

###Overview

* [createBlob(parts, options)](#createBlob)
* [createObjectURL(blob)](#createObjectURL)
* [revokeObjectURL(url)](#revokeObjectURL)
* [blobToBinaryString(blob)](#blobToBinaryString)
* [base64StringToBlob(base64, type)](#base64StringToBlob)
* [binaryStringToBlob(binary, type)](#binaryStringToBlob)
* [blobToBase64String(blob)](#blobToBase64String)
* [dataURLToBlob(dataURL)](#dataURLToBlob)
* [imgSrcToDataURL(src, type)](#imgSrcToDataURL)
* [canvasToBlob(canvas, type)](#canvasToBlob)
* [imgSrcToBlob(src, type)](#imgSrcToBlob)
* [arrayBufferToBlob(buffer, type)](#arrayBufferToBlob)
* [blobToArrayBuffer(blob)](#blobToArrayBuffer)
 
<a name="createBlob"></a>
###createBlob(parts, options)
Shim for
[new Blob()](https://developer.mozilla.org/en-US/docs/Web/API/Blob.Blob)
to support
[older browsers that use the deprecated <code>BlobBuilder</code> API](http://caniuse.com/blob).

**Params**

- parts `Array` - content of the <code>Blob</code>  
- options `Object` - usually just <code>{type: myContentType}</code>  

**Returns**: `Blob`  
<a name="createObjectURL"></a>
###createObjectURL(blob)
Shim for
[URL.createObjectURL()](https://developer.mozilla.org/en-US/docs/Web/API/URL.createObjectURL)
to support browsers that only have the prefixed
<code>webkitURL</code> (e.g. Android <4.4).

**Params**

- blob `Blob`  

**Returns**: `string` - url  
<a name="revokeObjectURL"></a>
###revokeObjectURL(url)
Shim for
[URL.revokeObjectURL()](https://developer.mozilla.org/en-US/docs/Web/API/URL.revokeObjectURL)
to support browsers that only have the prefixed
<code>webkitURL</code> (e.g. Android <4.4).

**Params**

- url `string`  

<a name="blobToBinaryString"></a>
###blobToBinaryString(blob)
Convert a <code>Blob</code> to a binary string. Returns a Promise.

**Params**

- blob `Blob`  

**Returns**: `Promise` - Promise that resolves with the binary string  
<a name="base64StringToBlob"></a>
###base64StringToBlob(base64, type)
Convert a base64-encoded string to a <code>Blob</code>. Returns a Promise.

**Params**

- base64 `string`  
- type `string` | `undefined` - the content type (optional)  

**Returns**: `Promise` - Promise that resolves with the <code>Blob</code>  
<a name="binaryStringToBlob"></a>
###binaryStringToBlob(binary, type)
Convert a binary string to a <code>Blob</code>. Returns a Promise.

**Params**

- binary `string`  
- type `string` | `undefined` - the content type (optional)  

**Returns**: `Promise` - Promise that resolves with the <code>Blob</code>  
<a name="blobToBase64String"></a>
###blobToBase64String(blob)
Convert a <code>Blob</code> to a binary string. Returns a Promise.

**Params**

- blob `Blob`  

**Returns**: `Promise` - Promise that resolves with the binary string  
<a name="dataURLToBlob"></a>
###dataURLToBlob(dataURL)
Convert a data URL string
(e.g. <code>'data:image/png;base64,iVBORw0KG...'</code>)
to a <code>Blob</code>. Returns a Promise.

**Params**

- dataURL `string`  

**Returns**: `Promise` - Promise that resolves with the <code>Blob</code>  
<a name="imgSrcToDataURL"></a>
###imgSrcToDataURL(src, type)
Convert an image's <code>src</code> URL to a data URL by loading the image and painting
it to a <code>canvas</code>. Returns a Promise.

<p/>Note: this will coerce the image to the desired content type, and it
will only paint the first frame of an animated GIF.

**Params**

- src `string`  
- type `string` | `undefined` - the content type (optional, defaults to 'image/png')  

**Returns**: `Promise` - Promise that resolves with the data URL string  
<a name="canvasToBlob"></a>
###canvasToBlob(canvas, type)
Convert a <code>canvas</code> to a <code>Blob</code>. Returns a Promise.

**Params**

- canvas `string`  
- type `string` | `undefined` - the content type (optional, defaults to 'image/png')  

**Returns**: `Promise` - Promise that resolves with the <code>Blob</code>  
<a name="imgSrcToBlob"></a>
###imgSrcToBlob(src, type)
Convert an image's <code>src</code> URL to a <code>Blob</code> by loading the image and painting
it to a <code>canvas</code>. Returns a Promise.

<p/>Note: this will coerce the image to the desired content type, and it
will only paint the first frame of an animated GIF.

**Params**

- src `string`  
- type `string` | `undefined` - the content type (optional, defaults to 'image/png')  

**Returns**: `Promise` - Promise that resolves with the <code>Blob</code>  
<a name="arrayBufferToBlob"></a>
###arrayBufferToBlob(buffer, type)
Convert an <code>ArrayBuffer</code> to a <code>Blob</code>. Returns a Promise.

**Params**

- buffer `ArrayBuffer`  
- type `string` | `undefined` - the content type (optional)  

**Returns**: `Promise` - Promise that resolves with the <code>Blob</code>  
<a name="blobToArrayBuffer"></a>
###blobToArrayBuffer(blob)
Convert a <code>Blob</code> to an <code>ArrayBuffer</code>. Returns a Promise.

**Params**

- blob `Blob`  

**Returns**: `Promise` - Promise that resolves with the <code>ArrayBuffer</code>  





Building the library
----
    npm install
    npm run build

Your plugin is now located at `dist/pouchdb.mypluginname.js` and `dist/pouchdb.mypluginname.min.js` and is ready for distribution.

To generate documentation:

    npm run jsdoc


Testing the library
----

### In the browser

Run `npm run dev` and then point your favorite browser to [http://127.0.0.1:8001/test/index.html](http://127.0.0.1:8001/test/index.html).

The query param `?grep=mysearch` will search for tests matching `mysearch`.

### Automated browser tests

You can run e.g.

    CLIENT=selenium:firefox npm test
    CLIENT=selenium:phantomjs npm test

This will run the tests automatically and the process will exit with a 0 or a 1 when it's done. Firefox uses IndexedDB, and PhantomJS uses WebSQL.