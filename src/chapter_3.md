# 3. 応用/実例

## 3.1. 名称未設定

```js
// Origin
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

```js
// gen 1
// プロダクトコード
function fizz(v) {                    // <- 関数を追加する
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

```js
// gen 2
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  const ret = []
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))             // <- 関数に置き換える
  }

  return ret
}
```

```js
// gen 3
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  let ret = []                      // <- const から let に変更した
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))
  }
  ret = [1,2,3].map(fizz)           // <- for ループと同じ処理を map で上書きした

  return ret
}
```

```js
// gen 4
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  let ret = []
  // for (let i=1; i<=3; i++) {     // <- 消した
  //   ret.push(fizz(i))            // <-
  // }                              // <-
  ret = [1,2,3].map(fizz)

  return ret
}
```

```js
// gen 5
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  let ret = [1,2,3].map(fizz)       // <- 1行にした

  return ret
}
```

```js
// gen 6
// プロダクトコード
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  // let ret = [1,2,3].map(fizz)    // <- 消した

  return [1,2,3].map(fizz)          // <- 1行にした
}
```
