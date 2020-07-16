# 2.4. 関数の導出 - 変数名の同期

<!-- TOC -->

- [2.4. 関数の導出 - 変数名の同期](#24-関数の導出---変数名の同期)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - コピペ関数の作成](#step-1---コピペ関数の作成)
  - [Step 2 - 関数の利用](#step-2---関数の利用)
  - [Step 3 - 不使用コードの削除](#step-3---不使用コードの削除)
  - [Step 4 - テストコードのメンテナンス](#step-4---テストコードのメンテナンス)

<!-- /TOC -->

## Step 0 - 最初のコード

処理が複雑になり、一部を関数に切り出したくなるのはよくあることです。安全に関数を外部に切り出しましょう。

```js
// テストコード
it('49 なら Low になること', () => {
  expect(main(49)).toEqual('Low')
})
it('50 なら High になること', () => {
  expect(main(50)).toEqual('High')
})
```

```js
// プロダクトコード
function main(value) {
  let ans = null
  if (value>=50) {
    ans = 'High'
  } else {
    ans = 'Low'
  }
  return ans
}
```

## Step 1 - コピペ関数の作成

```js
// プロダクトコード
function highAndLow(value) {  // <- 関数を作成します
  let ans = null              // <- コピペ
  if (value>=50) {            // <- コピペ
    ans = 'High'              // <- コピペ
  } else {                    // <- コピペ
    ans = 'Low'               // <- コピペ
  }                           // <- コピペ
  return ans                  // <- コピペ
}                             // <- 関数の作成
function main(value) {
  let ans = null
  if (value>=50) {
    ans = 'High'
  } else {
    ans = 'Low'
  }
  return ans
}
```

## Step 2 - 関数の利用

```js
// プロダクトコード
function highAndLow(value) {
  let ans = null
  if (value>=50) {
    ans = 'High'
  } else {
    ans = 'Low'
  }
  return ans
}
function main(value) {
  let ans = null
  if (value>=50) {
    ans = 'High'
  } else {
    ans = 'Low'
  }
  ans = highAndLow(value)   // <- 関数を使う行を追加します
  return ans
}
```

## Step 3 - 不使用コードの削除

```js
// プロダクトコード
function highAndLow(value) {
  let ans = null
  if (value>=50) {
    ans = 'High'
  } else {
    ans = 'Low'
  }
  return ans
}
function main(value) {
  let ans = null
  // if (value>=50) {         // <- 使わなくなったので消します。
  //   ans = 'High'           // <-
  // } else {                 // <-
  //   ans = 'Low'            // <-
  // }                        // <-
  ans = highAndLow(value)
  return ans
}
```

## Step 4 - テストコードのメンテナンス

main は表示するだけで、実質的な処理は highAndLow に移動しているので、テストコードをメンテナンスします。

```js
// テストコード
it('49 なら Low になること', () => {
  expect(highAndLow(49)).toEqual('Low')  // <- highAndLow のテストに変えます
})
it('50 なら High になること', () => {
  expect(highAndLow(50)).toEqual('High')  // <- highAndLow のテストに変えます
})
```

---

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
