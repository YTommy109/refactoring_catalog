# 2.2. 変数のインライン化

英語名: [Inline Variable](https://refactoring.com/catalog/inlineVariable.html)

<!-- TOC -->

- [2.2. 変数のインライン化](#22-変数のインライン化)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - 変数のインライン化](#step-1---変数のインライン化)
  - [Step 2 - 不要変数の削除](#step-2---不要変数の削除)
  - [Step 3 - 変数のインライン化](#step-3---変数のインライン化)
  - [Step 4 - 不要変数の削除](#step-4---不要変数の削除)

<!-- /TOC -->

## Step 0 - 最初のコード

ローカル変数の導出とは逆に、変数をインライン展開します。

```js
// テストコード
it('半径 2 の円の面積が 12.56 であること', () => {
  expect(menseki(2)).toBe(12.56)
})
```

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*pi
  return ret
}
```

## Step 1 - 変数のインライン化

```js
// プロダクトコード
function menseki(r) {
  const pi = 3.14
  const ret = r*r*3.14        // <- pi をインライン化します。
  return ret
}
```

## Step 2 - 不要変数の削除

```js
// プロダクトコード
function menseki(r) {
  // const pi = 3.14          // <- 不要になったので消します。
  const ret = r*r*3.14
  return ret
}
```

## Step 3 - 変数のインライン化

```js
// プロダクトコード
function menseki(r) {
  const ret = r*r*3.14
  return r*r*3.14             // <- 式をインライン化します。
}
```

## Step 4 - 不要変数の削除

```js
// プロダクトコード
function menseki(r) {
  // const ret = r*r*3.14     // <- 不要になったので消します
  return r*r*3.14
}
```

---

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
