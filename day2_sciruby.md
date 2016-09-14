# SciRuby: Machine Learning Current Status & Future

Rubyでmachine learningのalgorithmを書くのではなくdata scienceのworkflowをしたい。
機械学習に関係する工程でいくつがRubyでできる？ → できません。
個別の事例ならともかく、一般的には全部できないと思ったほうがいい。諦めることになる。Pythonは全部できる。だからみんなPython使うんや。

RubyをDSで使える言語にしたい。何が問題か？それを明らかにしたい

----

なんで機械学習？

* 実際のデータから意思決定したい
* アルゴリズムを使うかどうかはoptional
* 膨大なデータが集まっていて全体の傾向がわからないときは機械学習の手に頼ることになる

* 人にはできないタスク
* 現実的に解き方がわからないタスク
* を解決できる

情報推薦、異常検出、感情分析(sentiment analysis)、...

教師あり学習、教師なし学習、強化学習

----

liblinear-ruby.gem

rb-libsvm.gem 分類、回帰、cross-validation

decisiontree.gem ID3 decision tree (https://en.wikipedia.org/wiki/ID3_algorithm)

----

機械学習で「実用的」とはどういうことか？

* 量がたくさんあって
* 次元が高い
* 欠損値も多い

「cleaningに60%かかります」(これの前のsession)

どんなアルゴリズムが適してるか事前に判断できない

パラメータの渡し方、データフォーマットが揃ってる必要がある
cross validationが全てのアルゴリズムで必要

SciRubyでは実用的なレベルには達してない。Pythonではscikit-learnでできる。

----

Scikit-learnについて。
SciPyスタックの機械学習フレームワーク

NumPy, SciPy, Pandas, Matplotlib, Jupyter Notebook, ほか



