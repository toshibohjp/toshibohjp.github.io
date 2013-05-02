---
layout: post
title: "[RailsGuides] #02 Rails で Hello を実行する"
date: 2013-05-02 18:11
comments: true
shareing: true
published: true
categories: [Ruby on Rails, tutorials]
---
[前回](../rorguide01-creating-a-new-rails-project/) に引き続いて [RailsGuides](http://guides.rubyonrails.org/getting_started.html) の日本語の抜粋などを記述していきます。前回と同様に見出しは RailsGuides に準じます。

<!-- more -->

{:TOC:}

### 4.2 Rails で "Hello" と言う
Rails で "Hello" というために、最低限の Controller と View を作成します。以下のコマンドを実行します。

```bash
$ rails generate controller home index
      create  app/controllers/home_controller.rb
       route  get "home/index"
      invoke  erb
      create    app/views/home
      create    app/views/home/index.html.erb
      invoke  test_unit
      create    test/functional/home_controller_test.rb
      invoke  helper
      create    app/helpers/home_helper.rb
      invoke    test_unit
      create      test/unit/helpers/home_helper_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/home.js.coffee
      invoke    scss
      create      app/assets/stylesheets/home.css.scss
```
出力を見ると Controller と View に加えて、単体テスト、ヘルパーモジュール、CoffeScript、CSS のテンプレートが作成されていることに気づくでしょう。

次に、好きなエディタで `app/views/home/index.html.erb` を編集します。私の場合は `vim app/views/home/index.html.erb`です。

```erb app/views/home/index.html.erb
<h1>Hello, Rails</h1>
```

### 4.3 アプリケーションのホームページを設定する
Controller と View を作成しましたが、"Hello Rails!" を表示させたい時に Rails に指示する必要があります。この場合では、"Welcom Aboard" スモークテスト の変わりに URL のルート(例えば、http://localhost:3000 のように)に表示させたいと思います。

まず最初に、デフォルトページを削除します。私の場合は `git rm public/index.html` で Git から削除しましたが、Git を使わない場合は `rm public/index.html` です。
```bash
git rm public/index.html
```

次に Rails に実際のホームページがどこかを指示する必要があります。 `config/routes.rb` をエディタで開いて編集します。以下に編集箇所を diff で示します。
```diff config/routes.rb
@@ -48,7 +50,7 @@ Blog::Application.routes.draw do

   # You can have the root of your site routed with "root"
   # just remember to delete public/index.html.
-  # root :to => 'welcome#index'
+  root :to => 'home#index'

   # See how all your routes lay out with "rake routes"

```

Rails を起動してない場合は `rails server` で WEBrick を起動して、ブラウザでルート (http://localhost:3000/ など) で接続確認します。

{% img /images/rails_guides/say-hello.jpg 'Hello, Rails' 'Hello, Rails' %}

`config/routes.rb` は特殊な DSL として、入ってきたリクエストを Controller や Action にどのように接続するかを指示するエントリをアプリケーションのルーティングファイルとして定義します。先ほど編集した `root :to => "home#index" は、ルートアクションを home コントローラーの index アクションへマップするように指示しています。

ルーティングについてのより詳しい情報については [Rails Routing from the Outside in](http://guides.rubyonrails.org/routing.html) を参照してください。

次回に続きます。
