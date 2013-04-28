---
layout: post
title: "Octopress のヘッダを日本語に対応させる"
date: 2013-04-28 09:56
comments: true
sharing: true
categories: [octopress, localize] 
---
Octopress はデフォルトのままだと、タイトルやヘッダに日本語を使用しても正しく日本語が表示されません。

どうやら Octopress はデフォルトの設定で以下のように Web フォントを使用してことが原因のようです。

```bash
$ cd octopress
```

```html source/_includes/custom/head.html
<!--Fonts from Google"s Web font directory at http://google.com/webfonts -->
<link href="http://fonts.googleapis.com/css?family=PT+Serif:regular,italic,bold,bolditalic" rel="stylesheet" type="text/css">
<link href="http://fonts.googleapis.com/css?family=PT+Sans:regular,italic,bold,bolditalic" rel="stylesheet" type="text/css">
```
これを非常に簡単な設定で上書きします。

### _fonts.scss に日本語フォントを設定する
以下のように $heading-font-family に日本語フォントを追加することで、ヘッダに日本語を表示することができます。

```diff sass/custom/_fonts.scss
@@ -5,6 +5,6 @@
 //$sans: "Optima", sans-serif;
 //$serif: "Baskerville", serif;
 //$mono: "Courier", monospace;
-//$heading-font-family: "Verdana", sans-serif;
+$heading-font-family: "ヒラギノ角ゴ", "Hiragino Kaku Gothic Pro", "メイリオ", Meiryo, "ＭＳ Ｐゴシック","MS PGothic", "Verdana", sans-serif;
 //$header-title-font-family: "Futura", sans-serif;
 //$header-subtitle-font-family: "Futura", sans-serif;```
```

自分の環境は Windows Vista + Chrome なのですが、Mac や Windows XP の方々にも配慮して、"ヒラギノ角ゴシック"と"ＭＳ　Ｐゴシック"の設定を追加しています。

お好みで他のフォントに変更しても良いかもしれませんね。

HTML の lang 属性を ja に設定する
---------------------------------
あと、デフォルトの言語が英語になっており、Google翻訳などが起動したりと面倒なことになるので、HTML の言語を日本語に変更しました。

```diff source/_includes/head.html
@@ -1,7 +1,7 @@
 <!DOCTYPE html>
 <!--[if IEMobile 7 ]><html class="no-js iem7"><![endif]-->
 <!--[if lt IE 9]><html class="no-js lte-ie8"><![endif]-->
-<!--[if (gt IE 8)|(gt IEMobile 7)|!(IEMobile)|!(IE)]><!--><html class="no-js" lang="en"><!--<![endif]-->
+<!--[if (gt IE 8)|(gt IEMobile 7)|!(IEMobile)|!(IE)]><!--><html class="no-js" lang="ja"><!--<![endif]-->
 <head>
   <meta charset="utf-8">
   <title>{% if page.title %}{{ page.title }} - {% endif %}{{ site.title }}</title>
```

