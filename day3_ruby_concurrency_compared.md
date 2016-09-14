# Ruby Concurrency Compared

Concurrency vs Parallelism

by Joe Armstrong:

- Concurrency: Two queues and one coffee machine
- Parallelism: Two queues and two coffee machines

Conc: about dealing with lots of things at once
Para: about doing lots of things at once

Why we care?

- power wall
- memory wall
- ILP (instruction-level parallelism) wall
- which means: only solution is multi-processing

Scheduling

- preemptive : task/thread can be temprorarily suspended
- non-preemptive : task/thread cannot be suspended
- cooperative : a process voluntarily gives up to other process

How to do

* Thread/Mutex
* STM
* Actors
* Processes/IPC
* CSP
* Actors
* Futures/Promises
* Co-routines
* Evented

---

Java : Threads

shared mutability is root of all evil

（あとで読んだほうがよさそうなので以降控えめに略）

---

Clojure: STM

