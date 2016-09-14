# Unifying Fixnum and Bignum into Integer

Ruby 2.4からFnとBnがIntegerに統合されます

Rubyプログラマにとっての影響
FixnumとBignumクラスがなくなります
Fixnum、Bignumっていう定数（名前）はIntegerクラスを指すようになる（NameErrorは起きない）

C拡張する人
内部表現は変わりません
rb_cFixnum, rb_cBignumというglobal constantはなくなります
class-based dispatchは動きません
RUBY_INTEGER_UNIFICATION macroが定義された

----

Integerの実装について。

Fixnumの範囲はポータブルでない
32bit CRuby -> -2^30 to 2^30 - 1
64bit CRuby (LLP64) -> -2^30 to 2^30 - 1
64bit CRuby (LP64) -> -2^62 to 2^62 - 1
JRuby : -2^63 to 2^63 - 1
(LLP64とはWindowsのこと)

Rubyには標準規格というものがあります。JISにもISOにもなってる。
Integerクラスはrangeはunbounded
実装によってサブクラスを定義してもよい
FixnumやBignumという名前はどこにもでてこない。国際規格では定義されていない！定義することは許されている。
Ruby 2.3、Ruby 2.4はどちらもspecificationには準拠している
規格に準拠するプログラムはFixnumやそのrangeに依存してはいけない

規格は2012年やそのちょっと前くらいに書かれている
予想して書かれたわけではない
「内部実装だからわざわざ書かないようにしよう」という判断をしたのでは…？Fixnumの仕様がportableでないことを受けてそう判断したのか…？

lib/rubygems/specification.rb で不思議な使い方をしてる（Fixnumのrangeに依存した書き方になってる）

Ruby 2.4では原理的にFixnum/Bignumに依存できない。obj.is_a?(Fixnum)はobj.is_a?(Integer)と同義だから。

1が整数であることは知ってる。ただし1がFixnumであることはRubyを勉強する前に知ってる人はいないのでは？
教科書を簡単にできる。Programming Ruby 2nd Edition p.59　→ 8行くらいの文章が3行になる。
ドキュメントの重複が減らせる。rdoc, ri

Fixnumの上限を探すコードはbrokenになる
test/ruby/test_integer_comb.rb
無限ループになる。メモリを食いつぶすまでループする無限ループ。
CRuby only non-recommended solution: RbConfig::SIZEOF['long'] を使う
JRuby？勝手にやってください。

SequelではFixnum/BignumをDSLの一部として使ってたらしい。Bignumという単語を使いたいだけであればSymbolを使え。

---

利点はほぼ初心者向け。
微妙にincompatibilityがあってライブラリが更新されないと厄介だけど…