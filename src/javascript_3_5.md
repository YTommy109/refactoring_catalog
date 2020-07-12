# 3.5. 関数の統合

<!-- TOC -->

- [3.5. 関数の統合](#35-関数の統合)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - ローカル変数の導出 1](#step-1---ローカル変数の導出-1)
  - [Step 2 - ローカル変数の導出 2](#step-2---ローカル変数の導出-2)
  - [Step 3 - ローカル変数の利用 3](#step-3---ローカル変数の利用-3)
  - [Step 4 - 関数の導出](#step-4---関数の導出)
  - [Step 5 - 関数の利用](#step-5---関数の利用)
  - [Step 6 - 変数のインライン化](#step-6---変数のインライン化)
  - [Step 7 - 不要変数の削除](#step-7---不要変数の削除)
  - [Step 8 - 変数のインライン化](#step-8---変数のインライン化)
  - [Step 9 - 不要変数の削除](#step-9---不要変数の削除)
  - [Step 10 - 新関数の利用](#step-10---新関数の利用)
  - [Step 11 - テストコードの新関数への置き換え](#step-11---テストコードの新関数への置き換え)
  - [Step 12 - 旧関数の削除](#step-12---旧関数の削除)

<!-- /TOC -->

## Step 0 - 最初のコード

似たような関数を一つにまとめます。

```js
// テストコード
it('5 倍になること', () => {
  expect(raise5(2)).toBe(10)
})
it('10 倍になること', () => {
  expect(raise10(2)).toBe(20)
})
```

```js
// プロダクトコード
function raise5(value) {
  return value * 5
}
function raise10(value) {
  return value * 10
}
```

## Step 1 - ローカル変数の導出 1

```js
// プロダクトコード
function raise5(value) {
  const multiplier = 5                  // <- 直値をローカル変数にします
  return value * multiplier             // <-
}
function raise10(value) {
  return value * 10
}
```

## Step 2 - ローカル変数の導出 2

```js
// プロダクトコード
function raise5(value) {
  const multiplier = 5
  const ret = value * multiplier        // <- 式をローカル変数にします
  return value * multiplier
}
function raise10(value) {
  return value * 10
}
```

## Step 3 - ローカル変数の利用 3

```js
// プロダクトコード
function raise5(value) {
  const multiplier = 5
  const ret = value * multiplier
  return ret                            // <- 戻り値をローカル変数に変えます
}
function raise10(value) {
  return value * 10
}
```

## Step 4 - 関数の導出

```js
// プロダクトコード
function raise(value, multiplier) {     // <- 式の部分を外部関数にします
  return value * multiplier             // <-
}                                       // <-
function raise5(value) {
  const multiplier = 5
  const ret = value * multiplier
  return ret
}
function raise10(value) {
  return value * 10
}
```

## Step 5 - 関数の利用

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  const multiplier = 5
  const ret = raise(value, multiplier)  // <- 新関数に置き換えます
  return ret
}
function raise10(value) {
  return value * 10
}
```

## Step 6 - 変数のインライン化

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  const multiplier = 5
  const ret = raise(value, multiplier)
  return raise(value, multiplier)       // <- 変数をインライン化します
}
function raise10(value) {
  return value * 10
}
```

## Step 7 - 不要変数の削除

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  const multiplier = 5
  // const ret = raise(value, multiplier) // <- 使わなくなった変数を消します
  return raise(value, multiplier)
}
function raise10(value) {
  return value * 10
}
```

## Step 8 - 変数のインライン化

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  const multiplier = 5
  return raise(value, 5)                // <- 変数のインライン展開します
}
function raise10(value) {
  return value * 10
}
```

## Step 9 - 不要変数の削除

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  // const multiplier = 5               // <- 使わなくなった変数を消します
  return raise(value, 5)
}
function raise10(value) {
  return value * 10
}
```

## Step 10 - 新関数の利用

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  return raise(value, 5)                // <- 追加した関数に置き換えます。
}
function raise10(value) {
  return raise(value, 10)               // <- 追加した関数に置き換えます。
}
```

旧関数内で新関数を呼んでテストが通れば、新関数の振る舞いを信用できます。そうすれば、安心してテストコードで新関数を利用できます。

## Step 11 - テストコードの新関数への置き換え

```js
// テストコード
it('5 倍になること', () => {
  expect(raise(2, 5)).toBe(10)          // <- 新しい関数への置き換えます
})
it('10 倍になること', () => {
  expect(raise(2, 10)).toBe(20)         // <- 新しい関数への置き換えます
})
```

## Step 12 - 旧関数の削除

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
// function raise5(value) {           // <- 使わなくなった関数を削除します
//   return raise(value, 5)           // <-
// }                                  // <-
// function raise10(value) {          // <-
//   return raise(value, 10)          // <-
// }                                  // <-
```

---

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
