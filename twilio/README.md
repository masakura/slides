# Twilio イベント LT 案
## DemaeBocco 紹介
DemaeBocco のデモ動画を流す。どういうものかの簡単な説明をする。


## TwilioServer の紹介
まだ作成途中だけど...

Twilio の特徴としてとても簡単に電話の応答をするアプリを簡単に書く事ができます。これは大変素晴らしいことです。ですが、応答が複雑になるとプログラムが複雑化してしまいます。

これは、一般的な Web API の使い方と異なるためです。一般的に、API へのリクエストはサーバーへ送信され、結果がレスポンスとして返ってきます。

```javascript
app
  .request('命令')
  .then(function (response) {
     console.log(response.result);
  });
```

Twilio では、開発者から見るとこれが逆転しています。Twilio への命令は Twilio へのレスポンスとして、その結果は Twilio からのリクエストとして発生します。応答が複雑になるとコードが複雑化する傾向にあります。さらに、電話の相手を区別して処理を分岐しなければいけないこともコードが複雑化する事に拍車をかけています。

```javascript
server
.post('/', function (request, response) {
    // CallSID でユーザーの現在の状態を取得
    var state = status[request.body.CallSid];

    // 状態で分岐
    switch (state) {
      case: 'first':
        switch (request.body.Digits) {
          case: '0':
            // 命令を出す
            response.send('<Response>....</Response');
            // 次の状態に進める
            status[request.body.CallSid] = 'second';

            break;

            // ...以下省略
        }
        break;
        // ...以下省略
    }
  });
````

コードが複雑化する事を防ぐために DemaeBocco には TwilioServer というライブラリを作りました。これによって、電話の相手事の処理の分岐などの複雑な処理を書かずに済むことを実現しました。

```javascript
// Twilio Server の起動
var server = new TwilioServer();
server.start()
```

```javascript
server
  .dial('xxxxxxxxx')
  .gather('今日の天気は? 晴れは0、雨は1、それ以外は9を押してください')
  .then(function (result) {
    if (result.Digits = '0') {
      return server.say('お天気がいいのですね!');
    }
    if (result.Digits = '1') {
      server
        .gather('傘は持っていますか? 持って入れば0、持っていなければ1を押してください')
        .then(function (result) {
          return server
            .say(result.Digits === '1' ? 'コンビニで買えるよ!' : '傘を忘れないように!');
        });
    }
    return server.say('微妙なのですね!');
  })
  .say('ご協力ありがとうございました!');
```

この様な形で、さも Web API に命令を出す通常の書き方に近い書き方で、Twilio の電話応答を書く事ができます。

特に MA のように短時間でコードを書かないといけない場合に効果を発揮すると思います。

まだ、Twilio には不慣れで汎用性がありませんが、HTTPS 化やセキュリティに対する配慮、npm twilio モジュールとの親和性の向上などを行って公開しようと思っています。
