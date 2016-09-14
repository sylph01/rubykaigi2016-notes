# Hijacking syscalls with (m)Ruby

System call = The interface between low-level libs, and the OS

---

Why hijack system calls?

* Fault injection (like Chaos Monkey kind of tool)
    * Simulate memory allocation issues, slow/throttled IO, network issues, failing devices, ...
* Real-time, interactive monitoring
* Doing it better than FakeFs/Timecop
* Securing Ruby apps

---

Securing Ruby apps is tricky

Gem install is insecure
Some defaults are kind of ...
Code audit is near impossible : Tracking self-modifiable code is hard / Some regulation require companies to show they've run an audit of their gems

gem fetch

---

How can one replace syscalls -> ld

LD_PRELOAD=
DYLD_LIBRARY_PATH=

dlsym(3)

Library preloading does not require re-compilation = Non-intrusive

----

Why use mruby? -> 

* It's ruby (v)
* Higher-level interface : can become External DSL for C

truncate(2)
mrb_funcall()

---

Performance (it gets ugly)

