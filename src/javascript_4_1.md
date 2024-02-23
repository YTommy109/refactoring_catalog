# 4.1. FizzBuzz - 1

<!-- TOC -->

- [4.1. FizzBuzz - 1](#41-fizzbuzz---1)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1](#step-1)
  - [Step 2](#step-2)
  - [Step 3](#step-3)
  - [Step 4](#step-4)
  - [Step 5](#step-5)
  - [Step 6](#step-6)
  - [Step 7](#step-7)
  - [Step 8](#step-8)

<!-- /TOC -->

## Step 0 - 最初のコード

[FizzBuzz](https://ja.wikipedia.org/wiki/Fizz_Buzz) のうち、 1 から 3 まで動作するコードがあります。

```js
// テストコード
it('', () => {
  expect(main()).toEqual(['1', '2', 'Fizz'])
})

// プロダクトコード
function main() {
  const ret = []
  for (let i=1; i<=3; i++) {
    ret.push(i===3 ? 'Fizz' : String(i))
  }

  return ret
}
```

十分に短いコードではありますが、ループの中に分岐処理 (三項演算子) があり、不吉な匂いを感じます。なんとなく、このまま処理を追加してゆくことに不安を感じるので、リファクタリングした方が良いでしょう。

## Step 1

まずは、 **関数の導出** を使って、分岐 (三項演算子) のところをループの外に出します。

```js
// プロダクトコード
function fizz(v) {                    // <- 関数を追加します
  return v===3 ? 'Fizz' : String(v)   // <-
}                                     // <-

function main() {
  const ret = []
  for (let i=1; i<=3; i++) {
    ret.push(i===3 ? 'Fizz' : String(i))
  }

  return ret
}
```

## Step 2

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function main() {
  const ret = []
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))                 // <- 追加した関数に置き換えます
  }

  return ret
}
```

## Step 3

ループ処理は for で回すより、 map を使った方がシンプルに記述できます。変えましょう。

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function main() {
  let ret = []                      // <- let に変更します
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))
  }

  return ret
}
```

## Step 4

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function main() {
  let ret = []
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))
  }
  ret = [1,2,3].map(fizz)           // <- ret を map で上書きします

  return ret
}
```

## Step 5

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function main() {
  let ret = []
  // for (let i=1; i<=3; i++) {     // <- 消します
  //   ret.push(fizz(i))            // <-
  // }                              // <-
  ret = [1,2,3].map(fizz)

  return ret
}
```

## Step 6

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function main() {
  let ret = []

  ret = [1,2,3].map(fizz)

  return [1,2,3].map(fizz)        // <- 変数のインライン化をします
}
```

## Step 7

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function main() {
  // let ret = []                  // <- 消します

  // ret = [1,2,3].map(fizz)       // <- 消します

  return [1,2,3].map(fizz)
}
```

## Step 8

```js
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function main() {
  return [1,2,3].map(fizz)
}
```

最初のに比べて、かなりすっきりしました。

---

&copy; 2020 Tommy@[Degino Inc.](https://www.degino.com/)
