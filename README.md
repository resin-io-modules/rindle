rindle
======

[![npm version](https://badge.fury.io/js/rindle.svg)](http://badge.fury.io/js/rindle)
[![dependencies](https://david-dm.org/jviotti/rindle.png)](https://david-dm.org/jviotti/rindle.png)
[![Build Status](https://travis-ci.org/jviotti/rindle.svg?branch=master)](https://travis-ci.org/jviotti/rindle)
[![Build status](https://ci.appveyor.com/api/projects/status/cjyj0u68pq3x7031?svg=true)](https://ci.appveyor.com/project/resin-io/rindle)

Collection of utilities for working with Streams.

Description
-----------

This is a collection of functions that operate on streams to encapsulate some of the tasks I usually have to do in my project. I'll be adding more as I encounter more patterns.

Installation
------------

Install `rindle` by running:

```sh
$ npm install --save rindle
```

Documentation
-------------


* [rindle](#module_rindle)
  * [.wait(stream, callback)](#module_rindle.wait)
  * [.extract(stream, callback)](#module_rindle.extract)
  * [.bifurcate(stream, output1, output2, callback)](#module_rindle.bifurcate)

<a name="module_rindle.wait"></a>
### rindle.wait(stream, callback)
This functions listens for the following events:

- `close`.
- `end`.
- `done`.

If those events pass any argument when being emitted, you'll be able to access them as arguments to the callback.

**Kind**: static method of <code>[rindle](#module_rindle)</code>  
**Summary**: Wait for a stream to close  
**Access:** public  

| Param | Type | Description |
| --- | --- | --- |
| stream | <code>Stream</code> | stream |
| callback | <code>function</code> | callback (error, args...) |

**Example**  
```js
var fs = require('fs');
var rindle = require('rindle');

var input = fs.createReadStream('foo/bar');
var output = fs.createWriteStream('foo/baz');

input.pipe(output);

rindle.wait(output, function(error) {
  if (error) throw error;
  console.log('The output stream was closed!');
});
```
<a name="module_rindle.extract"></a>
### rindle.extract(stream, callback)
Notice this function only extracts the *remaining data* from the stream.

**Kind**: static method of <code>[rindle](#module_rindle)</code>  
**Summary**: Extract data from readable stream  
**Access:** public  

| Param | Type | Description |
| --- | --- | --- |
| stream | <code>StreamReadable</code> | stream |
| callback | <code>function</code> | callback (error, data) |

**Example**  
```js
var fs = require('fs');
var rindle = require('rindle');

var input = fs.createReadStream('foo/bar');

rindle.extract(input, function(error, data) {
  if (error) throw error;
  console.log('The file contains: ' + data);
});
```
<a name="module_rindle.bifurcate"></a>
### rindle.bifurcate(stream, output1, output2, callback)
The callback is called when both output stream close.

**Kind**: static method of <code>[rindle](#module_rindle)</code>  
**Summary**: Bifurcate readable stream to two writable streams  
**Access:** public  

| Param | Type | Description |
| --- | --- | --- |
| stream | <code>StreamReadable</code> | input stream |
| output1 | <code>StreamWritable</code> | first output stream |
| output2 | <code>StreamWritable</code> | second output stream |
| callback | <code>function</code> | callback (error) |

**Example**  
```js
var fs = require('fs');
var rindle = require('rindle');

var input = fs.createReadStream('foo/bar');
var output1 = fs.createWriteStream('foo/baz');
var output2 = fs.createWriteStream('foo/qux');

rindle.bifurcate(input, output1, output2, function(error) {
  if (error) throw error;

  console.log('All files written!');
});
```

Support
-------

If you're having any problem, please [raise an issue](https://github.com/jviotti/rindle/issues/new) on GitHub and I'll be happy to help.

Tests
-----

Run the test suite by doing:

```sh
$ gulp test
```

Contribute
----------

- Issue Tracker: [github.com/jviotti/rindle/issues](https://github.com/jviotti/rindle/issues)
- Source Code: [github.com/jviotti/rindle](https://github.com/jviotti/rindle)

Before submitting a PR, please make sure that you include tests, and that [jshint](http://jshint.com) runs without any warning:

```sh
$ gulp lint
```

License
-------

The project is licensed under the MIT license.
