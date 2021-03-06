---
title: NILScriptの使い方と書き方
author: azu
layout: post
permalink: /2010/0816/res1888/
SBM_count:
  - '00042<>1355446114<>37<>0<>3<>2<>0'
dsq_thread_id:
  - 300802802
categories:
  - javascript
  - NILScript
  - software
tags:
  - javascript
  - twitter
  - Windows
  - エクスプローラ
  - クリップボード
  - プラグイン
---
[NILScript][1]とはSpiderMonkeyを使ったJavaScriptベースのスクリプト実行環境です。(Windows専用+Wine環境でもある程度)  
直感的に言うと、WSH + AutoHotKeyみたいな感じというのが分かりやすいかな。  
できることなど細かい事は[NILScript][1]のページやDocを読んでもらうとして、SpiderMonkey(JavaScript 1.8.1ベース)を使った処理系なのでFirefoxと同じJavaScriptの文法で、エクスプローラでのバッチ処理やキーバインドの変更などWSHやAutoHotKeyで行っていたような事が**JavaScript**で書けます。

### 使い方

[NILScript][1]のページからnil.zipをダウンロードして、適当な場所に解凍します。  
解凍して出てくるng.exe(コンソールあり)とngw.exe(コンソールなし)のどちらかに、NILScriptのスクリプトファイルである*.ngファイルを渡すと実行されます。(WSHだとng = Cscript ,ngw = Wscript みたいな感じ)  
毎回ng.exeなどにD&Dや引数などでngファイルを渡すのは面倒なので、ngファイルをクリックで実行できるように関連づけを行うinstall.batというのが入っています。実行するとng.exeと*.ngファイルが関連づけされ、クリックで実行できるようになります。  
(多分レジストリを使うのはここだけ)  
これで、実行する準備はできたので試しに何かスクリプトを実行してみましょう。

**追記:現在は[Home &#8211; NILScript &#8211; GitHub][2]にほとんどのスクリプト置いてあります**  
自分が作った適当なスクリプトで試してみる。

*   [displayInputKeys.ng][3]

[<img class="alignnone size-full wp-image-1889" title="ss-2010-08-16-1" src="http://efcl.info/wp-content/uploads/2010/08/ss-2010-08-16-1.png" alt="" width="153" height="78" />][4]

上のngスクリプトはちっちゃいウィンドウが出てきて押したキー(組み合わせも大丈夫)を表示するだけのスクリプトです。

*   [Twitter\_friend\_search.ng][5]

[<img class="alignnone size-medium wp-image-1890" title="ss-2010-08-16-2" src="http://efcl.info/wp-content/uploads/2010/08/ss-2010-08-16-2-179x300.png" alt="" width="121" height="203" />][6]

こっちのngスクリプトは、Twitterで自分がフォローしてる人からユーザー名を検索してコピーするスクリプトです。   
初回の起動時はOAuth認証します。(NILScriptで立てた簡易サーバをリダイレクト先にしている)  
2回目の起動で自分のフォローしてるユーザーのリストを取得してから、ユーザー名を検索するウィンドウを表示します。  
ユーザー名を途中まで入力して↓キーなどを押せば、入力したユーザー名の所まで飛べます。Ctrl+Cでユーザー名をコピーできます。

このようにngスクリプトを書けば、ローカルで動くものがJavaScriptで書けるので簡単なものを書いたりするのに便利です。  
特にエクスプローラなどのファイル操作周りはWSHより簡単に書けるので、簡易なバッチ処理を書きやすい。  
次はngスクリプトの書き方を見ていきます。

### 書き方

ドキュメントはダウンロードしたnilフォルダにdocというフォルダがあり、その中にユニット(機能)毎のドキュメントが書いてあります。  
いろいろあるので何から見れば良いか迷いますが、まずは_readme.txtを開いてみると、NILScriptには大きく分けて、  
Base.ng内で定義されているNIL標準クラス・関数 と require()関数で読み込んで使うユニットスクリプトがあることが分かります。  
簡単言うと、前者はngスクリプトにそのまま書いて使える関数などで、後者はrequire(&#8216;Clipboard&#8217;)のようにユニットスクリプトを読み込んでから使えるようになる関数などが存在するという事です。  
まずはNIL標準クラス・関数にどんなものがあるかをbase_*.txtなどを軽く見て把握しましょう。  
眺めているとprintlnなどのよくあるような感じのものからsleepやスレッドやポインタなどのJavaScriptでは扱えないものもあることが分かります。(base_main.txtはよく使う関数があるので必須)  
正直、まだドキュメントは箇条書き程度で文例などが不足しているので、実際に動かしながら確認する必要がある。  
(<a href="http://nilscript.blogspot.com/" target="_blank">NILScriptブログ</a>で使い方などの解説も始まったので合わせて読む)  
必要な機能は必要になったらgrepなどで検索して探すので効率的。

ドキュメントばかり見てもよく分からないので、サンプルファイルを見ながらどういう風に書いているのかを見てみましょう。  
サンプルはnilフォルダのsampleというフォルダに入っています。  
NIL標準クラス・関数はそのまま使えるので、ドキュメントや動作を見れば何となく分かってくると思います。  
ユニットスクリプトを使うものはrequireしてから使う必要があるので、requireの仕方を見てみましょう。  
例えばsampleのclipboard.ngを見ると、clipboardを扱うためのユニットスクリプトを読み込んでいるのが分かります。(詳細はClipboard.txtに)

<pre class="brush:javascript;">/*
タスクトレイにアイコンを登録して常駐し、
クリップボードの内容が変更された時に、形式に応じた情報を表示します。
*/

Main.createNotifyIcon();// タスクバーにアイコン表示
var Clipboard=require('Clipboard').Clipboard;// ユニットスクリプトを読み込む

Clipboard.observe('change',function(obj){// クリップボードが変化したら動作するリスナー
	if(this.hasFormat('files')){// クリップボードの中身がファイル形式
		var a=this.files,r=&#91;&#93;;
		for(var i=0,l=a.length;i&#60;l;i++){
			r.push(a&#91;i&#93;.name);
		}
		Main.notifyIcon.showInfo(r.join("n"),this.isMove?"move":"copy");// タスクトレイのアイコンにバルーンメッセージを表示する
	}else if(this.hasFormat('html')){// html形式
		var h=this.html;
		Main.notifyIcon.showInfo(h.fragment,h.url);
	}else if(this.hasFormat('text')){// テキスト形式
		Main.notifyIcon.showInfo(this.text,'text');
	}
});</pre>

上ではvar Clipboard=require(&#8216;Clipboard&#8217;).Clipboard;というようにクリップボードのユニットスクリプトを読み込んで、  
読み込んだクリップボードスクリプトのClipboardというクラスをClipboardという変数に渡しています。  
_readme.txtの●ユニットスクリプト機能にも書いてありますが、ユニットスクリプトrequire(&#8220;SomeUnit&#8221;).SomeClass という感じで、  
多くのものはSomeUnitとSomeClassは同じ名前を持っているので上のような読み込み方法がよく出てきます。  
他にもユニットスクリプトの使い方はあるので詳しくは[NILScript: ユニット(拡張ライブラリスクリプト)の読み込み方法][7]を見ると良いでしょう。(withでやるやり方はWindowでよく使います)

まずはHelloWorldを書いて実行するところから始めて、クリップボードが使いたくなったら関係するサンプルとClipboard.txtを見て作ってみる(それでもよく分からないときはlibフォルダにユニットスクリプトのソースがあるので中身を見てしまうと言うのもあります)

<pre class="brush:javascript;">println("Hellow World");</pre>

サンプルを見ていくと大体雰囲気は使えめると思いますが、まだまだ文例が足りないのでハマる事があります。  
くだらないものを書いてもどんどん公開して、いろんな書き方が見られるようになるともっと使いやすいものになると思います！

### 作例

**追記:現在は[Home &#8211; NILScript &#8211; GitHub][2]にほとんどのスクリプト置いてあります**

というわけで自分が今までに自分が書いたNILScript。

*   [gist: 401178 &#8211; NILScript用のスクリプト- GitHub][8]  
    コピーする深さを決められるtreeコマンドみたいなもの。  
    一番最初に書いたものなので何かおかしいところがあったり。(フォルダ判定はsizeより、dir.attributes.directoryがフォルダならtrueを返してくれるのでそっちを使いましょう)
*   [gist: 425452 &#8211; 30分ごとにLDRの未読数を表示するNilScript- GitHub][9]  
    アイコンとタイマーとHTTP.getText
*   [gist: 526275 &#8211; 押したキーを表示するNILScript- GitHub][10]  
    上で紹介していたもの。  
    ウィンドウの表示方法とKeyboard。
*   [gist: 453042 &#8211; クリップボードのJavaScriptコードを整形してクリップボードに返すNILScript &#8211; GitHub][11]  
    JavaScriptで実装されたJS Beautifierをそのまま使えるって話
*   [gist: 453264 &#8211; プロセスを起動して落ちるのを待つだけ- GitHub][12]  
    サンプルにも似たようなものがあるのでそっちの方が参考になる  
    exeの起動方法
*   [gist: 526351 &#8211; プロセスの起動 &#8211; GitHub][13]  
    上と殆ど同じ。プロセスのフルパスはエスケープする必要がある。
*   [gist: 454863 &#8211; NILScriptでXMLHttpRequest的なものを書くテスト- GitHub][14]  
    GM_xmlhttpRequestてきな感覚でXHRを書くのをテスト  
    同じインスタンスのリクエストは上から順に実行される?
*   [gist: 456001 &#8211; ファイルの作成して書き込み #NILScript- GitHub][15]  
    そのまま。
*   [gist: 467939 &#8211; #NILScript にデフォルト値を入れたものを新規作成- GitHub][16]  
    上のを土台にした作ったもの。  
    Greasemonkeyの新規作成ダイアログみたいなもので、// ==NILScript==のようなメタ情報を書いたngファイルを生成する。  
    AutoHotKeyでもそうだったけど、スクリプト単体で配布されることが多いので、後でこれどこで拾ったけ?って事があると思うので、メタ情報はできるだけ入れておいた方がよいと思う。
*   [gist: 463430 &#8211; D&Dしたディレクトリ以下をフラットにする #NILScript- GitHub][17]  
    フォルダ(1)内のフォルダ(2)の中身をフォルダ(1)に移動させて、フォルダ(2)を削除</p> 
    <pre>フォルダ
    ├fileA
    ├fileB
    └フォルダ&#91;DIR&#93;
     ├fileC
     └fileD
が
    フォルダ
    ├fileA
    ├fileB
    ├fileC
    └fileD</pre>
    
    のようにするスクリプト。複数のフォルダを同時にD&Dしてもできるようにしている。  
    treeコマンドの時よりフォルダの扱いがちゃんとしてるはず。

*   [gist: 485691 &#8211; #NILScript からTwitterへ投稿- GitHub][18]  
    Greasemonkey向けにOAuthラッパを書いてあったので、NILScriptでも動くようにしてみたサンプル。  
    NILscriptでのOAuthについては後ろの方でまた書くのでこれは見る程度で。  
    [azu&#8217;s OAuth-for-Greasemonkey at master &#8211; GitHub][19]
*   [gist: 487432 &#8211; 自分のフォローしてる人からユーザー(スクリーン)名を検索する #NILScript- GitHub][20]  
    これもOAuth使ったサンプル。  
    OAuth.jsとかも一緒くたにしてるので、分離する方法はまた後ろ

<br class="spacer_" />

最後の方で出てきた**OAuthをNILScriptで使う方法**。(上のサンプルは無視していいよ)

*   [OAuth.ng][21] (ライブラリみたいなものです)

をダウンロードして、OAuthを使うスクリプト内でinclude(&#8220;OAuth.ng&#8221;);して、後は[OAuth for Greasemonkeyのラッパーライブラリ | Web scratch][22]とメソッドの使い方は同じ。(xhrは実装してないです)  
単純なサンプルで、OAuth.ngと同じフォルダに入れて実行するとOAuth認証ができます。

*   [gist: 526378 &#8211; OAuth for NILScriptのサンプル- GitHub][23]

本当はユニットスクリプトやプラグインにした方が良いんだけどやり方がイマイチ分からなかったのでincludeにしてみた。

### まとめ

良いところ

SpiderMonkeyベースなのでE4Xなども使えて文字列処理がやりやすいので、Firebugなどでマクロ的な文字列処理をしてる人とかJavaScriptで書くならこっちの方が便利かもよ。(JavaScript の新しめの機能(Array の拡張)とかも動くよ)  
デフォルトで[マルチストローク対応ホットキー][24]やマウスジェスチャープラグインなどもあるので、AutoHotKeyでよくあるキーバインドを変更するとかもできるよ(自分の設定を公開とかやるといいかも)  
WSHに比べてファイル操作をするものがかなり書きやすいよ。(base_io.txtを読む)  
AutoHotKeyと違ってマルチバイト文字列に対応してるのでダメ文字とか日本語書けないとかに悩まされないよ。  
標準関数やユニットスクリプトなどもNILScriptで書かれているので、ソースを直接読むことができる(この辺、情報不足感といいJetpackによく似てる。requireするとこも)

悪いところ

知名度が少ないので情報が少ない(→作ったものはどんどん公開して欲しい)  
SpiderMonkeyにはDOM実装はないので、LooseXMLという独自のパーサーになっている。XPathに似た表記で使えたり悪いとは言い切れないけど、普段使うJavaScriptのDOMとはちょっと違うので手間取る。  
GUIの表現が貧弱。Windowを使う事で基本的なウィンドウは簡単に作れるが、文字サイズなど装飾など細かい事ができない部分も多い。

NILScriptの用途として個人的に使うものを書く場合が多いと思いますが、書いたものをどんどん公開していこうぜ！という感じで〆

[**NILScript**][1]
:   NILScript配布場所

<a href="http://nilscript.blogspot.com/" target="_blank">NILScriptブログ</a>
:   特徴や使用例の紹介記事

 [1]: http://lukewarm.s151.xrea.com/nilscript.html
 [2]: https://github.com/azu/NILScript/wiki "Home - NILScript - GitHub"
 [3]: http://gist.github.com/raw/526275/67c3dd182b194beee8c48ceac80a00c0bc5d834d/displayInputKeys.ng
 [4]: http://efcl.info/wp-content/uploads/2010/08/ss-2010-08-16-1.png
 [5]: http://gist.github.com/raw/487432/8adefe14ee39f7bb54130c3c4ab55dda731ca011/Twitter_friend_search.ng
 [6]: http://efcl.info/wp-content/uploads/2010/08/ss-2010-08-16-2.png
 [7]: http://nilscript.blogspot.com/2010/08/blog-post_2026.html
 [8]: http://gist.github.com/401178
 [9]: http://gist.github.com/425452
 [10]: http://gist.github.com/526275
 [11]: http://gist.github.com/453042
 [12]: http://gist.github.com/453264
 [13]: http://gist.github.com/526351
 [14]: http://gist.github.com/454863
 [15]: http://gist.github.com/456001
 [16]: http://gist.github.com/467939
 [17]: http://gist.github.com/463430
 [18]: http://gist.github.com/485691
 [19]: http://github.com/azu/OAuth-for-Greasemonkey
 [20]: http://gist.github.com/487432
 [21]: http://gist.github.com/raw/526379/fa17c5da823169bd8d4c6f4f6159ae2cf277b85e/OAuth.ng
 [22]: http://efcl.info/2010/0610/res1721/
 [23]: http://gist.github.com/526378
 [24]: http://nilscript.blogspot.com/2010/08/hotstrokes.html