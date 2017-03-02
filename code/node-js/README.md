# Node.js

- background info
  - V8
  - servers for idiots

----
## background info

### V8

V8 is actually written in c++ - it takes javascript and outputs machine code.

node.js is also written in c++. it is a "runtime environment"\* that uses v8 to process javascript and also adds some features. so the extra javascript that's possible with node is possible because it converts it to c++ code.

\* a runtime environment is the system that executes code live, with the help of a compiler (like v8). an operating system is a kind of runtime environment.

----

### servers for idiots

a server is a computer computer connected to the internet. it has an operating system installed but its only job is to run the server program that sits on it. these programs **listen for requests and return a response** and use a protocol called HTTP.

when you type a website into a browser, the browser does a DNS lookup to workout the IP of the server computer. the computer is on all day and the operating system lets the server program know when it receives a HTTP request.
