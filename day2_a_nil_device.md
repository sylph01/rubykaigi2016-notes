# A Nil Device, a Lonely Operator, & a Voyage to the Void Star

Ruby Wizardryの人！
Idrisのコミッタ！

---

A Nil device

special singleton class : NilClass FalseClass TrueClass

Nil is a value
RUBY_Qnil = 0x08 or 4 (depending on USE_FLONUM or not)

nil.object_id => 4 # in 1.9.3
nil.object_id => 8 # in 2.3.0

Nil in Ruby

nil.class => NilClass
nil.singleton_class => NilClass

NilClass.instance_methods(false).sort

Nil is an API

Nil means something
No value, No meaningful value, Neutral value, failure, ...

---

A Lonely Operator

my_hash[:imaginary] => nil
@oops => nil (improperly initialized instance variable)
I/O: File.size? 'pretend.txt'

Common theme : I/O and mutation can lead to nil

`&.` : safe naviagtion operator
in Swift: `?.`

Why is Nil dangerous?
-> If we don't handle nil, it infects everything

ActiveSupport's Object#try

```
@fox.try(:best_friend).try(:favorite_food)
```

requires active support, we already have language level support (2.3.0 or later)

Null Object Pattern

----

