# Node.js

- background info
  - V8
  - servers for idiots
  - why node
- libraries


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

### globals

- Buffer
  work with memory allocations (ES6 TypedArray does similar stuff)
- `__dirname`
  the dir of the current module
- `__filename`
  the filename of the current module
- `module`
  the current module
- `process`
  the current node.js process

----

### libraries

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

### modules

import:
```js
require('./file')
```

export:
```node
module.exports = data
```

----
