# Who reordered my code?

Live example of Decker's algorithm

flag1 = flag2 = false

th1 : flag1 = true -> flag2 ? contention : critical_section
th2 : flag2 = true -> flag1 ? contention : critical_section

----

Ruby goals

Performance
CRuby 3x3
Ruby OMR Preview (IBM J9)
JRuby (RH)
JRuby + Truffle (Oracle)

Concurrent library
actors, isolation, channels, streams
easy to use high-level concurrency abstraction

unanswered -> How do we write fast concurrent data-structures? How do we write more concurrent abstractions?

----

When you can see reordering

conditions: 

- Fast Ruby implementation
- Parallel execution

fast ruby imp = speculatively optimizing dynamic compilation and parallel execution is needed

speculative: method body is stable, constant's value is stable, type speculation, ... ( make assumptions )

optimizing: does all the clever optimizations as e.g. GCC

dynamic: JIT compilation of hot methods + also deoptimize when speculatively taken assumptions fail

JRuby + Truffle is such an implementation

----

Sources of reordering

(1) Compiler
-> optimizes by transforming the code

the code has the same result / assumes they are on the same thread

"seemingly sequential" ruby code

(2) even if compiler doesn't, the processor could do it anyways!

(3) Cache reordering

flag1 = true -> goes to store buffer
flag2 read -> reads from global memory

----

We do not care who reordered the code; only the actual execution matters

Do we want reordering? -> Yes.
W/out reordering, even basic code transformations would be forbidden.
We want to let the code be reordered because we want it to run faster than we wrote it

-----

Taming reordering

Sequential consistency

allows to reason about the program as if it is executed interleaved on one thread even though it's executed in parallel on many threads
cannot be done for all variables

(力尽きたので後で追い返す)

