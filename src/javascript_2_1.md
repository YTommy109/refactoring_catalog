# 2.1. 変数の導出

英語名:  ([Extract Variable](https://refactoring.com/catalog/extractVariable.html))

<!-- TOC -->

- [2.1. 変数の導出](#21-変数の導出)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - 直値を変数へ代入](#step-1---直値を変数へ代入)
  - [Step 2 - 変数利用](#step-2---変数利用)
  - [Step 3 - 式の結果を変数へ代入](#step-3---式の結果を変数へ代入)
  - [Step 4 - 変数利用](#step-4---変数利用)

<!-- /TOC -->

## Step 0 - 最初のコード

```js
// テストコード
it('半径 2 の円の面積が 12.56 であること', () => {
  expect(menseki(2)).toBe(12.56)
})
```

```js
// プロダクトコード
function menseki(r) {
  return r*r*3.14
}
```

## Step 1 - 直値を変数へ代入

直値を変数に切り出します。

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14             // <- 変数を用意します
  return r*r*3.14
}
```

## Step 2 - 変数利用

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  return r*r*pi               // <- 変数利用に変えます
}
```

## Step 3 - 式の結果を変数へ代入

return 中の式を変数に移動します。

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi        // <- 変数を用意します。
  return r*r*pi
}
```

## Step 4 - 変数利用

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi
  return ret                // <- 変数利用に変えます
}
```

---

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
