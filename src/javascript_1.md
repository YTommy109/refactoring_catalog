# 1. 初級 - JavaScript

<!-- TOC -->

- [1. 初級 - JavaScript](#1-初級---javascript)
  - [1.1. ローカル変数導出](#11-ローカル変数導出)
    - [1.1.1. Step 1](#111-step-1)
    - [1.1.2. Step 2](#112-step-2)
  - [1.2. 変数のインライン化](#12-変数のインライン化)
    - [1.2.2 Step 1](#122-step-1)
    - [1.2.2 Step 2](#122-step-2)
  - [1.3. 変数のリネーム](#13-変数のリネーム)
    - [1.3.1 Step 1](#131-step-1)
    - [1.3.2 Step 2](#132-step-2)
  - [1.4. 関数のリネーム](#14-関数のリネーム)
    - [1.4.1. Step 1](#141-step-1)
    - [1.4.2. Step 2](#142-step-2)
    - [1.4.3. Step 3](#143-step-3)
    - [1.4.4. Step 4](#144-step-4)

<!-- /TOC -->

## 1.1. ローカル変数導出

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

### 1.1.1. Step 1

直値を変数に切り出します。

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14             // <- 変数を用意します
  return r*r*3.14
}
```

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  return r*r*pi               // <- 変数利用に変えます
}
```

### 1.1.2. Step 2

return 中の式を変数に移動します。

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi        // <- 変数を用意します。
  return r*r*pi
}
```

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi
  return ret                // <- 変数利用に変えます
}
```

## 1.2. 変数のインライン化

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

### 1.2.2 Step 1

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*3.14        // <- pi をインライン化します。
  return ret
}
```

```js
// プロダクトコード
function menseki(r) {
  // const pi = 3.14          // <- 不要になったので消します。
  const ret = r*r*3.14
  return ret
}
```

### 1.2.2 Step 2

```js
// プロダクトコード
function menseki(r) {
  const ret = r*r*3.14
  return r*r*3.14             // <- 式をインライン化します。
}
```

```js
// プロダクトコード
function menseki(r) {
  // const ret = r*r*3.14     // <- 不要になったので消します
  return r*r*3.14
}
```

## 1.3. 変数のリネーム

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

### 1.3.1 Step 1

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  const ret = pi      // <- 新たな変数を作ります。
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

### 1.3.2 Step 2

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
  // const pi = 3.14  // <- 最後に pi を消します。
  const ret = 3.14
  return ret
}
```

リネームが完了しました。

## 1.4. 関数のリネーム

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

### 1.4.1. Step 1

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

### 1.4.2. Step 2

テストコードを直します。

``` js
// テストコード
it('0 を返すこと', () => {
  expect(zero()).toBe(0)  // <- 新たな関数を使います。
})
```

### 1.4.3. Step 3

プロダクトコードを直します。

``` js
// プロダクトコード
function adhoc() {
  return 0
}

function zero() {
  return 0              // <- 元の adhoc 関数の中身を移します。
}
```

### 1.4.4. Step 4

``` js
// プロダクトコード
// function adhoc() {   // <- 使わなくなった関数を消します。
//   return 0           //
// }                    //

function zero() {
  return 0
}
```

---

&copy; 2020 Tommy@degino Inc.
