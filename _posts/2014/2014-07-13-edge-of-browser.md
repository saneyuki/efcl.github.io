---
title: ブラウザエンジン先端観測会 アウトラインメモ
author: azu
layout: post
categories:
    - イベント
tags:
    - イベント
    - Browser
    - JavaScript
    - C++
    - CSS
    - JIT
    - Gecko
    - Firefox

---

# ブラウザエンジン先端観測会

[ブラウザエンジン先端観測会 : ATND](http://atnd.org/events/52121 "ブラウザエンジン先端観測会 : ATND") に参加してきたのでメモ。

途中から参加なのでServoの話は途中からです。

Twitterのログはこちら

* [ブラウザエンジン先端観測会 #1 - Togetterまとめ](http://togetter.com/li/692648 "ブラウザエンジン先端観測会 #1 - Togetterまとめ")

---

## Servo (by Tetsuharu OHZEKI a.k.a @saneyuki_s)

- スライド: [Servo parallelism](http://www.slideshare.net/saneyuki/servo-parallelism "Servo parallelism")

- ACID2は通った
- Servoのレイアウトの計算
    - ボトムアップの場合
         - 子供のノードは並列に計算する
         - 全ての計算が終わった時に親に行く
    - トップダウン
         - もっとシンプルに親からやっていく
    - floatの話
         - floatの影響を受けるものはフラグを立てる
         - フラグが立ったものは遅延的に計算する
         - floatの計算終了後に再度計算する
         - = assign height
         - 遅延的にやるのは、同計算していいのか分からないため
    - overflow
         - Block format contextのこと
         - assign-widthの時に仮定を指定widthをやっていく
         - 大体でやっていく、Blockとしてやれると仮定
- DOMのライフサイクル管理の一本化
    - 過去の事例 - DOM.js
         - DOM.jsというJavaScriptでDOMかいていいじゃね => メモリバカ食い遅い => 失敗
    - 現在 - SpiderMonkeyのGCに任せて管理
    - GPUでのセレクタマッチング
    - Intelの一時メモキャッシュ早いということがわかった
- まとめ
    - ACID2は達成
    - 大体アーキテクチャできた
    - Samsungの人がどっかいった
         - 以前はTableレイアウトとか頑張ってた
    - NO PLAN
         - 何も考えずにつくってる

---

## アクセシビリティオブジェクトが作られない場合（by @Takenspc）

- スライド: [アクセシビリティオブジェクトについて](http://www.slideshare.net/takenspc/20140713-accessibility-object "アクセシビリティオブジェクトについて")

- ブラウザのHTML5対応コンテキスト
- アクセシビリティAPIについて
- アクセシビリティオブジェクトとは何か
    - Access + Ability = アビリティ , A11Y
- ウェブへのアクセス
    - 画面をみれなかったり
    - マウス、キーボードじゃなくてタッチスクリーンだったり
    - 音声入力だったり
    - ウェブへのアクセス方法も色々ある
- 支援技術
    - Assistive Technology
    - 特定の技術を使いやすくする
    - そのままで行えないものを出来るようするための製品、ツール、デバイス
- 支援技術のデモ
    - 「オープンオペラ」 in Mozillaオフィス
    - ブラウザが対応してるから音声入力で操作できる
- アクセシビリティAPIの特徴
    - アクセシビリティAPiの語彙、セマンティックはHTMLとは違う
    - ブラウザ専用じゃないので
    - ブラウザがコンテンツをアクセシビリティオブジェクトに変換して利用する
- ブラウザはコンテンツを支援技術に伝える
    - コンテンツをアクセシビリティオブジェクトに変換する
- アクセシビリティオブジェクト
    - 役割  : 種類(role) ≒ HTMLの要素
    - 状態 : 動的な属性 - チェックされてるとか、ボタンが押されてるとか
    - プロパティ : 静的な属性
    - その他
         - 位置や大きさ、関連要素、文字の大きさ色
- アクセシビリティオブジェクトを作るには
    - DOMツリー + レンダリングツリー => アクセシビリティツリー
    - アクセシビリティツリーをプラットフォーム別のアクセシビリティツリーに変換
    - 変換したアクセシビリティツリーを支援技術に伝える
- (Gecko)DOMからアクセシビリティオブジェクトの作成
    - 非表示だったらオブジェクトは作らない
    - WAI-ARIAを見てやる
    - HTML要素だったら型や属性を見る
    - それでも出来ない場合はFrameの種類(レンダーオブジェクトから)からオブジェクトを作る
- Webkit/Chromeのアクセシビリティオブジェクト
    - レンダーツリーをイテレート
    - レンダーツリーからやっていくのが基本的な流れ
    - CanvasはDOMをみる
    - WebCoreとBlinkは色々特殊
         - ノードのWAI-ARIAや種類を見る
         - ノードがWAI-ARIAのグリッドのケースの処理
         - roleの決定が複雑
    - Roleの決定
         - WAI-ARIAを見る
         - レンダーオブジェクトのboxの種類を見て判定するコードを並ぶ
         - レンダーツリーの隣接レンダーオブジェクトを見ていく
- HTML5対応とは何か
    - HTML5には色々な側面がある
    - [HTML to Platform Accessibility APIs Implementation Guide](http://www.w3.org/TR/html-aapi/ "HTML to Platform Accessibility APIs Implementation Guide") 実装ガイド
    - [HTML5 accessibility - work in progress March 2014](http://html5accessibility.com/ "HTML5 accessibility - work in progress March 2014") 実装状況
- アクセシビリティオブジェクトの調査
    - どうやってブラウザが対応してるのかをチェックするか
    - チェックツール
         - プラットフォームのアクセシビリティツリーをチェックするツール(プラットフォーム固有)
              - [aViewer 2013 | The Paciello Group Blog](http://blog.paciellogroup.com/2013/03/aviewer-2013/ "aViewer 2013 | The Paciello Group Blog")
         - 内部的なアクセシビリティツリーをチェックするツール(ブラウザが持ってる)
              - DOM Inspector
    - 期待と違う結果
         - そもそも対応してないケース
         - レンダーツリーが影響してる場合
         - HTML5と一対一対応ではない => フラット化してる
- フラット化
    - アクセシビリティオブジェクトはツリー構造
    - 重要と判断したノードに対してだけ見せる(=ユーザーに伝える)
    - 重要かどうかはブラウザが判断する
    - HTML要素と一対一対応だと意味ないネストが発生する(装飾目的)
    - そのためフラット化してシンプルにしていく
- WebKitのバグ
    - 支援技術に見せるかどうかでの判定があまい
    - 見せるべきものを伝えない場合がある
- まとめ
    - ブラウザにはHTMLに対応したアクセシビリティツリーを作る責務
    - HTML5対応にはアクセシビリティAPIの側面を含めて考える必要がある

---

## CSS JIT (by @Constellation)

- スライド : [CSS JIT: Optimizing CSS Selector Matching with Just-in-Time Compilation // Speaker Deck](https://speakerdeck.com/constellation/css-jit-optimizing-css-selector-matching-with-just-in-time-compilation "CSS JIT: Optimizing CSS Selector Matching with Just-in-Time Compilation // Speaker Deck")
- 関連記事 : [Surfin&#39; Safari - Blog Archive » Little overview of WebKit’s CSS JIT Compiler](https://www.webkit.org/blog/3271/webkit-css-selector-jit-compiler/ "Surfin&#39; Safari - Blog Archive » Little overview of WebKit’s CSS JIT Compiler")

- WebKitコミッター
- モチベーション
    - CSSセレクタはものすごい回数が実行される
    - この要素にはこのスタイルを当てるというだけでも多い
    - QuerySelectorでも大量に行われる
    - `.rabitt .house{ }` というのだけでも全ての要素に対してやっていく必要がある
- QuerySelectorの実装
    - rootから全ての要素を列挙する
    - 一個一個セレクタにマッチするかどうかを確認
         - 現実的に動いてるのはC++が早いだけなのでは
    - return マッチしたものを返す
- CSS JITのゴール
    - セレクタマッチングを高速化するのは重要な意味を持つ
    - あるセレクタが与えられた要素にマッチするかを返すpredicateの高速化
- 現在の高速化実装
    - C++実装
    - セレクタを div と ~ という感じに分ける
    - ~や+は関係性を示すもの(combinator)
    - div 単純な子孫、desendant(simple)
    - => 合わせてcompound selector
- CSS Selectorの基礎
    - descendantから親を辿る列は常に一定になる (子 -> 親 -> 親)
    - CSSセレクタは右からマッチしていく
- div> のケース
    - 子(div) -> 親を見て間違ってる時、そこで探査は終わる
    - > は子なので、親がdivじゃない時で探査はやめていい
- バックトラッキング
    - セレクタのマッチを確認して行った時に間違ったマッチが起きた時にバックトラックをする
    - 間違っていた場合はdescendantに戻ってマッチを再開
    - マッチに失敗した時、次にマッチを再開するときにマッチの候補は少なくなるはず
    - そういう仮定をして、 `section > div＊1  div＊2  span` という時
         - ＊1で間違ってた時に、＊2の方をずらしたりしない
         - 余計なバックトラックだとわかってるので、div＊2にマッチする要素を再度探索するところからはやらない
         - ＊1に関してを探索していく
- CSS Selector JIT
    - セレクタのマッチングをJITをしていく
- マッチングコードのコンパイル
    - バックトラッキングを開始する時
    - descendantに戻る
    - descendantはレジスタのどこにあるのか分かってる
    - そのため、レジスタにある一個前(descendant)へjumpすればいい
    - 関数コールとかが必要なくなる
    - これでバックトラッキングが高速に出来る
    - バックトラック用のレジスタが1つでいける = スタックに積んでいない
    - バックトラックで必要なのは最後に見たやつだけ
    - ステートマシンみたいなジャンプするコードが吐かれてる
- アセンブラで見るCSS JIT
    - spanとかの文字列はatomic、ポインタで持ってくる
    - 直接`cmp`して比較、jnzでジャンプ
    - parentを見た時に、null pointer = rootまできちゃった
    - textノードじゃなくてelementかどうかを `test`でチェック
- More intelligent バックトラッキング
    - `ul > li > p span`
    - ulが失敗した時、子供のelementとして pや spanを持ってる事を知ってる
    - そのため、spanのチェックを省けて再開する
    - つまり保存する1つのレジスタを削れる + 無駄なバックトラッキングを1つ減る
    - 親をlookupする処理は重い = ノードは適当に作られるのでメモリの分布がバラバラでキャッシュ効率が悪くて、ジャンプするコストがある
         - バックトラッキングがひとつ減る効果はある
- ステータス
    - x64 / ARMv7s(nthでの計算命令) / ARM64 バックエンド 等に対応してる
- まとめ
    - CSS Selectorのマッチングコストは大きいので高速化できる
    - 普通にやっていくとドンドンスタックを積んでいくので遅くなる
    - CSS JITでは最小限をレジスタに置く、関数コールを省いてジャンプコールだけでやれる
    - より賢いバックトラッキングをやっていく
- Q&A
    - Q. 全てのセレクタが対象?
    - A. 7割ぐらいは対象、擬似要素、擬似クラスは対象外、今後やっていく
    - Q. :hoverとか :activeとかも効率的?
    - A. element自体にそういうフラグが入ってるので、CSS JITで高速化の対象になる
    - Q. 並列?
    - A. 今はシーケンシャルな性能についてを検証、実装
    - Q. JITコンパイルしたコードの容量の問題はないの?
    - A. JsCoreのインラインマクロでJIT生成するので、基本はそっちが破棄とか自動でやってくれるので気にしてない。
    - ある程度自動的に解放してくれたりをやってくれる
    - Q. CSSセレクタ自体の最適化やってる?
    - A. 常に失敗するというケースはスグにFAILさせるというのはある。
    - 静的に失敗するケースが分かるものはその時点で捨ててる
    - Q. atomizeするタイミング
    - A. JSでタグ作った時とかCSSパースした時とか出てきた時に何でもatomizeしていく
    - https://twitter.com/objectxplosive/status/488229099177930753
    - Q. descendantだけ?
    - A. 隣接とかも同じような仕組みでやってる
    - descendantとsibling等でグルーピングしてマッチングのバックトラッキングはやる
    - siblingが並ぶパターン自体が少ない

----

* メモ
    * omnioutliner
