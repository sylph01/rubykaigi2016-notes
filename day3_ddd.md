# Deletion Driven Development: Code to delete code!

Static code analyzer + point out unused code

## Parsing the code

- CFG
- BNF

(build time) parse.y -> (bison) -> parse.c

Ruby uses LALR parser generation called Bison

https://github.com/tenderlove/racc
https://github.com/seattlerb/ruby_parser

## Processing the s-expression

S expression processor -> Method tracking processor

## Building the dead method finder

keep track of known methods and called methods
difference between known methods and called methods -> uncalled methods

`dog.send(:pet)` is not processed properly! edge case

when `send, public_send, __send__`, handle the edge case

`attr_accessor` is never actually used!

----

Ruby is complex (to parse)

Edge cases...

`reuben.fed = true`

doesn't work yet.

Rails DSLs, your own helper DSLs, ...

GH: seattlerb/debride

https://github.com/tcopeland/olde_code_finder
https://github.com/joshuaclayton/unused

method Aでmethod Bを呼んでてBでAを呼んでるけどその2つは他から参照されない、みたいな