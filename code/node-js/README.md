# Node.js

- background info
  - V8
  - servers for idiots
  - why node
- globals
- libraries
- modules
- events
- async
- files & fs
  - streams
  - pipes


----
## background info

### V8

V8 is actually written in c++ - it takes javascript and outputs machine code.

node.js is also written in c++. it is a "runtime environment"\* that uses v8 to process javascript and also adds some features. so the extra javascript that's possible with node is possible because it converts it to c++ code.

\* a runtime environment is the system that executes code live, with the help of a compiler (like v8). an operating system is a kind of runtime environment.

----

### servers for idiots

a server is a computer connected to the internet. it has an operating system installed but its only job is to run the server program that sits on it. these programs **listen for requests and return a response** and use a protocol called HTTP.

when you type a website into a browser, the browser does a DNS lookup to workout the IP of the server computer. the computer is on all day and the operating system lets the server program know when a HTTP request arrives at its IP. the server program will do something in response to the request, for example it might send back a HTML file, or a piece of JSON, or it may interpret some PHP/ruby/python to create a HTML file. this is sent back to the requesting IP address.

node allows us to use javascript to both manipulate the DOM in the browser, and manage HTTP requests on the server.

----

### why node?

along with allowing us to use the same language across the **full stack**, node adds libraries to js (written in c++) that deal with:

- file I/O
- databases
- networking
- HTTP

since javascript is single threaded, this makes it unsuitable for dealing with multiple tasks at the same time, as servers must do. node takes an **asynchronous** approach to make this possible

----

## globals

- Buffer
  work with memory allocations (ES6 TypedArray does similar stuff, but [use Buffer with node](https://nodejs.org/api/buffer.html#buffer_buffers_and_typedarray))
- `__dirname`
  the dir of the current module
- `__filename`
  the filename of the current module
- `module`
  the current module
- `process`
  the current node.js process

----

## libraries

useful libraries that are part of node core:

- `assert`  
  assertion testing
- `crypto`  
  hashing etc
- `events`  
  add listeners + emit events
- `fs`  
  file reading + writing
- `http`/`https`  
  server
- `path`  
  directory utils
- `querystring`  
  parse URL query strings
- `stream`  
  work with streaming data (like processes or HTTP requests)
- `url`  
  parse URL strings
- `util`  
  various utils like string formatting, but mostly deprecated
- `zlib`  
  zipping


----

## modules

import:
```node
require('./file')
```

export:
```node
module.exports = data
```

----

## events

there are 2 types of events in node:

- system events (managed by c library libuv)
- custom events (part of the node EventEmitter api, written in js)

when system events happen, they're handled by the custom js events api. it uses an event object similar to the DOM Event.

EventEmitter uses a pubsub style pattern for managing events, with `on` and `emit` methods

typically, you create your own custom event handlers by inherting from EventEmitter

----

## async

v8 is **synchronous** (because its running javascript)  
node.js is **asynchronous** - it can do other stuff while running javascript with v8

**libuv** is a c library that allows node to connect to the operating system and request for processes to get done  
it has its own event loop and provides a callback to v8 when an os process is complete  
v8 will finish the job its doing and then run the callback from libuv

node is **event-driven non-blocking I/O in V8 javascript**  
- node runs os I/O processes using javascript-like syntax
- node uses javascript events to manage those processes
- node runs os processes at the same time as the javascript thread, so it doesnt block the js thread

----

## files & fs

simple example:

```node
const fs = require('fs')

// synchronously read file content
const fileContent = fs.readFileSync(`${__dirname}/file.txt`, 'utf8')
console.log(fileContent)

// asynchronously read file content
fs.readFile(`${__dirname}/file.txt`, 'utf8', (err, fileContent) => console.log(fileContent))
```

----

### streams

Stream is a base class that inherits from EventEmitter

there are several types of Streams which inherit from Stream:

- Readable
- Writable
- Duplex (can both read + write)
- Transform (a type of Duplex that rewrites stream data according to a function)

like event handlers, you typically create your own custom streams by inheriting from one of the above  
node has its own custom stream objects, such as ReadStream (inherits from Readable)

example:

```node
const fs = require('fs')

// createReadStream returns a ReadStream object
// it takes an optional options object as its 2nd parameter
const readable = fs.createReadStream(`${__dirname}/file.txt`, { encoding: 'utf8' })

// since it returns a Stream, we can use EventEmitter methods
readable.on('data', (chunk) => {
  console.log(`Received text chunk: ${chunk}`);
});
```

> NB. a chunk is typically 64kb. this can be changed with the highWaterMark option in the createReadStream options object

----

### pipes

we could do the following:

```node
const fs = require('fs')

const readable = fs.createReadStream(`${__dirname}/file.txt`)
const writable = fs.createWritableStream(`${__dirname}/fileCopy.txt`)

readable.on('data', (chunk) => {
  writable.write(chunk)
});
```

this reads the stream from file.txt and writes the data chunk by chunk into fileCopy.txt in order to duplicate the file.

using a readable stream to write data elsewhere is so common that there's an easier way to do this: **pipes**

`pipe` is a method on all Readable streams. it takes a writable stream as a parameter, writes the readable stream to it and returns the writable stream.

```node
src.pipe(dest)
```

so the previous code could be rewritten as:

```node
const fs = require('fs')

const readable = fs.createReadStream(`${__dirname}/file.txt`)
const writable = fs.createWritableStream(`${__dirname}/fileCopy.txt`)

readable.pipe(writable);
```

because `pipe` returns the writable stream, we can chain pipes so long as they're Duplex or Transform (readable + writable) streams:

```node
const fs = require('fs')

const readable = fs.createReadStream(`${__dirname}/file.txt`)
const compressed = fs.createWritableStream(`${__dirname}/file.txt.gz`)

// createGzip creates a Transform stream that transforms data into a gzipped format as it reads it in
const gzip = zlib.createGzip()

// the readable stream is read into the gzip Transform stream
// it is transformed into gzipped data
readable.pipe(gzip)
// then it writes that data into our compressed stream: file.txt.gz
  .pipe(compressed)
```
