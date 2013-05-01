---
layout: post
title: "Linxu 上の HTTPS ポートで SSH を使う"
date: 2013-05-02 05:28
comments: true
shareing: true
categories: [github, ssh]
---

私の仕事場では、社内にプロキシサーバが設置されており、通常の SSH ポート(22番)が外部サーバに対して使用できません。git みたいなオシャレなプロトコルには対応していないのです。しかしそれでも、GitHub に接続する必要にかられることがあります。

ここでは、HTTP ポート(80番) と HTTPS ポート(443番) はどのプロキシでも接続できるので、443番ポートを使用して SSH で GitHub に接続する方法をご紹介します。

<!-- more -->

{:TOC}

環境変数を設定する
------------------

まずは必要な環境変数を設定します。
以下のような情報でプロキシに接続するとします。

 項目                   | 値                                
 -----------------------|-----------------------------------
 ユーザ名               | user                               
 パスワード             | passwd                            
 プロキシサーバ         | your.proxy-server.com             
 ポート番号             | 8080                              
 プロキシから除外       | localhost,127.0.0.1,192.168.0.0/24


以下のスクリプトを .bashrc に記述します。また私は CentOS を使用しているので、yum のための環境変数もここで設定しています。さらに私は個人用途のサーバで別のユーザを作成したりする開発環境なので、 /etc/profile.d/proxy.sh というファイルを作って、そこに突っ込んでいますが、オススメはしません。
```bash
#!/bin/bash

export http_proxy=http://user:passwd@your.proxy-server.com:8080
export https_proxy=$http_proxy
export http_proxy_user=user
export http_proxy_pass=pass
export https_proxy_user=$http_proxy_user
export https_proxy_pass=$http_proxy_pass
export no_proxy="localhost,127.0.0.1,192.168.0.0/24"
# yum
proxy=http://your-server.com:8080
proxy_username=$http_proxy_user
proxy_password=$http_proxy_pass
```

connet のインストール
---------------------
プロキシを中継するように connect をインストールします。

```bash connectのインストール
# apt の場合
$ aptitude install connect-proxy
# yum の場合
$ yum install connect
# cygwin の場合
$ apt-cyg install connect-proxy
```

パッケージ管理ツールが使えない場合は、以下のコマンドで自力でビルドします。
```bash
$ wget http://www.meadowy.org/~gotoh/ssh/connect.c
$ gcc -O2 -o connect connect.c
$ mv connect /usr/local/bin/
```

.ssh/config を設定する
----------------------
.ssh/config に以下の設定を追加します。`IdentityFile` には GitHub で使用する秘密鍵を `~/.ssh/id_rsa`  のように、`プロキシサーバ名：ポート番号` にはプロキシサーバ名とポート番号を、`proxy.your-domain.com:8080` のように編集します。

```
Host github.com
  User git
  Port 443
  Hostname ssh.github.com
  IdentityFile 秘密鍵のファイル名
  TCPKeepAlive yes
  IdentitiesOnly yes
  ProxyCommand connect -H プロキシサーバ名:ポート番号 %h %p
```

ここでは、SSH を使用する際に443番ポートを使用し connect でプロキシを中継して GitHub と通信するようにしています。

さらに、新規に .ssh/config を追加した場合は、アクセス権を以下のように設定しないと接続に失敗します。Unix に慣れている方には常識かも知れませんが、私は最初にアクセス権を設定をグループに読み書き権限が付与されており、接続エラーで結構ハマりました。

```bash
$ chmod 600 ~/.ssh/config
```

接続を確認する
--------------
SSH で GitHub に接続できるか確認します。以下のようにコマンドを実行し、以下のような出力結果になれば接続は成功です。

```bash
$ ssh git@github.com
Enter proxy authentication password for ユーザ名@proxy.your-domain.com:
Enter passphrase for key '/home/ユーザ名/.ssh/id_rsa':
PTY allocation request failed on channel 0
Hi ユーザ名! You've successfully authenticated, but GitHub does not provide shell access.
                Connection to ssh.github.com closed.
```

毎回 ssh のパスフレーズを入力するのを省略したい場合は、以下のコマンドを実行すると、それ以降のパスフレーズの入力を省略することができます。 `~/.ssh/id_rsa` はご自分の秘密鍵のファイル名に読み替えて入力してください。
```bash
$ ssh-agent bash
$ ssh-add ~/.ssh/id_rsa
```

これで以下のコマンドを実行すれば、443番ポート経由で SSH のアクセスが可能になってる思います。

試し octocat/Spoon-Knife リポジトリからクローンしてください。
```bash
$ git clone git@github.com:octocat/Spoon-Knife.git
```

git:// プロトコルにも対応する
-----------------------------
冒頭でプロキシが git プロトコルに対応していないと書きましたが、調べているうちに、git プロトコルに対応できることがわかりました。

まず、以下のようなスクリプトを、例えば $HOME ディレクトリに記述します。ファイル名は git-proxy.sh とします。
```bash ~/git-proxy.sh
#!/bin/sh
connect -H proxy.your-server.com:8080 $1 $2
```

記述したスクリプトのアクセス権を設定します。
```bash
$ chmod 755 ~/git-proxy.h
```

次に以下の設定を .bashrc に記述します。
```bash
export GIT_PROXY_COMMAND=~/git-proxy.sh
```

`source .bashrc` を忘れずに実行して、以下のようにコマンドを実行して接続を確認します。
```bash
git clone git://github.com/octocat/Spoon-Knife.git
```

参考
----
- [HTTPプロキシ経由でのSSH（その２）connectを使う](http://d.hatena.ne.jp/takuya_1st/20110813/1313223707)
- [プロキシ経由でgithubにpull&pushする](http://nobeans.hatenablog.com/entry/20090520/1242835619)
