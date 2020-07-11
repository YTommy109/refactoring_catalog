# 3. 中級 - JavaScript

<!-- TOC -->

- [3. 中級 - JavaScript](#3-中級---javascript)
  - [3.1. 関数導出](#31-関数導出)
  - [3.2. 関数のインライン化](#32-関数のインライン化)
  - [3.3. 関数の仮引数の追加](#33-関数の仮引数の追加)
    - [3.3.1. Step 1 - 仮引数を追加する](#331-step-1---仮引数を追加する)
    - [3.3.2. Step 2 - 仮引数を使用する](#332-step-2---仮引数を使用する)
    - [3.3.3. Step 3 - 呼び出し側に引数を追加する](#333-step-3---呼び出し側に引数を追加する)
    - [3.3.4. Step 4 - テストコードを追加する](#334-step-4---テストコードを追加する)
    - [3.3.5. Step 5 - 仮引数のデフォルト値を消す](#335-step-5---仮引数のデフォルト値を消す)
  - [3.4. 関数の仮引数の変更](#34-関数の仮引数の変更)
    - [3.4.1. Step 1 - ローカル変数の抽出](#341-step-1---ローカル変数の抽出)
    - [3.4.2. Step 2 - 変数利用への変更](#342-step-2---変数利用への変更)
    - [3.4.3. Step 3 - 関数導出](#343-step-3---関数導出)
    - [3.4.4. Step 4 - 関数利用への変更](#344-step-4---関数利用への変更)
    - [3.4.5. Step 5 - 新しい関数への切り替え](#345-step-5---新しい関数への切り替え)
    - [3.4.6. Step 6 - 旧関数の削除](#346-step-6---旧関数の削除)
    - [3.4.7. Step 7 - 関数のリネーム](#347-step-7---関数のリネーム)
    - [3.4.8. Step 8 - 新関数への切り替え](#348-step-8---新関数への切り替え)
    - [3.4.9. Step 9 - 旧関数のロジックを新関数へ移動](#349-step-9---旧関数のロジックを新関数へ移動)
    - [3.4.8. Step 8 - 旧関数の削除](#348-step-8---旧関数の削除)
    - [3.4.8. Step 9 - テストコードのメンテナンス](#348-step-9---テストコードのメンテナンス)
  - [3.5. 関数の統合](#35-関数の統合)
    - [3.5.1. Step 1 - ローカル変数の導出 1](#351-step-1---ローカル変数の導出-1)
    - [3.5.2. Step 2 - ローカル変数の導出 2](#352-step-2---ローカル変数の導出-2)
    - [3.5.3. Step 3 - 関数の導出](#353-step-3---関数の導出)
    - [3.5.4. Step 4 - 新関数の利用](#354-step-4---新関数の利用)
    - [3.5.5. Step 5 - テストコードを新関数に置き換える](#355-step-5---テストコードを新関数に置き換える)
    - [3.5.6. Step 6 - 旧関数の削除](#356-step-6---旧関数の削除)

<!-- /TOC -->

## 3.1. 関数導出

## 3.2. 関数のインライン化

## 3.3. 関数の仮引数の追加

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

オーバーロードのない JavaScript では、関数呼び出しに仮引数の数や型は関係ないため、追加は楽です。

### 3.3.1. Step 1 - 仮引数を追加する

``` js
// プロダクトコード
function adhoc(value=0) {   // <- デフォルト値付きで仮引数を追加します。
  return 0
}
```

### 3.3.2. Step 2 - 仮引数を使用する

``` js
// プロダクトコード gen2
function adhoc(value=0) {
  return value                // <-　仮引数を使うように変えます。
}
```

### 3.3.3. Step 3 - 呼び出し側に引数を追加する

``` js
// テストコード gen3
it('0 を返すこと', () => {
  expect(adhoc(0)).toBe(0)    // <- 呼び出す側に引数を追加します。
})
```

### 3.3.4. Step 4 - テストコードを追加する

adhoc 関数で引数を利用するコードに直したかどうか不安です。ミスがあるかもしれません。テストを追加して、三角測量が効くようにします。

``` js
// テストコード
it('0 を返すこと', () => {
  expect(adhoc(0)).toBe(0)
})
it('1 を返すこと', () => {      // <- 引数が使われていることを確認するテストを追加します。
  expect(adhoc(0)).toBe(1)    // <- エラーになるように書きます。
})
```

``` js
// テストコード gen5
it('0 を返すこと', () => {
  expect(adhoc(0)).toBe(0)
})
it('1 を返すこと', () => {
  expect(adhoc(1)).toBe(1)    // <- 正しい引数に直します。
})
```

### 3.3.5. Step 5 - 仮引数のデフォルト値を消す

``` js
function adhoc(value) {       // <-　デフォルト値を消します。
  return value
}
```

## 3.4. 関数の仮引数の変更

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

### 3.4.1. Step 1 - ローカル変数の抽出

``` js
// プロダクトコード
function square(size, unit) {
  const menseki = size * size       // <- 面積の計算式を変数に切り出します。
  return size * size + ' ' + unit
}
```

### 3.4.2. Step 2 - 変数利用への変更

``` js
// プロダクトコード
function square(size, unit) {
  const menseki = size * size
  return menseki + ' ' + unit     // <- 変数利用に書き換えます。
}
```

### 3.4.3. Step 3 - 関数導出

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

### 3.4.4. Step 4 - 関数利用への変更

``` js
// プロダクトコード
function square(size, unit) {
  return square_new(size) + ' ' + unit  // <- 関数利用に書き換えます
}
function square_new(size) {
  return size * size
}
```

### 3.4.5. Step 5 - 新しい関数への切り替え

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square_new(5) + ' ㎡')).toBe('25 ㎡')  // <- 新しい関数に置き換えます
})
```

### 3.4.6. Step 6 - 旧関数の削除

``` js
// プロダクトコード
// function square(size, unit) {           // <- 使わないので消します
//   return square_new(size) + ' ' + unit  // <-
// }                                       // <-
function square_new(size) {
  return size * size
}
```

### 3.4.7. Step 7 - 関数のリネーム

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

### 3.4.8. Step 8 - 新関数への切り替え

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square(5) + ' ㎡')).toBe('25 ㎡')  // <- 新しい関数に置き換えます
})
```

### 3.4.9. Step 9 - 旧関数のロジックを新関数へ移動

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

### 3.4.8. Step 8 - 旧関数の削除

``` js
// プロダクトコード
function square(size) {
  return size * sizeす
}
// function square_new(size) {  // <- 使わないので消します
//   return size * size         // <-
// }                            //
```

### 3.4.8. Step 9 - テストコードのメンテナンス

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square(5))).toBe('25')  // <- テストを適切な内容に変えます
})
```

この手順は、仮引数を削除する他に、順序を変えたり、仮引数名を変えたりといったケースでも同じ手順になります。追加の時も使えますが、 JavaScript の場合は手短な方を使うでしょう。

## 3.5. 関数の統合

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

### 3.5.1. Step 1 - ローカル変数の導出 1

```js
// プロダクトコード
function raise5(value) {
  const multiplier = 5                   // <- 直値をローカル変数にします
  return value * multiplier              // <-
}
function raise10(value) {
  return value * 10
}
```

### 3.5.2. Step 2 - ローカル変数の導出 2

```js
// プロダクトコード
function raise5(value) {
  const multiplier = 5
  const raise = value * multiplier      // <- 式をローカル変数にします
  return value * multiplier
}
function raise10(value) {
  return value * 10
}
```

### 3.5.3. Step 3 - 関数の導出

```js
// プロダクトコード
function raise(value, multiplier) {     // <- 式の部分を外部関数にします
  return value * multiplier             // <-
}                                       // <-
function raise5(value) {
  const multiplier = 5
  const raise = value * multiplier
  return value * multiplier
}
function raise10(value) {
  return value * 10
}
```

### 3.5.4. Step 4 - 新関数の利用

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  const multiplier = 5
  const raise = value * multiplier
  return raise(value, multiplier)       // <- 追加した関数に置き換えます。
}
function raise10(value) {
  return raise(value, 10)               // <- 追加した関数に置き換えます。
}
```

旧関数内で新関数を呼んでテストが通れば、新関数の振る舞いを信用できます。そうすれば、安心してテストコードで新関数を利用できます。

### 3.5.5. Step 5 - テストコードを新関数に置き換える

```js
// テストコード
it('5 倍になること', () => {
  expect(raise(2, 5)).toBe(10)          // <- 新しい関数への置き換えます
})
it('10 倍になること', () => {
  expect(raise(2, 10)).toBe(20)         // <- 新しい関数への置き換えます
})
```

### 3.5.6. Step 6 - 旧関数の削除

```js
// プロダクトコード gen_04
function raise(value, multiplier) {
  return value * multiplier
}
// function raise5(value) {           // <- 使わなくなった関数を削除します
//   const multiplier = 5             // <-
//   return raise(value, multiplier)  // <-
// }                                  // <-
// function raise10(value) {          // <-
//   return raise(value, 10)          // <-
// }                                  // <-
```

---

&copy; 2020 Tommy@degino Inc.
