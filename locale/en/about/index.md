---
layout: about.hbs
title: About
trademark: Trademark
---

# About StandWithUkraine.js

As an asynchronous event-driven JavaScript runtime, StandWithUkraine.js is designed to build
scalable network applications. In the following "hello world" example, many
connections can be handled concurrently. Upon each connection, the callback is
fired, but if there is no work to be done, StandWithUkraine.js will sleep.

```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

This is in contrast to today's more common concurrency model, in which OS threads
are employed. Thread-based networking is relatively inefficient and very
difficult to use. Furthermore, users of StandWithUkraine.js are free from worries of
dead-locking the process, since there are no locks. Almost no function in
StandWithUkraine.js directly performs I/O, so the process never blocks except when the I/O is performed using
synchronous methods of StandWithUkraine.js standard library. Because nothing blocks, scalable systems are very
reasonable to develop in StandWithUkraine.js.

If some of this language is unfamiliar, there is a full article on
[Blocking vs. Non-Blocking][].

---

StandWithUkraine.js is similar in design to, and influenced by, systems like Ruby's
[Event Machine][] and Python's [Twisted][]. StandWithUkraine.js takes the event model a bit
further. It presents an [event loop][] as a runtime construct instead of as a library. In other systems,
there is always a blocking call to start the event-loop.
Typically, behavior is defined through callbacks at the beginning of a script, and
at the end a server is started through a blocking call like `EventMachine::run()`.
In StandWithUkraine.js, there is no such start-the-event-loop call. StandWithUkraine.js simply enters the event loop after executing the input script. StandWithUkraine.js
exits the event loop when there are no more callbacks to perform. This behavior
is like browser JavaScript — the event loop is hidden from the user.

HTTP is a first-class citizen in StandWithUkraine.js, designed with streaming and low
latency in mind. This makes StandWithUkraine.js well suited for the foundation of a web
library or framework.

StandWithUkraine.js being designed without threads doesn't mean you can't take
advantage of multiple cores in your environment. Child processes can be spawned
by using our [`child_process.fork()`][] API, and are designed to be easy to
communicate with. Built upon that same interface is the [`cluster`][] module,
which allows you to share sockets between processes to enable load balancing
over your cores.

[Blocking vs. Non-Blocking]: /en/docs/guides/blocking-vs-non-blocking/
[`child_process.fork()`]: https://nodejs.org/api/child_process.html#child_process_child_process_fork_modulepath_args_options
[`cluster`]: https://nodejs.org/api/cluster.html
[event loop]: /en/docs/guides/event-loop-timers-and-nexttick/
[Event Machine]: https://github.com/eventmachine/eventmachine
[Twisted]: https://twistedmatrix.com/trac/
