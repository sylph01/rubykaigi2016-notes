# How DSL works in Ruby

```
task :awesome do
  puts :bar
end

task beat: [:awesome] do
  puts :buzz
end

task default: :beat
```

beatタスクの前提条件がawesomeなので先にawesomeが実行される（依存タスク）

`rake -j` でparallel executionできる（闇。何かがparallel executionできる…？）

Rake::FileList

Rake::TestTask

----

Rubyでは内部DSLとして使っているケースが多いのでは

---

Class method

```
class User
  has_many :foo
end
```

Module and Class ancestors

---

Implicit code block

```
Foo.configure do |c|
  c.bar = :buzz
end
```

プロキシを設定するとか、デバッグモードをOnにするとか、

```
module Foo
  def self.configure
    yield self
  end
  
  class << self
    attr_accessor :bar
  end
end
```

dry configurationとか使うとできるのかな？

---

Declarative setter

```
Foo.configure do
  bar :buzz
end
```

instance_eval(&block) する

---

```
gemfile = <<-GEM
  gem 'foo'
GEM

Foo.new.eval_gemfile(gemfile)
```

```
class Foo
  def gem(name)
    p name
  end
  def eval_gemfile(file)
    instance_eval(file)
  end
end
```

----

ここまでのパターンを組み合わせると大抵のRubyのDSLは作れる

Rake. Capistrano, Thor, Bundlerを見ていこう

---

Rake::Application#top_level

top_level メソッドは rake foo bar baz と書くと順番に実行する

load_rakefile : デフォルトファイル名か指定したファイルを load でガッとloadする。Rubyのファイルとして。

----

Rake::Task

Rake::TaskManager#define_task で定義される
Rake::Taskはmanipulation taskを持つ。clear, tasks, [], ...

(Capistranoのところから力尽き気味)

Capistrano 3 の task は実質 simple rake task
DSL拡張
3からrakeと一心同体

----

Thor: rakeとは関係ない

self-documenting command line utilityを作るためのもの

----

Bundler

Bundler::CLIはThorを継承してる。simple thor command

----

Rake 10から
ひたすらpull requestとかissueとかを見るところからはじめた
JRubyに詳しくなった
