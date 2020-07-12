# 2.3. 変数のリネーム

<!-- TOC -->

- [2.3. 変数のリネーム](#23-変数のリネーム)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - 新変数の作成](#step-1---新変数の作成)
  - [Step 2 - 新変数の利用](#step-2---新変数の利用)
  - [Step 3 - 旧変数への依存の除去](#step-3---旧変数への依存の除去)
  - [Step 4 - 旧変数の削除](#step-4---旧変数の削除)

<!-- /TOC -->

## Step 0 - 最初のコード

``` js
// テストコード
it('3.14 を返すこと', () => {
  expect(getPi()).toBe(3.14)
})
```

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  return pi
}
```

## Step 1 - 新変数の作成

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  const ret = pi              // <- 新たな変数を作ります。
  return pi
}
```

## Step 2 - 新変数の利用

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  const ret = pi
  return ret                  // <- 戻り値を新たな変数に変えます。
}
```

## Step 3 - 旧変数への依存の除去

``` js
// プロダクトコード
function getPi() {
  const pi = 3.14
  const ret = 3.14            // <- pi 依存から切り離します。
  return ret
}
```

## Step 4 - 旧変数の削除

``` js
// プロダクトコード
function getPi() {
  // const pi = 3.14          // <- 最後に pi を消します。
  const ret = 3.14
  return ret
}
```

リネームが完了しました。

---

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
