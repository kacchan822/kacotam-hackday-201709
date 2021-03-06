このドキュメントは、[特定非営利活動法人Kacotam](http://kacotam.com/)におけるプログラミング学習会のテキストとして執筆されたものです。  
このドキュメント内で使用するシステム名、製品名は、それぞれ各社の商標、または登録商標です。なお本文中では、&trade;, &copy;, &reg;は省略しています。  
このドキュメントは、[クリエイティブ・コモンズ 表示 4.0 国際 ライセンス](http://creativecommons.org/licenses/by/4.0/)の下に提供されています。  
 ![クリエイティブ・コモンズ 表示 4.0 国際 ライセンス](https://i.creativecommons.org/l/by/4.0/88x31.png "クリエイティブ・コモンズ 表示 4.0 国際 ライセンス")
 
 ---


## 目標

Google App Scriptで、定期的にやるようなこと（例：天気予報を調べる、運休情報を調べる、新刊の発売情報を調べる...）を自動化して、ちょっとした手間を省けるようになる。


## 対象

* なんだかプログラミングとかパソコンってものに興味があるけど、なかなか始めるきっかけがなかった．．．な人
* ちょっとJavascript触ったことあるけどよーわからん。Google App Scriptなにそれ．．．な人

*注：上記対象とするため、内容的に厳密性を各部分が散見されますが、入門編ということであしからず。*


## 準備するもの

* Googleアカウント  
  （新しく作るとき）https://accounts.google.com/SignUp
* Google Chrome または Mozilla Firefox  
  （Chromeの入手）https://www.google.co.jp/chrome/browser/desktop/index.html  
  （Firefoxの入手）https://www.mozilla.org/ja/firefox/new/


## リポジトリ

このドキュメントで出てくる各種コードは、次のリポジトリにまとめてあります。
https://github.com/KacotamLab/kacotam-hackday-201709


## [Google App Script](https://developers.google.com/apps-script/) とは

https://www.google.com/script/start/

以下、**Google App Script**のことを**GAS**と省略して表記します。


## [Javascript](https://developer.mozilla.org/ja/docs/Web/JavaScript) とは

以下、**Javascript**のことを**JS**と省略して表記します。


## 最初の一歩

**問001: 1 から 100 までの数字すべてを足し合わせるといくつになるか求めよ。**
高校の数学（数学B--数列、等差数列の和のあたり）でお目にかかる問題。また、著名な数学者[ガウスの幼少期の逸話](https://ja.wikipedia.org/wiki/%E3%82%AB%E3%83%BC%E3%83%AB%E3%83%BB%E3%83%95%E3%83%AA%E3%83%BC%E3%83%89%E3%83%AA%E3%83%92%E3%83%BB%E3%82%AC%E3%82%A6%E3%82%B9#.E7.94.9F.E3.81.84.E7.AB.8B.E3.81.A1.E3.81.A8.E5.B9.BC.E5.B9.B4.E6.9C.9F)としても有名な問題。
この問題をプログラムを書いて解いてみよう。

### [算術演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)

数学で出てくる代表的な以下の計算は、JSに限らずほとんどのプログラミング言語で使えます（言語によって一部の記号が違うことも）。
かけ算、○乗の記号以外は、馴染みのある記号がそのまま使われているのではないでしょうか。

* 足し算（加算）： `+`
* 引き算（減算）： `-`
* かけ算（乗算）： `*`
* わり算（除算）： `/`
* ○乗（べき乗）： `**`

それでは、簡単な計算を実際にやってみましょう。

ブラウザを起動して、キーボードのF12キーを押すと、開発者ツール（デベロッパーツール）という画面が出るので、そこのConsole（コンソール）という部分を開いて次のような計算を試してみましょう。

*> は、ブラウザのConsole（コンソール）で入力した行を表し、<- はその結果として表示された行を表します。*

```js

// 足し算
> 5 + 2
<- 7

// 引き算
> 5 - 2
<- 3

// かけ算
> 5 * 2
<- 10


// わり算
> 5 / 2
<- 2.5

// ○乗
> 5 ** 2
<- 25

// 複数の算術演算子を組み合わせた計算
> 5 + 2 * 3 / 2
<- 8

> (5 + 2) * 3 / 2
<- 10.5

```

これで「1 から 100 までの数字すべてを足し合わせる」ことができますね？

```js

> 1 + 2 + 3 + .... + 99 + 100
<- 5050

```

途中を省略しましたが、一生懸命すべての数字を足して行けば答えは出るはずです。でも、面倒くさいですね。もし意地悪な先生が、「じゃあ 1 から 10000 までだとどうでしょう？」なんて言ってきたら…。

もしかすると、ガウスの逸話を知っている人は、次のような計算式を考えるかもしれませんね。

```js

> (1 + 100) * 50
<- 5050

```

もしくは、

```js

> (1 + 100) * 100 / 2
<- 5050

```

これならば、100（や50）のところを与えられた数字に書き換えればいいだけなので、簡単です。

ん？なんだか数学的よくわからないって？
それでは、一生懸命やっていくパターン(q001-01)をプログラミングでよく使う**ループ**（繰り返し、反復）を使って書いてみましょう。

ループの前に、プログラミングを進めていく上で大切な**変数**や**データ型**について確認しておきましょう。

### [変数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Grammar_and_types#Variables)

**変数（へんすう）**は、プログラムの中で**いろいろなデータをしまっておく箱**のようなものだと思ってください。**変数名**というのは、その箱の名前です。

「あの箱の中身をつかってー」の「あの」が何を指しているのかはっきりしないと、プログラムが大混乱するので、**変数**には名前(**変数名**)をつけます。

JSの場合は、変数名には、次のような決まりがあります。

* 文字（A-Z, a-z）、アンダースコア (_)、あるいはドル記号 ($) から始まる
* 2文字目以降は、数字 (0-9) も使用できる
* 大文字と小文字は区別される

*JSの仕様としては、Unicode文字列名でも変数として使用できるので、日本語を変数に使うこともできますが、あまり一般的ではないようです。*

JSで変数を使うときには、次のように変数を使うよ！ということを宣言する必要があります。

```js

// とにかくなんか変数
var x = 0;

// ブロックというかたまりの中でだけ使える変数
let y = 1;

// 値を変更しない変数
const z = 10;

```


### [データ型](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Grammar_and_types#Data_types)

|型名                 |どんなの                            |
|--------------------|-----------------------------------|
|真偽値 (Boolean)     | true または false                  |
|null                | null 値を意味する特殊なキーワード      |
|undefined           | 値が未定義を意味する                  |
|数値 (Number)        | 42 や 3.14159 など                 |
|文字列 (String)      | "Howdy" など                       |
|シンボル (Symbol)    | インスタンスが固有で不変となるデータ型。 |
|オブジェクト (Object) | [いろいろ :arrow_right:](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects)　                         |


### [forループ](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/for)

繰り返しの処理は、「1から100までの数字をすべて足す」、「クラス全員の名前を呼ぶ（→クラスの人を一人目から最後の人まで呼ぶ）」というように基本的に「`hogehoge` **から** `pyopyo` **まで** `fugafuga` **する** 」というように表せるものです。

JSでは、`for (最初の値; いつまで続ける; 繰り返し後にやること) 何をする`のようにして繰り返し処理を書くことができます。

最初の例として、**「1から10までを数える。」**について考えてみましょう。
`for (最初の値; いつまで続ける; 繰り返し後にやること) 何をする`にどのように当てはめるかというと、

- **最初の値**： 数える数が1
- **いつまで続ける**： 数える数が10
- **繰り返し後にやること**： 数える数を次の数にする（1増やす）
- **何をする**：　数える数を表示する

となり、以下のように書けます。（数える数を`i`という変数に入れています。）

```js

> for (var i = 1; i <= 10; i = i + 1) {console.log(i)};
  1
  2
  3
  4
  5
  6
  7
  8
  9
  10
<- undefined
```

よく使う書き方ですが、`i = i + 1`が実はもうちょっとシンプルに書くことができます。
**[インクリメント](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment)** を使うと、`i++`と書くだけで済みます。

では、**問001: 1 から 100 までの数字すべてを足し合わせるといくつになるか求めよ。** をやってみましょう。

- **最初の値**： 最初の`足す数`は1
- **いつまで続ける**： 最後の`足す数`は100
- **繰り返し後にやること**：　`足す数`を次の数にする（1増やす）
- **何をする**：　`足す数`を`それまでの和`に足す

 **HINT**
- `足す数`と`それまでの和`の2つの変数が必要になります。
- `足す数`を`i`、`それまでの和`を`result`という変数に入れましょう。
- `それまでの和`は`for`の外で定義します。
- 命令文の区切りは、`;`です。

```js

// 自然数（1,2,3, ... , 100）の和を求める
> var result = 0; for (var i = 1; i <= 100; i++) {result = result + i}; console.log(result)
5050
<- undefined
```

## もう一歩踏み出そう

**問002: Fizz Buzzの正しい答えを30までについて書き出せ。**

> **Fizz Buzz**
> **Fizz Buzz**（フィズ・バズ、**Bizz Buzz**や**Buzz**とも呼ばれる）は英語圏で長距離ドライブ中や飲み会の時に行われる言葉遊びである。
> プレイヤーは円状に座る。最初のプレイヤーは「1」と数字を発言する。次のプレイヤーは直前のプレイヤーの次の数字を発言していく。ただし、3で割り切れる場合は「**Fizz**」（**Bizz Buzz**の場合は「**Bizz**」）、5で割り切れる場合は「**Buzz**」、両者で割り切れる場合（すなわち15で割り切れる場合）は「**Fizz Buzz**」（**Bizz Buzz**の場合は「**Bizz Buzz**」）を数の代わりに発言しなければならない。発言を間違えた者や、ためらった者は脱落となる。
> https://ja.wikipedia.org/wiki/Fizz_Buzz

この問題をプログラムを書いて解いてみよう。
「`hogehoge` **から** `pyopyo` **まで** `fugafuga` **する** 」パターンだ！と気がついたかな。でも少し違う。「`fugafuga` **する** 」の場所が、「`foo`なら`bar`する」という形になっています。

### [if...else](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/if...else)

「`foo`なら`bar`する」というのは、いわゆる**if..else文**を使うことで書くことができます。

```js

// 基本形
if ( 条件 ) {

  //条件に一致したときにやること

} else {

  //条件に一致しなかったときにやること(なくてもよい)

}


// ifを連ねる場合
if ( 条件1 ) {

 // 条件1に一致したときにやること

} else if ( 条件2 ) {

 // 条件1に一致せず、条件2に一致したときにやること

} else if ( 条件3 ) {

 // 条件1と2に一致せず、条件3に一致したときにやること

} else {

 // どの条件にも一致しなかったときにやること（なくてもいい）

}

```

ここからは、ブラウザのconsoleでやるのは大変なので、[今日のレポジトリ](https://github.com/KacotamLab/kacotam-hackday-201709) からダウンロードしてきたファイルを編集しながらやっていきましょう。

早速、 **問002: Fizz Buzzの正しい答えを30までについて書き出せ。** を`try-q002.js`にプログラムを書いて解いてみましょう。なお、画面への出力は、`document.write('出力したい文字');`とすることでできます。


```js
for (var i = 1; i <= 30; i++) {
    if (i % 3 === 0) {
        document.write('Fizz\n');
    } else if (i % 5 === 0) {
        document.write('Buzz\n')
    } else if (i % 15 === 0) {
        document.write('Fizz Buzz\n');
    } else {
        document.write(i + '\n')
    }
}
```

こんな具合でしょうか。
でも、実はこれだと、**15と30のときの結果が正しくありません**。**15は3でも5でも割れる数**なので、最初の条件`i % 3 === 0`を満たしてしまい、`Fizz`と表示されてしまいます。

```js

for (var i = 1; i <= 30; i++) {
    if (i % 15 === 0) {
        // 3でも5でも割り切れるのを先に調べる必要がある
        document.write('Fizz Buzz\n');
    } else if (i % 3 === 0) {
        // ３で割り切れる
        document.write('Fizz\n');
    } else if (i % 5 === 0) {
        // ５で割り切れる
        document.write('Buzz\n')
    } else {
        // ３でも５でも割り切れない
        document.write(i + '\n')
    }
}

```

## いざGoogleAppScriptの世界へ...
