# 2. 初級 - JavaScript

<!-- TOC -->

- [2. 初級 - JavaScript](#2-初級---javascript)
  - [2.1. ローカル変数導出](#21-ローカル変数導出)
    - [2.1.1. Step 1 - 直値を変数へ代入](#211-step-1---直値を変数へ代入)
    - [2.1.2. Step 2 - 変数利用](#212-step-2---変数利用)
    - [2.1.3. Step 2 - 式の結果を変数へ代入](#213-step-2---式の結果を変数へ代入)
    - [2.1.4. Step 4 - 変数利用](#214-step-4---変数利用)
  - [2.2. 変数のインライン化](#22-変数のインライン化)
    - [2.2.1 Step 1 - 変数のインライン化](#221-step-1---変数のインライン化)
    - [2.2.2 Step 2 - 不要変数の削除](#222-step-2---不要変数の削除)
    - [2.2.3 Step 3 - 変数のインライン化](#223-step-3---変数のインライン化)
    - [2.2.4 Step 4 - 不要変数の削除](#224-step-4---不要変数の削除)
  - [2.3. 変数のリネーム](#23-変数のリネーム)
    - [2.3.1 Step 1 - 新変数の作成](#231-step-1---新変数の作成)
    - [2.3.2 Step 2 - 新変数の利用](#232-step-2---新変数の利用)
    - [2.3.3 Step 3 - 旧変数への依存の除去](#233-step-3---旧変数への依存の除去)
    - [2.3.4 Step 4 - 旧変数の削除](#234-step-4---旧変数の削除)
  - [2.4. 関数のリネーム](#24-関数のリネーム)
    - [2.4.1. Step 1 - 希望名称の関数作成](#241-step-1---希望名称の関数作成)
    - [2.4.2. Step 2 - 新関数の利用](#242-step-2---新関数の利用)
    - [2.4.3. Step 3 - 旧関数への依存の除去](#243-step-3---旧関数への依存の除去)
    - [2.4.4. Step 4 - 旧関数の削除](#244-step-4---旧関数の削除)

<!-- /TOC -->

## 2.1. ローカル変数導出

```js
// テストコード
it('半径 2 の円の面積が 12.56 であること', () => {
  expect(menseki(2)).toBe(12.56)
})

// プロダクトコード
function menseki(r) {
  return r*r*3.14
}
```

### 2.1.1. Step 1 - 直値を変数へ代入

直値を変数に切り出します。

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14             // <- 変数を用意します
  return r*r*3.14
}
```

### 2.1.2. Step 2 - 変数利用

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  return r*r*pi               // <- 変数利用に変えます
}
```

### 2.1.3. Step 2 - 式の結果を変数へ代入

return 中の式を変数に移動します。

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi        // <- 変数を用意します。
  return r*r*pi
}
```

### 2.1.4. Step 4 - 変数利用

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi
  return ret                // <- 変数利用に変えます
}
```

## 2.2. 変数のインライン化

ローカル変数の導出とは逆に、変数をインライン展開します。

```js
// テストコード
it('半径 2 の円の面積が 12.56 であること', () => {
  expect(menseki(2)).toBe(12.56)
})

// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*3.14
  return ret
}
```

### 2.2.1 Step 1 - 変数のインライン化

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*3.14        // <- pi をインライン化します。
  return ret
}
```

### 2.2.2 Step 2 - 不要変数の削除

```js
// プロダクトコード
function menseki(r) {
  // const pi = 3.14          // <- 不要になったので消します。
  const ret = r*r*3.14
  return ret
}
```

### 2.2.3 Step 3 - 変数のインライン化

```js
// プロダクトコード
function menseki(r) {
  const ret = r*r*3.14
  return r*r*3.14             // <- 式をインライン化します。
}
```

### 2.2.4 Step 4 - 不要変数の削除

```js
// プロダクトコード
function menseki(r) {
  // const ret = r*r*3.14     // <- 不要になったので消します
  return r*r*3.14
}
```

## 2.3. 変数のリネーム

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

### 2.3.1 Step 1 - 新変数の作成

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  const ret = pi              // <- 新たな変数を作ります。
  return pi
}
```

### 2.3.2 Step 2 - 新変数の利用

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  const ret = pi
  return ret                  // <- 戻り値を新たな変数に変えます。
}
```

### 2.3.3 Step 3 - 旧変数への依存の除去

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  const ret = 3.14            // <- pi 依存から切り離します。
  return ret
}
```

### 2.3.4 Step 4 - 旧変数の削除

``` js
// プロダクトコード
function getPi() {
  // const pi = 3.14          // <- 最後に pi を消します。
  const ret = 3.14
  return ret
}
```

リネームが完了しました。

## 2.4. 関数のリネーム

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

### 2.4.1. Step 1 - 希望名称の関数作成

変更後の名前で、新たな関数として作ります。

``` js
// プロダクトコード
function adhoc() {
  return 0
}

function zero() {             // <- 新たに関数を作ります。
  return adhoc()              // <- 元の関数を呼び出すだけです。
}
```

### 2.4.2. Step 2 - 新関数の利用

テストコードを直します。

``` js
// テストコード
it('0 を返すこと', () => {
  expect(zero()).toBe(0)      // <- 新たな関数を使います。
})
```

### 2.4.3. Step 3 - 旧関数への依存の除去

プロダクトコードを直します。

``` js
// プロダクトコード
function adhoc() {
  return 0
}

function zero() {
  return 0                    // <- 元の adhoc 関数の中身を移します。
}
```

### 2.4.4. Step 4 - 旧関数の削除

``` js
// プロダクトコード
// function adhoc() {         // <- 使わなくなった関数を消します。
//   return 0                 //
// }                          //

function zero() {
  return 0
}
```

---

&copy; 2020 Tommy@degino Inc.
