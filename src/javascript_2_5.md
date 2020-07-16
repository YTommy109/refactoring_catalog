# 2.5. 関数の導出 - 依存変数の局所化

<!-- TOC -->

- [2.5. 関数の導出 - 依存変数の局所化](#25-関数の導出---依存変数の局所化)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - 一時変数の作成](#step-1---一時変数の作成)
  - [Step 2 - 一時変数の利用](#step-2---一時変数の利用)
  - [Step 3 - 不使用部分の削除](#step-3---不使用部分の削除)
  - [Step 4 - 関数の導出](#step-4---関数の導出)
  - [Step 5 - 新規関数の利用](#step-5---新規関数の利用)
  - [Step 6 - 不使用箇所の削除](#step-6---不使用箇所の削除)

<!-- /TOC -->

## Step 0 - 最初のコード

```js
// テストコード
it('1から3までの FizzBuzz を取れること', () => {
  expect(fizzbuzz(), [1, 2, 'Fizz'])
})
```

```js
// プロダクトコード
function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    if (i===3) {
      ret[i-1] = 'Fizz'
    } else {
      ret[i-1] = String(i)
    }
  }

  return ret
}
```

## Step 1 - 一時変数の作成

```js
// プロダクトコード
function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    let temp = null                     // <- 一時変数を作成します
    if (i===3) {
      temp = 'Fizz'                     // <- 一時変数に代入します
      ret[i-1] = 'Fizz'
    } else {
      temp = String(i)                  // <- 一時変数に代入します
      ret[i-1] = String(i)
    }
  }

  return ret
}
```

## Step 2 - 一時変数の利用

```js
// プロダクトコード
function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    let temp = null
    if (i===3) {
      temp = 'Fizz'
      ret[i-1] = 'Fizz'
    } else {
      temp = String(i)
      ret[i-1] = String(i)
    }
    ret[i-1] = temp                     // <- 一次変数を使用します
  }

  return ret
}
```

## Step 3 - 不使用部分の削除

```js
// プロダクトコード
function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    let temp = null
    if (i===3) {
      temp = 'Fizz'
      // ret[i-1] = 'Fizz'              // <- 使わなくなったので消します
    } else {
      temp = String(i)
      // ret[i-1] = String(i)           // <- 使わなくなったので消します
    }
    ret[i-1] = temp
  }

  return ret
}
```

## Step 4 - 関数の導出

```js
// プロダクトコード
function hantei(i) {                    // <- 新規作成を作成します
    let temp = null                     // <- コピペ
    if (i===3) {                        // <- コピペ
      temp = 'Fizz'                     // <- コピペ
    } else {                            // <- コピペ
      temp = String(i)                  // <- コピペ
    }                                   // <- コピペ
                                        // <- コピペ
    return temp                         // <- コピペ
}                                       // <- 新規作成を作成します

function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    let temp = null
    if (i===3) {
      temp = 'Fizz'
    } else {
      temp = String(i)
    }
    ret[i-1] = temp
  }

  return ret
}
```

## Step 5 - 新規関数の利用

```js
// プロダクトコード
function hantei() {
    let temp = null
    if (i===3) {
      temp = 'Fizz'
    } else {
      temp = String(i)
    }

    return temp
}

function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    let temp = null
    if (i===3) {
      temp = 'Fizz'
    } else {
      temp = String(i)
    }
    ret[i-1] = hantei(i)                // <- 作成関数を利用する
  }

  return ret
}
```

## Step 6 - 不使用箇所の削除

```js
// プロダクトコード
function hantei() {
    let temp = null
    if (i===3) {
      temp = 'Fizz'
    } else {
      temp = String(i)
    }

    return temp
}

function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    // let temp = null                  // <- 使わなくなったところを消します
    // if (i===3) {                     // <-
    //   temp = 'Fizz'                  // <-
    // } else {                         // <-
    //   temp = String(i)               // <-
    // }                                // <-
    ret[i-1] = hantei(i)
  }

  return ret
}
```
