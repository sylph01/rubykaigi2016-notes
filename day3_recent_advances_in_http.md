# Recent Advances in HTTP, controlling them using Ruby

## HTTP/2の現状

バンド幅ではなくレイテンシがWebのボトルネックになってきた
concurrencyを増やすことでレイテンシを隠蔽する

去年の5月に標準化が終わった。H2は去年7月で18%。今や全体の3割がH2を使ってる。（Firefoxから見たサーバー側の対応状況）

HTTP→HTTPSへの移行が進んでいる

## HTTP/2のフィーチャ

* ヘッダ圧縮(HPACK)
* multiplexing/prioritization
* push

### HPACK

- median 90% reduction
- 80th percentile 75% reduction
- 90th percentile 10% reduction

### multiplex/priority

クライアントがヒントを送る

### push

20%-30% speedup

47%くらいのpushが不要なpush
render timeが増えているのでは？
pushではなくpreloadのほうがいいのでは？

今後CDNのエッジサーバからpushするというようなことができるといいと言われてるがどうしたらできるのか？

## 修正

理想とするHTTP transactionとは？

* 優先順位が高いリクエストにサーバーがすぐに反応する
* CSS/JS -> HTML -> 画像
* push only the resources not cached by the client

実際どれもできてない。

---

head-of-line blocking
high-priority data blocked by preceding data in flight
TCPバッファに未送信ブロックが滞留しちゃう

CWNDサイズとunackedサイズはTCP_INFOでわかる→滞留するデータを0にすることができる
poll threshold = 書いていいよーとするタイミングを制御する

---

HTTP2の優先順位付け

weights, chaining

Safari, Blinkはprioritizationに失敗している
画像がほとんどのバンド幅を占めちゃう。ほんとはCSSが欲しいのに…

ルーターとかでパケットの優先順位付けするのと同じようにする。WFQ, DRR
nghttp2 -> WFQ O(log N) / H2O approximates WFQ in O(1)

頭の悪いクライアントをどうするか
H2Oは縦方向の優先順位を使ってなければとにかくCSS, JSを先に送るようにしてる

---

hidden resource

CSSの中にCSSとかJSとか参照があるとリソース見つけるまでに時間かかるしその間に画像流れちゃう。
これはwebサイト作る側が対策する必要がある。結合するかpreloadヘッダを指定するか。

----

## push

pushing for prioritization

HTMLが来てからその中身見てCSS取りに行くのではなくリクエスト受けたら即時CSS返しちゃう

Link preloadヘッダをつけるとH2サーバーはpushするかもしれない
pushしてもらっては困るものはrel=preload;nopushとするとnopush
（preloadヘッダつけろ、ってのは現在標準化が進行中）

リクエスト処理中にpushする
interim response
HTTP request 1つに対して ＜0個以上の中間レスポンスと＞ 1個の最終レスポンスを送ることができる
100 Continue

mruby handlerでH2Oはサーバーをconfigureしてpushを制御してる
「mrubyとか使って速いの？」→アルゴリズムでどうにかしてる。Trie構造を使った高速なIPマッチングとか。

エッジ(cdn)からpush
RUM(real-user-monitoring)-based approachをしているものもある
DSLを使ってやりたいという場合もある。GCPはhttp2-push-manifestでやってる…けどedgeからのpushはできない

### push vs cache

cached resourceをpushしたくない。bandwidthの無駄。

- cookie-based (H2O supports)
    - どのブラウザもサポートしてる
- cache-digest
    - Apache, H2O
    - ServiceWorker scriptとかのブラウザサポートが必要
    - IETFでの標準化を待つしかない
- 自前実装

### avoid negative effects caused by push

- cache-awareなメカニズムじゃなければpushしないほうがいい
- レンダリングをブロックするようなリソースをpush
    - CSS/JSなど確実に効果が出るところだけしたほうがいい
- tiny pushには適用されない


