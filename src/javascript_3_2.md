# 3.2. 関数の仮引数の変更

<!-- TOC -->

- [3.2. 関数の仮引数の変更](#32-関数の仮引数の変更)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - ローカル変数の抽出](#step-1---ローカル変数の抽出)
  - [Step 2 - 変数利用への変更](#step-2---変数利用への変更)
  - [Step 3 - 関数導出](#step-3---関数導出)
  - [Step 4 - 関数利用への変更](#step-4---関数利用への変更)
  - [Step 5 - 新しい関数への切り替え](#step-5---新しい関数への切り替え)
  - [Step 6 - 旧関数の削除](#step-6---旧関数の削除)
  - [Step 7 - 関数のリネーム](#step-7---関数のリネーム)
  - [Step 8 - 新関数への切り替え](#step-8---新関数への切り替え)
  - [Step 9 - 旧関数のロジックを新関数へ移動](#step-9---旧関数のロジックを新関数へ移動)
  - [Step 10 - 旧関数の削除](#step-10---旧関数の削除)
  - [Step 11 - テストコードのメンテナンス](#step-11---テストコードのメンテナンス)

<!-- /TOC -->

## Step 0 - 最初のコード

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square(5, '㎡')).toBe('25 ㎡')
})
```

``` js
// プロダクトコード
function square(size, unit) {
  return size * size + ' ' + unit
}
```

面積の計算と単位の付与を同時に行っているのが気になります。分離しましょう。

## Step 1 - ローカル変数の抽出

``` js
// プロダクトコード
function square(size, unit) {
  const menseki = size * size       // <- 面積の計算式を変数に切り出します。
  return size * size + ' ' + unit
}
```

## Step 2 - 変数利用への変更

``` js
// プロダクトコード
function square(size, unit) {
  const menseki = size * size
  return menseki + ' ' + unit     // <- 変数利用に書き換えます。
}
```

## Step 3 - 関数導出

``` js
// プロダクトコード
function square(size, unit) {
  const menseki = size * size
  return menseki + ' ' + unit
}
function square_new(size) {
  return size * size                    // <- 面積の計算を関数に切り出します。
}
```

## Step 4 - 関数利用への変更

``` js
// プロダクトコード
function square(size, unit) {
  return square_new(size) + ' ' + unit  // <- 関数利用に書き換えます
}
function square_new(size) {
  return size * size
}
```

## Step 5 - 新しい関数への切り替え

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square_new(5) + ' ㎡').toBe('25 ㎡')  // <- 新しい関数に置き換えます
})
```

## Step 6 - 旧関数の削除

``` js
// プロダクトコード
// function square(size, unit) {           // <- 使わないので消します
//   return square_new(size) + ' ' + unit  // <-
// }                                       // <-
function square_new(size) {
  return size * size
}
```

## Step 7 - 関数のリネーム

square_new は一時的な名称ですので、好ましい名称に変えます。

``` js
// プロダクトコード
function square(size) {           // <- 関数を追加します
  return square_new(size)         // <-
}                                 // <-
function square_new(size) {
  return size * size
}
```

## Step 8 - 新関数への切り替え

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square(5) + ' ㎡').toBe('25 ㎡')  // <- 新しい関数に置き換えます
})
```

## Step 9 - 旧関数のロジックを新関数へ移動

square_new は一時的な名称ですので、好ましい名称に変えます。

``` js
// プロダクトコード
function square(size) {
  return size * size          // <- square_new のロジックを移します
}
function square_new(size) {
  return size * size
}
```

## Step 10 - 旧関数の削除

``` js
// プロダクトコード
function square(size) {
  return size * size
}
// function square_new(size) {  // <- 使わないので消します
//   return size * size         // <-
// }                            //
```

## Step 11 - テストコードのメンテナンス

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square(5)).toBe(25)  // <- テストを適切な内容に変えます
})
```

この手順は、仮引数を削除する他に、順序を変えたり、仮引数名を変えたりといったケースでも同じ手順になります。追加の時も使えますが、 JavaScript の場合は手短な方を使うでしょう。

---

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
