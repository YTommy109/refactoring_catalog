# 3. 実例 - JavaScript

<!-- TOC -->

- [3. 実例 - JavaScript](#3-実例---javascript)
  - [3.1. FizzBuzz - 1](#31-fizzbuzz---1)
    - [3.1.1. Step 1](#311-step-1)
    - [3.1.2. Step 2](#312-step-2)
    - [3.1.3. Step 3](#313-step-3)
    - [3.1.4. Step 4](#314-step-4)
    - [3.1.5. Step 5](#315-step-5)
    - [3.1.6. Step 6](#316-step-6)
    - [3.1.7. Step 7](#317-step-7)
    - [3.1.8. Step 8](#318-step-8)

<!-- /TOC -->

## 3.1. FizzBuzz - 1

[FizzBuzz](https://ja.wikipedia.org/wiki/Fizz_Buzz) のうち、 1 から 3 まで動作するコードがあります。

```js
// テストコード
it('', () => {
  expect(hoge()).toEqual(['1', '2', 'Fizz'])
})

// プロダクトコード
function hoge() {
  const ret = []
  for (let i=1; i<=3; i++) {
    ret.push(i===3 ? 'Fizz' : String(i))
  }

  return ret
}
```

十分に短いコードではありますが、ループの中に分岐 (参考演算子) 処理があり、不吉な匂いを感じます。なんとなく、このまま処理を追加してゆくことに不安を感じるなら、リファクタリングした方が良いでしょう。

### 3.1.1. Step 1

まずは、 **関数の導出** を使って、分岐 (三項演算子) のところをループの外に出します。

```js
// プロダクトコード
function fizz(v) {                    // <- 関数を追加します
  return v===3 ? 'Fizz' : String(v)   // <-
}                                     // <-

function hoge() {
  const ret = []
  for (let i=1; i<=3; i++) {
    ret.push(i===3 ? 'Fizz' : String(i))
  }

  return ret
}
```

### 3.1.2. Step 2

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  const ret = []
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))                 // <- 追加した関数に置き換えます
  }

  return ret
}
```

### 3.1.3. Step 3

ループ処理は for で回すより、 map を使った方がシンプルに記述できます。変えましょう。

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  let ret = []                      // <- let に変更します
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))
  }

  return ret
}
```

### 3.1.4. Step 4

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  let ret = []
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))
  }
  ret = [1,2,3].map(fizz)           // <- ret を map で上書きします

  return ret
}
```

### 3.1.5. Step 5

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  let ret = []
  // for (let i=1; i<=3; i++) {     // <- 消します
  //   ret.push(fizz(i))            // <-
  // }                              // <-
  ret = [1,2,3].map(fizz)

  return ret
}
```

### 3.1.6. Step 6

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  let ret = []

  ret = [1,2,3].map(fizz)

  return [1,2,3].map(fizz)        // <- 変数のインライン化をします
}
```

### 3.1.7. Step 7

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  // let ret = []                  // <- 消します

  // ret = [1,2,3].map(fizz)       // <- 消します

  return [1,2,3].map(fizz)
}
```

### 3.1.8. Step 8

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  return [1,2,3].map(fizz)
}
```

最初のに比べて、かなりすっきりしました。
