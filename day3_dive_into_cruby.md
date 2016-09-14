# Dive into CRuby

CRubyに貢献したい人向けの話

* 新しい機能を追加する
   * 新しいプラットフォームでの動作、も含む
* 高速化・最適化する
* トラブルシューティングのための道具を提供する

## 新機能

プラットフォーム clang, VC++ 2015

なぜ新機能？
なぜ今あるものではだめなのか？
既存のメソッドがユースケースに合わない場合。ユースケースは実際のものでなくてはならない。人工的なものではダメ。

必要は発明の母、というけれど…「顧客が本当に必要だったもの」

Feature #6752 Replacing ill-formed subsequence
不正なバイトを修正したい
もともとutf-8のものを渡すとうまくいかなかった

ウェブフォームの例が挙げられていた → そもそも問題が起きた場合400 Bad Requestで返さないといけない。ほんとにちゃんとした理由なの？
ファイルを読むとき → 大抵適切なエンコーディングで読もうとしていないだけ。適切なencを指定せよ。
ウェブクローラーを作る場合 → リトライができないけど壊れた文字列をハンドルできる必要がある。Twitterっていうみなさんが使っているであろうwebsiteがありまして…。昔はWebページのタイトルをさらにきりつめるときに失敗していた。こういうページをちゃんと読む機能が欲しい。

インターフェースの設計
transcodeベース : iconvで慣れてる
regexpベース
→ String#encode, String#scrub の2つを作った
「21cにもなって文字コード変換なんてそんなにしないよね…」

doc: 「有益な情報がないと思ったらパッチ送ってください」

build: なかださんのすみか

platforms: cygwinはメンテナがいない

----

CRubyでは _p ってついたらだいたいRubyの ? と同じ

----

やりたいことが見つからなかったらけっこう大変

Feature #7361 (rejected)

behave like touch(1) といわれても…。2つの機能で使われている。ファイル日時を更新するのか、ファイルを作るのか。

----

clang (llvm-gcc)

gccよりもさらにアグレッシブに最適化してしまいRubyのGCを壊してしまった。 r34278（continuationのメモリアロケーションに関する修正）

VC++
Universal C Runtimeがcompatを壊してしまった

----

Performance improvements

速くする

* ベンチマークする stackprof, derailed_benchmarks
* ボトルネックを探す
* ボトルネックを最適化する
* 速くなる！

`String#blank?` : unicode aware string emptiness

RubyVMを速くする

「Ruby layerのボトルネックはほとんどない」、ならVMだ！

OProfile
perf(1)

vm_exec_core, vm_search_methodがだいたい重い。それはそう。

----

when stuck?

perf top
pid2line

-> カーネルが無限ループに入った場合とか

strace
FUTEX_*っていうシステムコールが大量に出てる：これはタイマースレッドが生きてるプロセスから情報得ようと必死な状態で正常。

procfs(5)
(30) kstkeip %lu : The current instruction pointer
今走ってる命令の場所がわかる
どのアドレスにどのファイルがマップされてるかを見る
ライブラリのsymbol tableを見て探す

pid2line.rb
perfが走る/をインストールする権限ないところでも動く

SEGVの場合
大変重要な情報が詰まってる
最初から最後まで全部重要な情報

通常は非公開になっている関数の名前は取れないはずなのだけどデバッグのために名前を入れている。gcc -gでコンパイルすればつく。DWARF形式。