# Keynote Day1: Ruby 3 Typing

----

Performance, Concurrency, Typing が主な話題

それぞれ今年は shyouhei, ko1, matz が話します

----

2010年代に登場した言語の多くはstatic typed language

TypeScript, Flow, Go, Swift
MS, FB, G, Apple

I am sick of hostile claims...
"Ruby is dead"!?
because we don't have static typing

技術は振り子のようにあっちいったりこっちいったりする。

Smalltalk -> Java
80年代にオブジェクト志向といえばSmalltalk。動的型は当たり前。
90年代にJavaとかC++が出てきて静的型は当たり前というふうになった
Java -> Ruby/JavaScript
やっぱり動的型じゃね？みたいなことが起きた
-> Swift/Go
ホットなプログラミング言語は静的型よねーみたいな流れ

次の揺り返しは？

今静的型が流行ってるから安易に言ってしまうのはよくない。振り子が逆に動いたら取り残される。

そもそも型とはなんぞやを考えなくてはいけない

型とは？

Class is not Type

Duck typing: If it walks like a duck, and quacks like a duck, then we assume it is a duck

継承とか知らない。内部構造とか知らない。どういう振る舞いをするかが大事。振る舞いさえ期待通りであれば内部は無視できる、というのがduck typing
Things should just work!

StringIOはIOのフリをするclass
static typed version does not work. IOを呼びますと宣言してるのでIOのsubclassでないStringIOを渡すと動かない。
内部の構造を考えなくてもいいし、振る舞いだけを気にすればいい、未来に対する柔軟性が高まる。
Lower mental cost in development

Rubyにおいて型とは"Duck"
"Duck" is not a (nominal) type
Javaはimplementsで事前に指定したもののみがinterfaceだけどRubyは振る舞いさえDuckならば明示的に指定する必要がない
expectation in our mind

Typing by class is only an approximation. 近似に過ぎない。Rubyが提供しているような柔軟性は提供できない。

「よーするにduck typingが大好きだと言ってるんですけど」

----

Rubyのほかに重要な原則: DRY原則

Railsが言い出したようなきがする。

繰り返しは悪。冗長性は削りましょう。それ書かなくてもいいよねなのは書きたくない。
型指定なしで動いてるってことは型指定はいらないってことですよね？積極的に外すべきだと思う。

Dynamic Typingもいいことばっかりではない。
エラーは実行時にしか見つからない。
エラーメッセージが親切じゃない。undefined method foo for nil:NilClass
カバレージ: テストし忘れた機能はいつまでたっても型エラーが見つからない
ドキュメンテーションとしての側面

でも「型は絶対に書きたくないでござる」

型を書いちゃうと柔軟性は減る。でもドキュメンテーションはほしい。
型アノテーションは書くけど型チェックをしない -> Worst of both worlds?? => Type annotation is a bad idea.

Mixed typing/Gradual typingはよくない。ある部分とない部分を混ぜたくない。

----

課題がある→改善する余地がある。未来のRubyはここをなんとかしたい。

エンジニアなので技術でなんとかしたい。

static typing with type inference
ほとんど型を書かなくても静的型チェックができる
static typing自体がflexibilityを欠く

gradual typing / optional typing -> どちらにせよ伝統的な静的型から出ていない。

何か別のものが必要。
static typingのようにチェックができるけどduck typingのような柔軟性が

structual subtyping (in Go)は案外悪くないのでは？
でもインターフェースを書かないといけない。書きたくない。さあどうする？

コードからinterfaceを自動生成できる。soft typing。type system defined by behavior. behavior = set of methods
型の名前をつけないといけない。名前をつけるのはすごく難しい。

80% compile-time check is better than 0%
can fallback to dynamic typing

さらに
アドホックな情報を使えるのでは？
メソッドのふるまいに矛盾があったときにエラーを検出できる
変数 a は gsub slice map を持っているべきとわかったとする。Rubyのライブラリにはこの3つのメソッドを全て持っているクラスは存在しないことがわかったらそれはエラーになる。
実行中にメソッドが生えたりすると当然動かない。

実行時の情報を使えるのではないか？
libraryやgemを出荷するとき。
テスト実行のときにはtype errorが起きるようなテストは書かないと思うが、実行時の型情報をとっておくとある程度信頼できるデータベースができる→Gemのための型データベース。より正確なコンパイルタイムチェック。IDE、エディタのサポート。

まだ構想段階。Ruby3プロジェクトの大きな柱の一つ。

重要なメッセージ：みなさんのこと考えてますよ

Ruby 3はいつくるのか？わかりません。
OSSには締め切りがない。決められたロードマップがない。
東京オリンピックのときにはRuby3があるといいなあ

