## 1.3. 応用/実例

### 1.3.1. 名称未設定

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

```js
// プロダクトコード gen1
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
// プロダクトコード gen1
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
// プロダクトコード gen2
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  const ret = []
  const ret2 = [1,2,3].map(fizz)  // <- map 版を作る
  for (let i=1; i<=3; i++) {
    ret.push(fizz(i))
  }

  return ret2                     // ret2 に着替える
}
```

```js
// プロダクトコード gen3
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  // const ret = []               // <- 消す
  const ret2 = [1,2,3].map(fizz)
  // for (let i=1; i<=3; i++) {   // <- 消す
  //  ret.push(fizz(i))           //
  // }                            //

  return ret2
}
```

```js
// プロダクトコード gen3
function fizz(v) {
  return v===3 ? 'Fizz' : String(v)
}

function hoge() {
  // const ret2 = [1,2,3].map(fizz) // <- 消す

  return [1,2,3].map(fizz)          // <- インライン化する
}
```
