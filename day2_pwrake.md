# Pwrake

Parallel Workflow extension for Rake
科学技術計算をクラスタ上でやるために開発した
Rakeタスクをconcurrentに実行
shコマンドを remote computing nodesに送る

例: Montage (astronomy image processing)

Directed Acyclic Graphでワークフローを頂点

ワークフロー記述言語

* マークアップ : DAX (DAGをXMLで表現したもの)
* 新しい言語を作る : Swift(Appleのではない)
* 既存の言語を使う : GXP Make

RakeだとRuby scriptでかける、pathmapという機能がある

Rakeをワークフロー記述言語として使う