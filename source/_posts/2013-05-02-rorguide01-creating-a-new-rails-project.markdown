---
layout: post
title: "[RailsGuides] #01 RailsGuides で Rails をはじめてみる"
date: 2013-05-02 08:56
comments: true
shareing: true
published: true
categories: [Ruby on Rails, tutorials]
---
ちゃんと [Ruby on Rails](http://rubyonrails.org/) (以下、Railsと略します) を覚えていないので、 [RailsGuides](http://guides.rubyonrails.org/) をはじめてみました。この記事自体は、誰にメリットがあるかわからない内容ですが、ここでは RailsGuide の日本語での抜粋と実際に手順にに沿ってやってみたことなどなどを、その都度だらだらと書いていきます。ちゃんとした和訳をお探しの方は、以下のサイトを参照すると良いと思います。

 - [ruby/rails/RailsGuidesをゆっくり和訳してみたよ/Getting Started with Rails](http://wiki.usagee.co.jp/ruby/rails/RailsGuides%E3%82%92%E3%82%86%E3%81%A3%E3%81%8F%E3%82%8A%E5%92%8C%E8%A8%B3%E3%81%97%E3%81%A6%E3%81%BF%E3%81%9F%E3%82%88/Getting%20Started%20with%20Rails)

<!-- more -->

{:TOC}

幸いにも RailsGuides は mobi 形式でも配布されているので、Kindle for iPad を横に置きながらぼちぼちとやっていきます。

ちなみに [原文](http://guides.rubyonrails.org/) は [Creative Commons Attribution-Share Alike 3.0](http://creativecommons.org/licenses/by-sa/3.0/) の元で公開されています。

Rails の環境
------------
いろいろなサイトがあるので、"rails 入門" や "rails インストール" などのキーワードで検索すれば、きっとインストールの仕方など、ご自分の環境にあった情報が得られるでしょう。

私の環境は、以下の通りとなっております。
```bash Rails実行環境
$ ohai platform platform_version
[
  "centos"
]
[
  "6.3"
]

$ rvm --version

rvm 1.18.21 (master) by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.io/]

$ ruby --version
ruby 1.9.3p392 (2013-02-22 revision 39386) [i686-linux]

$ rails --version
Rails 3.2.12
```

以下、見出しは基本的に RailsGuides の見出しに準じます。

1 このガイドの前提
------------------
対応する Ruby のバージョンと前提としている Rails のバージョンについて記述しています。Ruby 1.9.2 系 と Rails 3.2 系をインストールしていれば、ほぼ問題なく動作すると思います。それに加えて、まず Ruby を覚えてから Rails をやった方がいいよなどと書かれていますが、[はじめてのRuby](https://www.google.co.jp/search?q=%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AERuby&aq=f&oq=%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AERuby&aqs=chrome.0.57j0l3j62l2.11261j0&sourceid=chrome&ie=UTF-8)を読んでいれば理解できると思います。わからなければ、その都度検索エンジンのお世話になれば良いではないですか。

2 Rails とは何か？
------------------
Rails の特徴などが書かれています。省略します。

Rails は「生産性が高い」、「すぐにアプリケーションを作成できる」などといわれていますが、Web アプリケーションフレームワークなので、個人的には比較的学習コストが高いと思います。よく Rails の特徴として __「設定より規約を」__ と言われています。誤解を恐れずに言うならば、つまりは__規約を覚えなければ話にならない__わけです。

以下については、このガイドに説明があるので、単語だけ覚えるだけにとどめて、ガイドを読み進めるうちに覚えるか、検索エンジンのお世話になるかすれば良いと思います。最初からわかっていれば入門ガイドのお世話になる必要もありませんしね。

 - Action Pack
   - Action Controller
   - Action Dispatch
   - Action View
 - Action Mailer
 - Action Model
 - Action Record
 - Action Resource
 - Action Support
 - Railties

3 Rails プロジェクトを新規作成する
----------------------------------
### 3.1 Rails のインストール
RubyGems からインストールするのが簡単です。
```bash 
gem install rails
```

Windows を使用している人は Ruby と Rails を [Rails Installer](http://railsinstaller.org/) で簡単にインストールできます。

### 3.2 blog アプリケーションを作成する
私は GitHub 用にソースファイルをアップする予定なので、github というそのままの名前のディレクトリに `rails new` で作成する予定の blog というディレクトリを作成しました。Git の話を絡めるとややこしくなるので、リポジトリの作成と最初の push のコマンドまではここに記述しますが、これ以降は Git のコマンド等の話は省略します。蛇足になりますが、私は GitHub でリポジトリをあらかじめ作成しました。

```bash あらかじめblogディレクトリを作成して下準備をする
$ cd ~/github
$ mkdir blog
$ git init blog
$ cd blog
$ touch README.md
$ git add .
$ git commit -m 'Initial commit.'
$ git remote add origin git@github.com:toshibohjp/blog.git
$ git push -u origin master
```

以下のように blog アプリケーションを作成します。

```bash blogアプリケーションを作成する
$ cd ~/github
$ rails new blog
       exist
      create  README.rdoc
      create  Rakefile
      create  config.ru
      create  .gitignore
      create  Gemfile
      create  app
# （中略）
         run  bundle install
Fetching gem metadata from https://rubygems.org/...........
Fetching gem metadata from https://rubygems.org/..
Resolving dependencies...
Using rake (10.0.4)
# （中略）
Installing sass-rails (3.2.6)
Installing sqlite3 (1.3.7)
Installing uglifier (2.0.1)
Your bundle is complete!
Use `bundle show [gemname]` to see where a bundled gem is installed.
```
上記のような出力で Rails アプリケーションが作成されたので、ディレクトリの中身を覗いてみましょう。README.md 以外は `rails new blog` で作成されたファイルですが、たくさんありますね。何がなにやらよくわかりません。
```bash railsのフォルダ構成
$ cd blog
$ tree -L 2 -F
.
├── Gemfile
├── Gemfile.lock
├── README.md
├── README.rdoc
├── Rakefile
├── app/
│   ├── assets/
│   ├── controllers/
│   ├── helpers/
│   ├── mailers/
│   ├── models/
│   └── views/
├── config/
│   ├── application.rb
│   ├── boot.rb
│   ├── database.yml
│   ├── environment.rb
│   ├── environments/
│   ├── initializers/
│   ├── locales/
│   └── routes.rb
├── config.ru
├── db/
│   └── seeds.rb
├── doc/
│   └── README_FOR_APP
├── lib/
│   ├── assets/
│   └── tasks/
├── log/
├── public/
│   ├── 404.html
│   ├── 422.html
│   ├── 500.html
│   ├── favicon.ico
│   ├── index.html
│   └── robots.txt
├── script/
│   └── rails*
├── test/
│   ├── fixtures/
│   ├── functional/
│   ├── integration/
│   ├── performance/
│   ├── test_helper.rb
│   └── unit/
├── tmp/
│   └── cache/
└── vendor/
    ├── assets/
    └── plugins/

30 directories, 21 files
```
RailsGuides の説明によると、ファイルとフォルダ構成は以下のようになってます。

<table>
  <tr><th>ファイル／フォルダ名</th><th>目的</th></tr>
  <tr><td>app/</td><td>アプリケーションで使用する Controller, Model, View, Asset を含みます。このガイドの残りでは、このフォルダに焦点を当てます。
  </td></tr>
  <tr><td>config/</td><td>アプリケーションの実行時のルール、ルーティング、データベースなどを設定します。config/ に関しては Configureing Rails Applications で詳細を扱います。
  </td></tr>
  <tr><td>config.ru</td><td>Rack の設定です。Rack ベースのサーバーがアプリケーションを開始するのに使用します。
  </td></tr>
  <tr><td>db/</td><td>現在のデータベーススキーマとしてデータベースマイグレーションを含んでいます。
  </td></tr>
  <tr><td>doc/</td><td>アプリケーションの詳細なドキュメント</td></tr>
  <tr><td>Gemfile<br />Gemfile.lock</td><td>アプリケーションに必要な gem の依存関係を指定する事ができます。
  </td></tr>
  <tr><td>lib/</td><td>アプリケーションの拡張モジュール
  </td></tr>
  <tr><td>log/</td><td>アプリケーションログ</td></tr>
  <tr><td>public/</td><td>そのまま公開される唯一のフォルダです。静的なファイルと生成された Assets を含みます。</td>
  </tr>
  <tr><td>Rakefile</td><td>このファイルはコマンドラインから実行されるタスクを見つけて読み込みます。タスクの定義は Rails のコンポーネントを通して定義されます。Rakefile を変更するよりも、アプリケーションの lib/tasks ディレクトリにファイルを追加することで、タスクを追加するようにしてください。
  </td></tr>
  <tr><td>README.rdoc</td><td>アプリケーションの簡単な手順書です。アプリケーションが何をするかや、どのようにセットアップするかを、他の人に解るようにこのファイルを編集してください。
  </td></tr>
  <tr><td>script/</td><td>アプリケーションを起動する Rails のスクリプトを含みます。また、アプリケーションをデプロイ／実行するスクリプトを含めることもできます。
  </td></tr>
  <tr><td>test/</td><td>単体テスト、Fixture および 他のテストの仕組みです。これらについては Testing Rails Applications で扱います。
  </td></tr>
  <tr><td>tmp/</td><td>一時ファイル</td></tr>
  <tr><td>vendor/</td><td>サードパーティのコードを置くためのディレクトリです。典型的な Rails アプリケーションでは、Ruby Gems、 Rails のソースコード(プロジェクトに任意でインストールした場合)や追加パッケージ機能を含むプラグインをインクルードしています。
  </td></tr>
</table>
  
### 3.3 データベースを設定する
データベースの設定は <code>config/database.yml</code> にあります。なにも設定してなければデフォルトでは SQLite3 用にデータベースが設定されています。デフォルトで以下の3つの環境で Rails を実行する事ができます。

 - `development` : 手動でインタラクティブに実行するためのローカル開発用の環境
 - `test` : 自動テストを実行するための環境
 - `production` : アプリケーションをデプロイして公開するための環境

`rails new blog --database=mysql` でデータベースを指定することも可能ですし、手動で設定すべきではありません。

SQLiter3、MySQL、PostgreSQL、SQLiter3 + JRuby、MySQL + JRuby、PostgreSQL + JRuby の設定が記述されていましたが、今はデフォルトの SQLite3 で充分なため割愛します。

### 3.4 データベースを作成する
以下のコマンドで空のデータベースを作成します。
```bash
$ rake db:create
Could not find a JavaScript runtime. See https://github.com/sstephenson/execjs for a list of available runtimes.
/usr/local/rvm/gems/ruby-1.9.3-p392/gems/execjs-1.4.0/lib/execjs/runtimes.rb:51:in `autodetect'
# 以下スタックトレース
```

失敗しました。

JavaScript の実行環境がないと言っています。私もインストールした覚えはありません。上記のサイトを見てみるといろいろ実行エンジンがありますが、ここでは一番上にある therubyracer を使うことにします。

次のように Gemfile の末尾に以下を追加して、`bundle` を実行して therubyracer をダウンロードして、もう一度 `rake db::create` を実行します。

```bash
$ echo "gem 'therubyracer'" >> Gemfile
$ bundle
$ rake db:create
```

何も出力がありませんが、これでデータベースは作成されました。以下のファイルが追加されたはずです。

 - db/development.sqlite3
 - db/test.sqlite3

4 Hello, Rails!
---------------
### 4.1 Web サーバーを起動する
これで Rails を実行できる環境が整ったので、Rails を実行してみます。以下のような出力結果が得られます。

```bash
$ rails server
=> Booting WEBrick
=> Rails 3.2.12 application starting in development on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2013-05-02 15:26:50] INFO  WEBrick 1.3.1
[2013-05-02 15:26:50] INFO  ruby 1.9.3 (2013-02-22) [i686-linux]
[2013-05-02 15:26:50] INFO  WEBrick::HTTPServer#start: pid=8821 port=3000
```

WEBrick という Web サーバーが3000番ポートで起動してしたので、ブラウザでアクセスしてみます。

{% img /images/rails_guides/hello-rails.jpg 'Rails default page' 'Rails default page' %}

上手く表示されました。

Web サーバーを停止するには、起動したコンソールで<Control + C>を押します。また、development モードでは、サーバーがファイルの変更を自動的に検知するので、ファイルの変更の度に Rails を停止する必要はありません。

ちなみにこのページはスモークテストです。スモークテストとは一通り開発を終えた段階で簡易に起動確認を行う予備的なテストのことです。

切りがいいので、今回はこのあたりで[次回](../rorguide02-say-hello/)へ続きます。
