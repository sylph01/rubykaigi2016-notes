# Scalable Job Queue System Built With Docker

ジョブキューとは何か

Webアプリケーションではタスクを非同期的に処理するためのもの。遅いタスクを後でやるためのもの。

Rubyでjob queue systemをどうやって使うか
一般的にはGemを使う。resque, sidekiq, delayed_job, shoryuken, ...

Railsではabstraction layerでどうにかする。

Web consoleではresque-web, sidekiq(web console bundled), ...

何でできてるの？
データストア
ワーカー

Redis
RDBMS (MySQL, PostgreSQL)
RabbitMQ
Amazon SQS

ワーカーはデータストアに依存してる
Redis -> resque, sidekiq
RDBMS -> delayed_job
Amazon SQS -> shoryuken

----

Barbequeの紹介

つい最近OSS化したばっかり！
job queueのcoreが入っている

worker
  serverengineでできてる
  Dockerで実行
  slack通知
Web API
Web Console(rails engineです)
  manage registered applications and notifications
  you can see a log stored in S3

----

何で新しいjob queue systemが必要になったのか？

昔の話：ごく一部のアプリでしか使ってなかった。
Resque - 管理用アプリで使ってた。unicornでapp + console, resqueが動いてる
resqueはマルチプロセスで動く。スレッドセーフを気にしないでよい。Redisが分散されてなければ複数のメッセージが配信されることはない。
Sidekiq - Resqueより速い（曰くreadme）。スレッドセーフに実装されている必要がある。

何で使ってなかったの？
kuroko2っていう素晴らしいスケジュールバッチシステムがあった
web consoleがリッチ、Docker実行をサポートしていた
便利すぎてCookpadエンジニアがみんな使っちゃう。ジョブキューを使うべきところにkuroko2を使ってしまってた。
アプリケーションが増えてきたので…

(kuroko2はそのうちオープンソース化するらしい)

環境が80個以上ある。これにジョブキュー1個ずつつけると160個になってしまう。つらい。管理しやすくてスケールするような集中管理のもの。
ジョブが簡単にデプロイできる。
kuroko2-like web console

ほとんどのアプリケーションをDockerでデプロイしている。80+のデプロイ設定。
AWS ECSでDocker containerをclusterとして動かしてる。

----

内部がどのように設計されてるか？

QueueにはAmazon SQSを使っている。
＋: 速い、分散、reliable
問題点: QoSがat-least-once（複数のメッセージを送信しうる）、メッセージのdelayが900sまでしかできない（1日後にenqueueしたいとかできない）

SQSはat-least-onceのQoS
at-most-once  : unreliable but no duplication
at-least-once : reliable but duplicate may happen
exactly-once  : ほんとかよみたいなQoS。分散環境だと排他制御が必要、スケールしにくい

ECSでジョブを実行
hako という gem でやってる。自前のデプロイツール。
hako adapter for Barbequeはまだクローズソース…

RDS(MySQL)にstatusをstore
書き込みはworkerからしか起こらないようにしている
barbeque workerがhakoでECS batch clusterにコマンド実行
ログはS3に吐く
でstatusを最後にRDSに吐いて終了

AutoScalingが一番のメリット
ジョブを実行するために必要なリソースを正確に予測できる（スケールさせる）ことができている

----

まとめ

Dockerでひとつの環境上で集中管理したい
同じサーバーに複数のサーバーのcredentialが同居してて治安が悪い、みたいなのをDockerでクリーンな環境立ててできるようになった
リソースをうまく増減させることができるようになった

GH: cookpad/barbeque