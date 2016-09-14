# A Tale of Two String Representations

JRuby + Truffle
GraalVM ( http://www.oracle.com/technetwork/oracle-labs/program-languages/overview/index-2301583.html )
New High-speed Ruby implementation

----

byte array,
byte length,
encoding,
code range
(+ additional fields)

----

RStrings
Mutable, Flat representation (requires contiguous memory)
Byte-oriented
Shares memory via copy-on-write of byte array

----

Ropes

immutable tree structure

lazy operations + encoding information

ConcatRope
SubstringRope
RepeatingRope
LeafRope - AsciiOnlyLeafRope / ValidLeafRope / InvalidLeafRope

----

Operations

Concatenation

```
x = 'Hello'
y = ' world'
z = x + y
```

RString
O(n) operation
1 memory allocation (z.size = x.size + y.size)
2 memory copy operations
2 copies of x and y in memory

Rope
O(1) operation
1 Node allocation
0 memory copy op
1 copy of x and y in memory

Concat -> Ropes 80x+ faster
Append -> Ropes 2.3x performance (slower than RString)

Concatenation vs Append relative performance
MRI : + vs << is 40x
Ropes : performance is negligible

----

Substring

w/ Ropes, do not need a substring allocation, just allocate a rope

Multiplication

w/ Ropes, just allocate a RepeatingRope

all lazy operations. no memory allocation until we need it.

----

String Length Performance

17x faster than MRI. Constant time operation vs MRI/JRuby does linear time operation

'こにちわ'.bytesize => 12
'こにちわ'.size => 4

cache the bytesize inside the rope itself -> constant time operation!

we shouldn't have hidden performance pitfalls based on encoding (ASCII vs non-ASCII)

string length is invoked alot inside implementations for bound checking

----

Wrapping up

Rope benefits

* allow for interning of strings
* metaprogramming
    * faster name comparisons
* final values are great for Graal (partial evaluation)
* implicitly thread-safe
	* you will never see mysterious byte updates even if you have two threads race for it

Deficiencies

* Not good for "Byte buffer" use case
* Node costs dominate smaller string fragments
* While some ops are faster, others are slower
* Less production experience

----

"Ropes: an Alternative to Strings" (paper on Ropes)

