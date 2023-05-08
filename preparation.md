こんにちは。ぽっけです。

RubyKaigi 2023 で RBS に関するトークをします。この記事では、そのトークをより楽しむために予習しておくと良い内容をかんたんに紹介します。

[https://rubykaigi.org/2023/presentations/p_ck_.html#day3:embed:cite]


## トークの内容

このトークでは、RBSを使ったRubyアプリケーションの開発にフォーカスしています。

このトークの軸は2つです。1つは最近リリースされたRBS 3.1の新機能を紹介すること。もう1つはRBSを使った開発を実際にデモして雰囲気を味わってもらうことです。


## 予習しておくと良さそうなこと

今回のデモではRBSの様々な機能を使う予定ですが、RBS 3.1以前からある機能についてはあまり解説をしません。また、RBSの構文なども解説しません。そのため、それらの内容を予習しておくとよりトークを楽しめるでしょう。

ということで、予習しておくと良さそうな資料へのリンクをまとめておきます。

### RBS自体について

もしRBSの構文などを何も知らない場合、ruby/rbsリポジトリ内のRBS By ExampleというドキュメントがRBSの概略を知るのに役立つでしょう。

[https://github.com/ruby/rbs/blob/master/docs/rbs_by_example.md:embed:cite]

より詳しい構文を知りたい場合は、以下のドキュメントがおすすめです。

[https://github.com/ruby/rbs/blob/master/docs/syntax.md:embed:cite]

### rbs collection

rbs collection は RBS 2.0 で追加された、ライブラリのRBSを管理する機能です。

この機能については、私がRubyKaigi Takeout 2021で発表しました。

[https://rubykaigi.org/2021-takeout/presentations/p_ck_.html:embed:cite]

また公式リポジトリ内のドキュメントも参考になるでしょう。

[https://github.com/ruby/rbs/blob/master/docs/collection.md:embed:cite]

### rbs prototype

rbs prototype はRBSを生成するための機能です。

ちょっと古いですが、以前自分が書いた記事があるので参考になるかも知れません。

[https://pocke.hatenablog.com/entry/2020/12/18/230235:embed:cite]

実際に`rbs prototype`コマンドを使って試してみても良いでしょう。

### Steep

今回のデモでは型解析器としてSteepを使っています。Steepのドキュメントには詳しくないですが、公式リポジトリのREADMEから辿れる情報を読むと良いかも知れません。

[https://github.com/soutaro/steep:embed:cite]


### RBS Rails

今回のデモではRBS Railsも少し使う予定です。こちらも公式リポジトリを参照しておくと良いでしょう。

[https://github.com/pocke/rbs_rails:embed:cite]


### rbs subtract

今回のトークのメインコンテンツです。
なのでトークの中で十分に説明をするのですが、興味があれば事前に資料を読んでおいても良いかも知れません。

`rbs subtract`は以下のPRで実装されました。

[https://github.com/ruby/rbs/pull/1287:embed:cite]

また、このPRにもリンクがある次のドキュメントでデザインが詳しく説明されています。

[https://hackmd.io/seoMijXwRdG2uFITm2lLqw:embed:cite]


実行をすると何が起きるのかは、以下のテストコードを見てみるとわかりやすいでしょう。

[https://github.com/ruby/rbs/blob/master/test/rbs/subtractor_test.rb:embed:cite]


---

いよいよRubyKaigiも間近となりました。松本でお会いできるのを楽しみにしています。
興味があればトークを聞きに来ていただけると幸いです。
