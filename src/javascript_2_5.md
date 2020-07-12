# 2.5. 関数のインライン化

<!-- TOC -->

- [2.5. 関数のインライン化](#25-関数のインライン化)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - 変数名を同期 1](#step-1---変数名を同期-1)
  - [Step 2 - 変数名を同期 2](#step-2---変数名を同期-2)
  - [Step 3 - 関数のインライン化](#step-3---関数のインライン化)
  - [Step 4 - テストコードの削除](#step-4---テストコードの削除)
  - [Step 5 - 不使用関数の削除](#step-5---不使用関数の削除)

<!-- /TOC -->

## Step 0 - 最初のコード

```js
// テストコード
it('49 なら false になること', () => {
  expect(isHigh(49)).toBe(false)
})
it('50 なら true になること', () => {
  expect(isHigh(50)).toBe(true)
})
it('49 なら Low になること', () => {
  expect(heigAndLow(49)).toEqual('Low')
})
it('50 なら High になること', () => {
  expect(heigAndLow(50)).toEqual('High')
})
```

```js
// プロダクトコード
function isHigh(v) {
  return v >= 50
}

function highAndLow(value) {
  let ans = 'Low'
  if (isHigh(value)) {
    ans = 'High'
  }
  console.log(ans)
}
```

リファクタリングを進めた結果、とてもシンプルな関数に整理され、独立した関数にしておくことが適切ではなくなった…。そんな時は、インライン化しましょう。

## Step 1 - 変数名を同期 1

インライン化してもエラーが出ないように、事前に変数名に合わせておきます。

```js
// プロダクトコード
function isHigh(v) {
  const value = v             // <- 変数を用意します
  return v >= 50
}

function highAndLow(value) {
  let ans = 'Low'
  if (isHigh(value)) {
    ans = 'High'
  }
  console.log(ans)
}
```

## Step 2 - 変数名を同期 2

インライン化してもエラーが出ないように、事前に変数名に合わせておきます。

```js
// プロダクトコード
function isHigh(v) {
  const value = v
  return value >= 50          // <- 作成した変数に切り替えます
}

function highAndLow(value) {
  let ans = 'Low'
  if (isHigh(value)) {
    ans = 'High'
  }
  console.log(ans)
}
```

## Step 3 - 関数のインライン化

インライン化してもエラーが出ないように、事前に変数名に合わせておきます。

```js
// プロダクトコード
function isHigh(v) {
  const value = v
  return value >= 50
}

function highAndLow(value) {
  let ans = 'Low'
  if (value >= 50) {          // <- インライン化します
    ans = 'High'
  }
  console.log(ans)
}
```

## Step 4 - テストコードの削除

もう isHigh のテストは不要です。

```js
// テストコード
// it('49 なら false になること', () => {   // <- 消します
//   expect(isHigh(49)).toBe(false)       // <-
// })                                     // <-
// it('50 なら true になること', () => {    // <-
//   expect(isHigh(50)).toBe(true)        // <- 
// })                                     // <-
it('49 なら Low になること', () => {
  expect(heigAndLow(49)).toEqual('Low')
})
it('50 なら High になること', () => {
  expect(heigAndLow(50)).toEqual('High')
})
```

## Step 5 - 不使用関数の削除

インライン化してもエラーが出ないように、事前に変数名に合わせておきます。

```js
// プロダクトコード
// function isHigh(v) {       // <- 消します
//   const value = v          //
//   return value >= 50       //
// }                          //

function highAndLow(value) {
  let ans = 'Low'
  if (value >= 50) {
    ans = 'High'
  }
  console.log(ans)
}
```
