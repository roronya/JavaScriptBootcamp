# JavaScriptBootcamp
- フロントエンドエンジニアの定義は広くて深い
- サーバーサイドからユーザービリティまで考える
- 世間的な定義はフルスタックエンジニアに近い

## javascripting
- https://nodeschool.io/ja/
- https://github.com/recruit-tech/javascripting

### 高階関数のよいところ
- 無駄な変数宣言が無い
- だからバグの混入が少ないと言われている


# JavaScriptはどんな言語か
- Schemeベースで言語を作りたいと思っていた。
- NetExplorerでブラウザに組み込む言語として作った。
- Javaのようなシンタックス、Selfのプロトタイプベース、Schemeのオブジェクト指向っぽいところを混ぜた
- 実行環境によって大量のビルトインオブジェクトが組み込まれる
- よく聞かれる「普通のJSとNode.jsはどうちがうの？」＝＞違いません
- 違うのはビルトインオブジェクトが違う。ブラウザでは知らせるとDOMとかwindowとかのAPIが入ってる。Node.jsだとglobalとかfsとか入ってる。JSON、Date、Mathは共通。
- アイソモーフィック
	- ブラウザでもNode.jsでも共通なものを使おうという思想
- Node.jsでJSを学べばブラウザでも使える？
	- YesだけどNo

## 値
- "あたい"
- "ち"とは読まないｗ
- プリントデバッグでもいいじゃん。console.logデバッグでもいいけど、IDEのやつ覚えたほうが現場で役に立つでしょう。

## 変数
- 値に名前をつける仕組み。
- 値に名前をつけることを変数宣言。
- 変数を参照できる範囲をスコープ。
- 変数宣言にはやりかたが3つあるvar, let, const。
- ほんとは4つある(何もつけない）愚行なので教えない。
	- use strictしてると怒ってくれる
	- babelとか使ってると基本的にstrictモード
	- ゴルフのときはstrictモード外すといいよｗ
- const > let >>>>> (超えられない壁) >>>>>> var
- constで宣言すると再代入できない
- 再代入しようとするとTypeError
- varはいつ使うの
	- 基本的に使わない
	- ES2015以降で有効だからそれ以前のブラウザで動作するときは書く
	- シビアな性能が求められる時使う
		- varの方が若干速い
		- const/letは宣言したときにチェックが色々入る。（再代入してたら怒らないといけないとか）
- スコープ
	- JSのスコープは3つ
		- グローバル どこからでも
		- ローカル 関数の中のみ
		- ブロック ブレース'{}'で囲まれた領域のみ
- 変数は少ない方が良い
	- どこから読まれるのかをずっと考えないといけないので
	- 可読性が落ちるから
- varは巻き上げっていうのが起こる。どこで宣言しようと一番上で宣言したことになる。varはどれだけメモリ確保しておけばよいのか先に計算できるから速い。

## 関数
- 処理をまとめて1つのものを定義するもの
- JavaScriptでは関数はオブジェクトのひとつ
- JSにといて重要な概念
- 関数定義は2通りある
	- 文による定義  function a() {}
	- 式による定義  const a = function() {};
- どっちを使うべきか？
- function文だと巻き上げが起こるから、functionの宣言より前に呼び出しても呼べる
- 式はクロージャのときとか、スコープを制限したいときに使う
- アロー関数
	- 無名関数

## this

Q1. 関数
A.foo()
B.foo()
それぞれ何を指すか

```javascript
const A = {
	foo: () => {
		console.log(this);
	}
}

const B = {
	foo: function() {
		console.log(this);
	}
}
```

A. A.foo()はグローバルオブジェクト、B.foo()はB

アロー関数の内部にはthisが存在しないので、関数定義時にthisが静的に決定される。

通常の関数の場合はthisの指す値は動的に決まる

```javascript
B.foo() // class B
const a = B.foo;
a() // global object
```

呼び出し時に決まる

a()をBにしたいときbindを使う

```
const a = B.foo.bind(B);
```

1つの式しか無いときはreturnを省略できる

```javascript
const a = [1,2,3,4].filter(x => x % 2 ===0).map(x => x * x);
```

## ショートハンド関数
キープロパティとfunction文を省略できるようになった

```
const A = {
	foo () {
	}
}
```

## 即時関数
もう使うことは無いかも

```
(function() {
	var a = 1;
}();
```

定義した直後に呼び出すイディオム。スコープを閉じ込めたかった。const/letあるからもう使わない

## データ型
- Null型とUndefined型は違う
- 変数を宣言したけど何も入ってないものをNull、変数を宣言してない状態をUndefined型にしたかったんだけど、いまや混沌の極み…
- Number型は整数と少数が別れてない。全て浮動小数になっている。どう良いか？3/2は1になっちゃうが、JSは1.5って返る。
- 0除算のときに各言語がどう動くか
- int 1 / int 0
	- ZeroDiveError
- float 0 / float 0
	- infinity
- JSはすべてinifinity
- IEEE754に沿った演算しかやらないのがJS
- Symbol型 あんまり使わないかな…メタ情報書きたいときに使います。
- typeof演算子で型が文字列で返ってくる
- typeof null => "object" は仕様バグ。本当は"null"が帰って欲しい。
	- ここを"null"にすると動かなくなってしまうライブラリがめちゃめちゃあって直せてない
- typeof function() {}; => function
	- functionはobjectなのにfunctionが返る
- Objectのprotorype継承から外れることができる。
- Object.create(null)
	- hasOwnPropertyやtoStringといった組み込みメソッドが不要なときに使う。
- String, Number, BooleanどれもがObjectになりうる。
	- リテラルで書いたときはprimitive型だが、クラスからnewするとObjectになる
	- instanceof のみ typeofのときは大丈夫

```
"hoge" instanceof Object // => true
new String("hoge") instanceof Object // => true
```

## クラス
- ES2015から入った
- クラスという新しい概念になったわけでなくプロトタイプベースの関数

```
class A { 	foo() {
		console.log("A)
	}
}
```

- ビルトインオブジェクトも継承可能
- 便利なのはError
- errorStackを壊さずに継承できる
- 昔はビルトインオブジェクトが継承できなかったから、自前でErrorを作って、自前でErrorStackにつまないといけなかった。

```
class MyError extends Error {
}
```

## 条件分岐
-　これはもうよいですね。

## ループ
- これももうよいですね
- いっこだけ
- for of

```
for (const a of [ 1, 2, 3 ]) { 	console.log(a);
}
```

for of はiterableなものを繰り返すときの特別なfor文なのでconstで宣言できる。他のforやwhileはlet。

## 配列の高階関数
- forEach, filter, map, reduce

## もっと知りたい人
- js-primerを読む
- https://asciidwango.github.io/js-primer/

# TODOリストを作る
1. 外観を作る
1. 動きを与える
1. クライアントのテストを書く

