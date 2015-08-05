# 題名未定
本日は HTML5 アプリを作り始める時に覚えておいた方がいいことをささーっとやります。あと、脈絡もなく断片的に話をします。ですので、フレームワークのお話などのより実践的なものはあまり出てきません。ごめんなさい。

また、ささーっとやりますので、細かいところは検索して調べることをお勧めします。


## メモレベル


### HTML4 と HTML5 の違い
細かいお話は調べれば出てきますので... 大雑把に言うと、HTML4 まではあくまでドキュメントを書くための言語だったのに対し、HTML5 からはアプリを作ることも (`も`に注意ね) ミッションのひとつになっていることです。

HTML5 Conference 2015 in 鹿児島の吉川さんの講義を見ると大変参考になります。信じられないようなことができたりしますので、帰ってから一度見てみることをお勧めします!

アプリを作るために何が必要か? と考えた時に、Cookie じゃとてもアプリのデータを保存するのはできないので Web Storage みたいな仕組みを導入しよう! のような流れで、HTML5 へどんどん機能が追加されて行っています。

最近ですと、以下のものが流行りのようです。

* WebRTC/ORTC
* Web Components
* Web Audio API/Web MIDI API

他にもたくさんあって紹介しきれません! ゲームコントローラーもつけられますし、何ができるのかは自分もよく分からないです。

HTML5 はどんどん進化を続けます! それ Flash 使わないとできませんとか、ネイティブじゃないとできませんのようなことはどんどんなくなっていきます。


### 開発者視点から見た今までの Web アプリと HTML5 アプリの違い
今までの PHP や Ruby on Rails などを利用したウェブアプリと HTML5 アプリの一番の違いはどこでコードが動作するか? です。今までのウェブアプリではほとんどのコードがサーバー側で実行されていましたが、HTML5 アプリではブラウザー側で動作するコードが圧倒的に多くなります。

その結果、ブラウザー側に求められる機能が多くなり、それらはライブラリやフレームワークという形で提供されることになりました。それに加え、HTML5 アプリでネイティブアプリのようなこともできるようにということで、HTML5/CSS3/JavaScript を進化させることにつながっています。

これらの変化により、JavaScript のコードの量が飛躍的に増大します。


### Single Page Application
今までのウェブアプリでは、一画面につき一 HTML のようなスタイルで書いていましたが、HTML5 アプリでは Single Page Application (SPA) という一枚の index.html だけでアプリを書くのが主流になっています。

今までのウェブアプリの JavaScript は操作を助けたりする目的で、そんなに大きなものではありません。ですので、速度面でもそれほどきにする必要はありませんでした。HTML5 アプリの場合、JavaScript のコードの量が大きくなりますので、ページを遷移するたびに新しく HTML ファイルを読み込んでいては非常に遅くなります。

SPA で HTML5 アプリを組むことで、HTML5/CSS3/JavaScript などのロードをページ遷移ごとに全部行うことを防ぐことができます。アプリの規模が大きくなるほど、この差は顕著になります。

なお、ページ遷移の際に URL を書き換えるため、History API というものを用います。ですが、ほとんどの場合、AngularJS や Backbone.js のようななんらかの MVC フレームワークを使用し、それに従うのが良いでしょう。

ToDo 実際に URL が変化する実例を見せた方がいいと思う

SPA なんて特別な言葉を使いましたけど、クライアントサイドでただ単にアプリが動くだけですね。構造的には Visual Basic などで書かれていたアプリとそれほど変わるものではありません。(ただ、昔とはアプリの規模が違いますけどね)


### 開発者ツールを使いこなせ!
HTML5 アプリではブラウザー上で動作するコードが増えるため、今までより開発者ツールを使いこなさないといけません。開発者ツールの機能を簡単にデモをします。ただ、自分よりウェブ屋さんの方が詳しいと思いますので、全部知ってることだったらごめんなさい。

今日は Google Chrome Developer Tools を使いますが、他のブラウザーでもだいたい同じような感じです。


### ベータ版ブラウザーを使おう!
Google Chrome に対する Chrome Canary や、Firefoxに対する Firefox Developer Edition などのようなちょっと先の実装のブラウザーを使うのもわりとお勧めです。もちろん、常用するのではなく、最先端の HTML5 API を触る時は大変有用です。

より最先端のものを触りたい時は Nightly Build を使いましょう!


### HTML5 API をあまり直接使わないこと
最近の HTML5 API は Extensible Web という考え方で作られています。ブラウザーは要件を満たす低レベルな API を用意し、開発者が使うライブラリーで簡便さを実現するような仕組みです。

要件が少しでも外れたらそれは使いにくい API になってしまいます。ブラウザー側で要件に近い高レベルな API を用意すると、ブラウザーがアップデートされるまでは対処できないといったことになります。そのあたりを、まだ修正や配布が容易なライブラリ側に任せることで、いち早く使えるものを提供できるようになっています。

HTML5 API を直接触った方ならお分かりだと思いますが、意外と面倒なんですね。これは、こういった理由で意図的に面倒に作られています。

ですので、一般的には HTML5 API を直接使うのではなく、より便利なライブラリを探してきてそれを使うようにしてください。

* WebRTC -> PeerJS/SkyWay
* WebGL -> Three.js
* WebComponents -> Polymer


### Strcit モードと無名関数の即時実行
HTML5 アプリでは JavaScript のコードが飛躍的に増えるのはお話した通りです。そのせいで、規模が小さいうちは大した問題にならないことが、HTML5 アプリでは大きな問題になります。

それを解決する手段はさまざまですが、本日は Strict モードと無名関数の即時実行を紹介します。(どちらも有用ですので、既に使われている方もいらっしゃると思います)


#### Strict モード
細かい話は置いておいて...

```javascript
var document = 'hoge';

console.log(document);
```

このコードを実行するとどうなると思います?

正解は `document` オブジェクトが表示されます。

一行目の変数の初期化はグローバルスコープで行われているため、`window.document` プロパティへの代入として実行されます。ですが、`window.document` プロパティは読み取り専用ですので、結果として `hoge` は設定されずに動作し、(スルーされるのがびっくりですよ!) 結果として `document` オブジェクトが表示されます。

以下のように `'use strict';` を書くことで Strict モードで動作するようになり、`var documment = 'hoge';` で例外が発生し、プログラムは停止します。

```javascript
'use strict';

var document = 'hoge';

console.log(document);
```

Strict モードにはこれ以外のたくさんの違いがありますので、コードを何百行も書いてから Strict モードに変えようということはかなり難しいです。ですので最初から Strict モードを使いましょう。


#### 無名関数の即時実行
ToDo 日付を書く

この前 ECMAScript6 が勧告されましたが、今のブラウザーで使えるのはほとんどの場合 ECMAScript5 です。ECMAScript5 では、他の一般的な言語と異なり関数でしかスコープを作ることができませんです。

ですので、スコープが欲しい時は、関数を書いて、その場で実行するという手段を取る必要があります。この関数は名前がある必要がありませんので、無名関数として宣言しそのまま実行するようなことを行います。

```javascript
(function () {
  // ここに処理を書く...
})();
```

書き方は他にもありますが、これが主流なのでこの書き方で揃えましょう!

なお、先ほど紹介した Strict モードもスコープで有効になります。グローバルスコープで `'use strict';` を書くと、読み込んだすべての JavaScript ライブラリが Strict モードで動作することになります。中には Strict モードではうまく動作しないライブラリもありますので、Strict モードの宣言も、関数スコープの中で行った方がいいです。

```javascript
(function () {
  'use strict';

  // ここに処理を書く...
})();
```

### その他注意事項
ウェブ系の開発者にとっては当たり前かもしれませんが、文字コードは UTF-8 (BOM なし) で、改行コードが LF のみというのが主流のようです。

Windows でよく使われているエディターの中には Shift_JIS や UTF-8 でも BOM をつけたり、概要コードが CRLF なものもありますのでご注意ください。