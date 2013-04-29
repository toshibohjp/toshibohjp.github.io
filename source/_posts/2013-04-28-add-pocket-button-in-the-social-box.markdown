---
layout: post
title: "Octopress に「Pocket」に追加するボタンをつける"
date: 2013-04-28 12:15
comments: true
sharing: true
categories: [octopress, pocket]
---
私は [Pocket](http://getpocket.com/) をかなり使い倒してます。何か興味深いページがあれば、「Pocketに追加する」で Pocket に保存しておき、後で読み返しながら興味深ければ Twitter でツイートして、後で使いそうな知識であれば Evernote に保存しておきタグで整理しています。

さて、役に立つかどうかはわかりませんが、私の Github pages にも「Pocket に追加する」ようなボタンを追加したいと思います。

<!-- more -->

Pocket ボタンのコードを取得する
---------------------------------
[Pocket for Publishers: Pocket Button](http://getpocket.com/publisher/button)にアクセスして、Pocket ボタンのコードを取得します。スタイルはお好みで選んでください。

_config.yml に設定を追加する
----------------------------
せっかくなので、設定で Pocket ボタンをオン／オフできるようにしてみます。

_config.yml の末尾に `pocket_putton` という属性を追加します。`git diff` を取ったものが以下のコードです。

```diff _config.yml
@@ -93,3 +93,6 @@ 

 # Facebook Like
 facebook_like: true
+
+# Pocket
+pocket_button: true
```

コピー＆ペースト用のコードも添付しておきます。

```yaml _config.yml
# Pocket
pocket_button: true
```

shareing.html を修正する
------------------------
source/_includes/post/sharing.html に先ほどのコードを貼り付けます。加えて、先ほど _config.yml に設定した属性でボタンの表示をするかどうかのテストを行うように '{% if site.pocket_button %}` ... `{% endif %} で囲みます。

Pocket はなるべく早く押せたほうがいいので、ソーシャルボタンの先頭になるようにします。貼り付けたままだとインデントが気になるので、私は `Vim` 上から `:setlocal expandtab ts=2 sts=2 sw=2` とコマンドを入力して、`g=G` でインデントを修正しました。

横幅が長くなりますが `git diff` を取った結果は以下の通りになります。

{% raw %}
```diff source/_includes/post/sharing.html
@@ -1,4 +1,8 @@
 <div class="sharing">
+  {% if site.pocket_button %}
+  <a data-pocket-label="pocket" data-pocket-count="vertical" class="pocket-btn" data-lang="en"></a>
+  <script type="text/javascript">!function(d,i){if(!d.getElementById(i)){var j=d.createElement("script");j.id=i;j.src="https://widgets.getpocket.com/v1/j/btn.js?v=1";var w=d.getElementById(i);d.body.appendChild(j);}}(document,"pocket-btn-js");</script>
+  {% endif %}
   {% if site.twitter_tweet_button %}
   <a href="http://twitter.com/share" class="twitter-share-button" data-url="{{ site.url }}{{ page.url }}" data-via="{{ site.twitter_user }}" data-counturl="{{ site.url }}{{ page.url }}" >Tweet</a>
   {% endif %}
```
{% endraw %}

コピー＆ペースト用のコードも添付しておきます。

{% raw %}
```html source/_includes/post/sharing.html
  {% if site.pocket_button %}
  <a data-pocket-label="pocket" data-pocket-count="vertical" class="pocket-btn" data-lang="en"></a>
  <script type="text/javascript">!function(d,i){if(!d.getElementById(i)){var j=d.createElement("script");j.id=i;j.src="https://widgets.getpocket.com/v1/j/btn.js?v=1";var w=d.getElementById(i);d.body.appendChild(j);}}(document,"pocket-btn-js");</script>
  {% endif %}
```
{% endraw %}

参考サイト
----------
- [5分でできる簡単 Octopress セッティング - 酒と泪とRubyとRails](http://morizyun.github.io/blog/octopress-hatena-disqus-new-tab/)

