---
title: Github上(のみ)で自分自身にpull requestを送る方法
author: azu
layout: post
permalink: /2014/0305/res3702/
dsq_thread_id:
  - 2365071533
categories:
  - webサービス
tags:
  - github
---
Githubでpull requestを送る場合はブランチを切ってローカルで編集してpushして、pull requestを作る感じになると思います。

Githubではオンラインでそのままファイルを編集できるので、ドキュメントのようなものは直接オンラインで作成してpull requestをしたいことがあると思います。

自分以外のリポジトリにpull requestを送る場合はEditから編集するだけで自動的にforkから編集、pull requestまでできるので簡単ですが、自分以外のリポジトリに対してそれをやる場合は少し異なる手順が必要です。

この記事はその手順を解説します。

### 自分自身のリポジトリにpull request

#### 1 まずは、新たにブランチを作成します。

<img src="http://efcl.info/wp-content/uploads/2014/03/quickcast-compressed.mp4.gif" alt="Quickcast compressed mp4" title="quickcast-compressed.mp4.gif" border="0" width="600" height="400" />

*   [Create and delete branches][1]

#### 2 つぎに、pull requestで送るファイルを作成 or 編集します

*   [Creating files on GitHub][2]

#### 3 最後に、pull requestボタンから自分自身へpull requestを送ります

<img src="http://efcl.info/wp-content/uploads/2014/03/Promises-book-at-outline-2014-03-05-23-03-40-2014-03-05-23-03-43.jpg" alt="Promises book at outline 2014 03 05 23 03 40 2014 03 05 23 03 43" title="Promises-book at outline 2014-03-05 23-03-40 2014-03-05 23-03-43.jpg" border="0" width="600" height="223" />

<img src="http://efcl.info/wp-content/uploads/2014/03/Promises-book-2014-03-05-23-10-24-2014-03-05-23-10-26.jpg" alt="Promises book 2014 03 05 23 10 24 2014 03 05 23 10 26" title="Promises-book 2014-03-05 23-10-24 2014-03-05 23-10-26.jpg" border="0" width="600" height="396" />

やや面倒臭いですが、このような手順を行う事でブラウザのみで自分自身へのpull requestを送る事が出来ます。

*   [[WIP] この書籍で扱う内容について by azu · Pull Request #1 · azu/Promises-book][3]

自分の場合は、議論しながら書くような感じの内容については、この方法を使って自分自身にpull requestのissueを立ててやってる事が多いです。  
(仮定を残したいだけなので、マージしないで捨てたりもしますが)

 [1]: https://github.com/blog/1377-create-and-delete-branches "Create and delete branches"
 [2]: https://github.com/blog/1327-creating-files-on-github "Creating files on GitHub"
 [3]: https://github.com/azu/Promises-book/pull/1 "[WIP] この書籍で扱う内容について by azu · Pull Request #1 · azu/Promises-book"