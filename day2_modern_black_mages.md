# Modern Black Mages Fighting in the Real World

メタプログラミングの技術の話。何を解決するために使うのか？

Fluentd, msgpack-inspect, Norikra, ...

---

Fluentd 0.14

everything changed.

ネームスペースを変更した
Fluent::Plugin::* 以下にプラグイン関係のものを配置、ネームスペースの衝突を回避

Class Hierarchyの整理

OutputPluginに整理

---

```
class B < A
  include M
  def foo
```

superを呼んだとき : class B の bar -> module M の bar -> class A の bar

Singleton Class (B.new.singleton_class)
Bのsingleton class -> class B -> module M -> class A

Singleton Classの中にさらにM2 moduleをincludeしないといけない

```
b = B.new
b.singleton_class.include M2
b.bar
```

Rubyではこれをすごくやりやすくするために `b.extend` ということをしてる。実際はシングルトンクラスを引きずり出してる。

----

0.14では：

methods in superclass controls everything,
methods in subclass do things only for themselves
(not for data flow, control flow nor others)

----

req: (almost) all existing plugins should work well without modification!?

---

Module#prepend

継承とシングルトンクラスがあります

毎回やってほしいものをincludeするのはだるい

---

テスト
途中のメソッドの呼び出し内容とか見たい。どうする？
moduleをprependして対象のメソッドを一旦乗っ取る、その中身でassertをかける？そうもいかない…
singleton classがあるとインターセプトできない

何よりも強い何かが必要
→singleton classにさらにprependする！何者よりも強いメソッドが手に入る！

----

fluentd is built on top of bunch of black magics :p
