# 2.5. 関数の導出 - 依存変数の切り離し

<!-- TOC -->

- [2.5. 関数の導出 - 依存変数の切り離し](#25-関数の導出---依存変数の切り離し)
  - [Step 0 - 最初のコード](#step-0---最初のコード)
  - [Step 1 - 一時変数の作成](#step-1---一時変数の作成)
  - [Step 2 - 一時変数の利用](#step-2---一時変数の利用)
  - [Step 3 - 変数のインライン化](#step-3---変数のインライン化)
  - [Step 4 - 不使用部分の削除](#step-4---不使用部分の削除)
  - [Step 5 - 関数の導出](#step-5---関数の導出)
  - [Step 6 - 新規関数の利用](#step-6---新規関数の利用)
  - [Step 7 - 不使用箇所の削除](#step-7---不使用箇所の削除)

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

[FizzBuzz](https://ja.wikipedia.org/wiki/Fizz_Buzz) は、この後、判定を増やしてゆくことになりますが、ループ処理の中にある if 文が複雑になりそうです。今のうちに、判定処理を独立した関数にしましょう。

## Step 1 - 一時変数の作成

if 文を外部関数にしたいのですが、ループのにある ret 変数に依存しているため、単純には関数化できません。先にそれを解決するため、一時変数を使って ret から切り離すことにします。

```js
// プロダクトコード
function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    let temp = null                       // <- 一時変数を作成します
    if (i===3) {
      ret[i-1] = 'Fizz'
      temp = ret[i-1]                     // <- 一時変数に代入します
    } else {
      ret[i-1] = String(i)
      temp = ret[i-1]                     // <- 一時変数に代入します
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
      ret[i-1] = 'Fizz'
      temp = ret[i-1]
    } else {
      ret[i-1] = String(i)
      temp = ret[i-1]
    }
    ret[i-1] = temp                     // <- 一次変数を使用します
  }

  return ret
}
```

## Step 3 - 変数のインライン化

```js
// プロダクトコード
function fizzbuzz() {
  const ret = []
  for (let i=1; i<=3; i++) {
    let temp = null
    if (i===3) {
      ret[i-1] = 'Fizz'
      temp = 'Fizz'                   // <- インライン化します
    } else {
      ret[i-1] = String(i)
      temp = String(i)                // <- インライン化します
    }
    ret[i-1] = temp
  }

  return ret
}
```

## Step 4 - 不使用部分の削除

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

これで if 分は ret から切り離されましたので、外部関数化にできます。

## Step 5 - 関数の導出

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
    return temp                         // <- 新規作成を作成します
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

## Step 6 - 新規関数の利用

```js
// プロダクトコード
function hantei(i) {
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

## Step 7 - 不使用箇所の削除

```js
// プロダクトコード
function hantei(i) {
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
