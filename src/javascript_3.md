# 3. 中級 - JavaScript

<!-- TOC -->

- [3. 中級 - JavaScript](#3-中級---javascript)
  - [3.1. 関数導出](#31-関数導出)
  - [3.2. 関数のインライン化](#32-関数のインライン化)
  - [3.3. 関数の仮引数の追加](#33-関数の仮引数の追加)
    - [3.3.1. Step 1](#331-step-1)
    - [3.3.2. Step 2](#332-step-2)
    - [3.3.3. Step 3](#333-step-3)
    - [3.3.4. Step 4](#334-step-4)
    - [3.3.5. Step 5](#335-step-5)
  - [3.4. 関数の仮引数の変更](#34-関数の仮引数の変更)
    - [3.4.1. Step 1](#341-step-1)
    - [3.4.2. Step 2](#342-step-2)
    - [3.4.3. Step 3](#343-step-3)
    - [3.4.4. Step 4](#344-step-4)
    - [3.4.5. Step 5](#345-step-5)
    - [3.4.6. Step 6](#346-step-6)
    - [3.4.7. Step 7](#347-step-7)
    - [3.4.8. Step 8](#348-step-8)
    - [3.4.8. Step 9](#348-step-9)
  - [3.5. 関数の統合](#35-関数の統合)
    - [3.5.1. Step 1](#351-step-1)
    - [3.5.2. Step 2](#352-step-2)
    - [3.5.3. Step 3](#353-step-3)
    - [3.5.4. Step 4](#354-step-4)
    - [3.5.5. Step 5](#355-step-5)

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

仮引数の型や数は関数呼び出しに関係ないため、追加は楽です。

### 3.3.1. Step 1

``` js
// プロダクトコード
function adhoc(value=0) {   // <- 仮引数をデフォルト付きで追加します。
  return 0
}
```

### 3.3.2. Step 2

``` js
// プロダクトコード gen2
function adhoc(value=0) {
  return value                // <-　仮引数を使うように変えます。
}
```

### 3.3.3. Step 3

``` js
// テストコード gen3
it('0 を返すこと', () => {
  expect(adhoc(0)).toBe(0)    // <- 呼び出す側に引数を追加します。
})
```

### 3.3.4. Step 4

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

### 3.3.5. Step 5

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

### 3.4.1. Step 1

``` js
// プロダクトコード
function square(size, unit) {
  const menseki = size * size       // <- 面積の計算式を変数に切り出します。
  return size * size + ' ' + unit
}
```

``` js
// プロダクトコード
function square(size, unit) {
  const menseki = size * size
  return menseki + ' ' + unit     // <- 変数利用に書き換えます。
}
```

### 3.4.2. Step 2

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

``` js
// プロダクトコード
function square(size, unit) {
  return square_new(size) + ' ' + unit  // <- 関数利用に書き換えます
}
function square_new(size) {
  return size * size
}
```

### 3.4.3. Step 3

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square_new(5) + ' ㎡')).toBe('25 ㎡')  // <- 新しい関数に置き換えます
})
```

### 3.4.4. Step 4

``` js
// プロダクトコード
// function square(size, unit) {           // <- 使わないので消します
//   return square_new(size) + ' ' + unit  // <-
// }                                       // <-
function square_new(size) {
  return size * size
}
```

### 3.4.5. Step 5

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

### 3.4.6. Step 6

``` js
// テストコード
it('正方形の面積を求める', () => {
  expect(square(5) + ' ㎡')).toBe('25 ㎡')  // <- 新しい関数に置き換えます
})
```

### 3.4.7. Step 7

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

### 3.4.8. Step 8

square_new は一時的な名称ですので、好ましい名称に変えます。

``` js
// プロダクトコード
function square(size) {
  return size * sizeす
}
// function square_new(size) {  // <- 使わないので消します
//   return size * size         // <-
// }                            //
```

### 3.4.8. Step 9

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

### 3.5.1. Step 1

```js
// プロダクトコード
function raise5(value) {
  const multiplier = 5                   // <- ローカル変数の導出
  return value * multiplier              // <-
}
function raise10(value) {
  return value * 10
}
```

### 3.5.2. Step 2

```js
// プロダクトコード
function raise(value, multiplier) {     // <- 関数の導出
  return value * multiplier             // <-
}                                       // <-
function raise5(value) {
  const multiplier = 5
  return value * multiplier
}
function raise10(value) {
  return value * 10
}
```

### 3.5.3. Step 3

```js
// プロダクトコード
function raise(value, multiplier) {
  return value * multiplier
}
function raise5(value) {
  const multiplier = 5
  return raise(value, multiplier)       // <- 追加した関数に置き換えます。
}
function raise10(value) {
  return riase(value, 10)               // <- 追加した関数に置き換えます。
}
```

### 3.5.4. Step 4

```js
// テストコード
it('5 倍になること', () => {
  expect(raise(2, 5)).toBe(10)          // <- 新しい関数への置き換えます
})
it('10 倍になること', () => {
  expect(raise(2, 10)).toBe(20)         // <- 新しい関数への置き換えます
})
```

### 3.5.5. Step 5

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
//   return riase(value, 10)          // <-
// }                                  // <-
```

---

&copy; 2020 Tommy@degino Inc.
