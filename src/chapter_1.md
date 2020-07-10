# 1. 初級

## 1.0 解説

リファクタリングは、テストが通る状態を維持したまま、プロダクトコードの質を高めてゆくことです。

一見すると、無駄な手順を踏んでいるように思われるかもしれませんが、細かく刻んで進んでゆくことで、ハマることなく、スムーズにコードの改善が進んでゆくので、慣れてしまえば高速に開発できるようになります。悩んでたり、ハマってたりして、時間がどんどん溶けてゆく…そんなプログラマーの日常を激変させるのがリファクタリングテクニックと言えます。

初級では、ごく基本的なものを紹介します。リファクタリングをサポートする IDE やエディターを利用している場合は、そちらに任せた方が素早く安全にリファクタリングできます。ただ、一時的なバイパスを作って、少しずつ整理されたコードへ置き換えてゆくパターンは、以後の複雑なリファクタリングの基礎となるものです。 IDE やエディターのサポートがなくても自然にできるように、体にインストールしておくことは損にはならないでしょう。

## 1.1. ローカル変数導出

```js
function menseki(r) {
  return r*r*3.14
}
```

```js
// 定数を切り出す
function menseki(r) {
  const pi = 3.14       // <- 変数を用意する
  return r*r*pi         // <- 変数を利用する
}
```

```js
// 式を切り出す
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi    // <- 式を変数に入れる
  return ret            // <- 変数を利用する
}
```

### 1.2. 変数のインライン化

```js
function menseki(r) {
  //const pi = 3.14       // 消す
  const ret = r*r*3.14    // pi をインライン化する
  return ret
}
```

### 1.3. 変数のリネーム

``` js
// テストコード
it('3.14 を返すこと', () => {
  expect(getPi()).toBe(3.14)
})
```

``` js
// プロダクトコード org
function getPi() {
  const pi = 3.14
  return pi
}
```

``` js
// プロダクトコード gen1
function getPi() {
  const pi = 3.14
  const ret = pi    // <- 新たな変数を作ります。
  return pi
}
```

``` js
// プロダクトコード gen2
function getPi() {
  const pi = 3.14
  const ret = pi
  return ret          // <- 戻り値を新たな変数に変えます。
}
```

``` js
// プロダクトコード gen3
function getPi() {
  const pi = 3.14
  const ret = 3.14    // <- pi 依存から切り離します。
  return ret
}
```

``` js
// プロダクトコード gen4
function getPi() {
                      // <- 最後に pi を消します。
  const ret = 3.14
  return ret
}
```

リネームが完了しました。

### 1.4. 関数のリネーム

``` js
// テストコード
it('0 を返すこと', () => {
  expect(adhoc()).toBe(0)
})
```

``` js
// プロダクトコード org
function adhoc() {
  return 0
}
```

変更後の名前で、新たな関数として作ります。

``` js
// プロダクトコード gen1
function adhoc() {
  return 0
}

function zero() {         // <- 新たに関数を作ります。
  return adhoc()          // <- 元の関数を呼び出すだけです。
}
```

テストコードを直します。

``` js
// テストコード gen2
it('0 を返すこと', () => {
  expect(zero()).toBe(0)  // <- 新たな関数を使います。
})
```

プロダクトコードを直します。

``` js
// プロダクトコード gen3
function adhoc() {
  return 0
}

function zero() {
  return 0              // <- 元の関数を中身を移します。
}
```

``` js
// プロダクトコード gen4
// function adhoc() {   // <- 使わなくなった関数を消します。
//   return 0           //
// }                    //

function zero() {
  return 0。
}
```
