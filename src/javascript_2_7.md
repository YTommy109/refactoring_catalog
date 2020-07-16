# 2.7. 関数のリネーム

<!-- TOC -->

- [2.7. 関数のリネーム](#27-関数のリネーム)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - 希望名称の関数作成](#step-1---希望名称の関数作成)
  - [Step 2 - 新関数の利用](#step-2---新関数の利用)
  - [Step 3 - 旧関数への依存の除去](#step-3---旧関数への依存の除去)
  - [Step 4 - 旧関数の削除](#step-4---旧関数の削除)

<!-- /TOC -->

## Step 0 - 最初のコード

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

## Step 1 - 希望名称の関数作成

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

## Step 2 - 新関数の利用

テストコードを直します。

``` js
// テストコード
it('0 を返すこと', () => {
  expect(zero()).toBe(0)      // <- 新たな関数を使います。
})
```

## Step 3 - 旧関数への依存の除去

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

## Step 4 - 旧関数の削除

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

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
