## 2. 中級

### 2.1. 関数導出

### 2.2. 関数のインライン化

### 2.3. 関数の仮引数の追加

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

``` js
// プロダクトコード gen1
function adhoc(value=0) { // <- 仮引数をデフォルト付きで追加します。
  return 0
}
```

``` js
// プロダクトコード gen2
function adhoc(value=0) {
  return value            // <-　仮引数を使うように変えます。
}
```

``` js
// テストコード gen3
it('0 を返すこと', () => {
  expect(adhoc(0)).toBe(0)  // <- 呼び出す側に引数を追加します。
})
```

``` js
// テストコード gen4
it('0 を返すこと', () => {
  expect(adhoc(0)).toBe(0)
})
it('1 を返すこと', () => {    // <- 引数が使われていることを確認するテストを追加します。
  expect(adhoc(0)).toBe(1)  // <- エラーになるコードを書きます。
})
```

``` js
// テストコード gen5
it('0 を返すこと', () => {
  expect(adhoc(0)).toBe(0)
})
it('1 を返すこと', () => {    // <- 引数が使われていることを確認するテストを追加します。
  expect(adhoc(1)).toBe(1)  // <- 通れば引数が影響していることが確認できます。
})
```

``` js
// プロダクトコード gen6
function adhoc(value) {     // <-　デフォルト値を消します。
  return value
}
```

### 2.4. 関数の仮引数の変更

仮引数を削除したり、順序を変えたり、仮引数名を変えたり…と万能で使える手順です。追加の時も使えます。関数名の変更と同じ手順です。

``` js
// テストコード org
it('正方形の面積を求める', () => {
  expect(space(5, 5)).toBe(25)
})
```

``` js
// プロダクトコード org
function space(width, height) {
  return width * height
}
```

