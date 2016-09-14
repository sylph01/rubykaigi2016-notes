# Web Server Concurrency

(concurrency in general)

nothing special about web server architecture. just a server architecture

Simplest Ruby web server...

`ruby -run -e httpd -- -p 8080 .`

kind of cheating, uses un.rb

---

A server needs a way to receive requests and to return responses

(1) Native ruby networking support (TCPServer and friends)
(2) EventMachine

A simple Ruby web server : Scrawls

Pluggable IO engines
Pluggable HTTP parsing

Shared core makes it easy to use to look at the impact of different concurrency options

Pluggable IO engines:

single threaded
multiprocess
multithread

---

Main Loop
single threaded
blocking - waits until a request is handled before accepting and handling a new one

scrawls --ioengine single

How to address problems of single thread

Concurrency: Decomposibility of a problem into order independent or partially ordered units
= chunks of work can happen independently of each other

-> we want independent events to happen at the same time = parallelism

---

Multiprocessing

just run a bunch of blocking servers, and have something else distribute and balance the load for them.

managing processes can be complex
limited sharing of resources can be expensive

very simple to implement

Listen on a port, then fork. A child processes share opened ports. OS load balances.
Does not work on Windows. Does not work on JRuby.

Multiprocessing Simple Blocking Server

Ruby pre-2.0 was very copy-on-write unfriendly & multiprocessing consumed large amounts of RAM
Modern rubies are more resource friendly when forking

----

Multithreading

problems:
threading implementations vary a lot across Ruby impls
Threading is complicated, easy to screw up

Diminishing returns when thread count gets high wen there is no thread pool

----

Event-driven

EventMachine, Celluloid.io, SimpleReactor

