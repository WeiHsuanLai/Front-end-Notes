在 JavaScript 中，**Number** 只有一種數值型別，它採用的是 **IEEE 754 雙精度 64 位元浮點數**格式，所以精確度是介於 `-(2^53 − 1)` 與 `2^53 − 1` 之間。
這意味著在 JS 中，整數與浮點數（小數）在本質上是相同的。
而在這個範圍之外，就會有精準度的問題，這時候要用另一個原生值 `BigInt`。
`+Infinity`, `-Infinity`, 與  [NaN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN) 都是 `number` 這個型別，所以我們用 `typeof` 來檢查的話，會得到 `number`。

```
console.log(typeof NaN); // number
```

`NaN`（Not a Number）在 JavaScript 中是一個特殊的數值，屬於 `Number` 型別。它通常出現在**數學運算無法產生有意義的數字結果**時。

#### 1.失敗的型別轉換

當你嘗試將一個非數字的字串、物件或 `undefined` 強制轉換為數字，但該內容不符合數字格式時。

```
const result = Number("Hello"); // NaN
const age = parseInt("abc");    // NaN
const value = undefined + 5;    // NaN
```

#### 2.不確定的數學運算

某些數學運算在邏輯上沒有定義，或結果不是實數。

```
console.log(Math.sqrt(-1));  // NaN (負數開平方根)
console.log(0 / 0);          // NaN (零除以零)
console.log(Infinity - Infinity); // NaN
console.log(Infinity / Infinity); // NaN
```

#### 3. 字串與數字的錯誤運算

除了加法（`+`）會變成字串接合外，其餘算術運算子（`-`, `*`, `/`）若遇到無法轉換的字串，會產生 `NaN`。

```
console.log("apple" * 10); // NaN
console.log("100px" - 50); // NaN (注意：parseInt("100px") 是 100，但直接算會是 NaN)
```

#### 4. 如何偵測 NaN？

這是在開發中最常需要處理 `NaN` 的時刻。因為 `NaN` 是 JS 中唯一一個「不等於自己」的值，你不能用 `if (x === NaN)` 來判斷。

##### 常用的判斷方式：

1. **`Number.isNaN(value)`** (推薦)： 最準確的方法，只有當值真的是 `NaN` 時才回傳 `true`。
2. **`isNaN(value)`** (舊方法)： 會先嘗試把參數轉成數字再判斷，有時會誤判（例如 `isNaN("hello")` 會是 `true`）。

#### 5.特殊數值 (Special Values)

在處理數字時，這三個特殊值非常重要，開發中經常需要判斷它們：

- **`NaN`**：Not a Number，表示運算失敗或無意義（如 `0 / 0`）。
    
- **`Infinity` / `-Infinity`**：正負無窮大（如 `1 / 0`）。
    
- **`Number.MAX_SAFE_INTEGER`**：最大安全整數 ($2^{53} - 1$)，超過此值的整數運算可能會失去精確度。

#### 6.常用靜態方法 (Static Methods)

靜態方法是指直接透過 `Number.xxx()` 呼叫的方法，主要用於**判斷**與**型別轉換**。

| 方法                  | 功能                            | 範例                                |
| ------------------- | ----------------------------- | --------------------------------- |
| Number.isNaN()      | 判斷是否為 `NaN` (比全域 `isNaN` 更嚴謹) | Number.isNaN(NaN) // true         |
| Number.isFinite()   | 判斷是否為有限數字                     | Number.isFinite(100) // true      |
| Number.isInteger()  | 判斷是否為整數                       | `Number.isInteger(1.5) // false`  |
| Number.parseInt()   | 解析字串並回傳整數                     | Number.isInteger(1.5) // false    |
| Number.parseFloat() | 解析字串並回傳浮點數                    | Number.parseFloat("10.5") // 10.5 |
#### 7.常用實例方法 (Instance Methods)

實例方法用於對已存在的數字進行**格式化**，它們通常會回傳**字串**。

| 方法             | 功能                   | 範例                                |
| -------------- | -------------------- | --------------------------------- |
| toFixed(n)     | 四捨五入到小數點後 n 位 (回傳字串) | (3.1415).toFixed(2) // "3.14"     |
| toPrecision(n) | 格式化為指定精確度 (總長度)      | `(12.34).toPrecision(2) // "12"`  |
| toString(base) | 轉為字串，可指定進位制 (2~36)   | `(10).toString(16) // "a"` (十六進位) |
|                |                      |                                   |
#### 8.數學運算工具：Math 物件

雖然 `Math` 不是 `Number` 的方法，但處理數字絕對離不開它。

- **`Math.round(x)`**：四捨五入。
    
- **`Math.ceil(x)`**：無條件進位。
    
- **`Math.floor(x)`**：無條件捨去。
    
- **`Math.abs(x)`**：取絕對值。
    
- **`Math.random()`**：產生 0 到 1 之間的隨機浮點數。

## *必記重點：0.1 + 0.2 難題*

```
console.log(0.1 + 0.2 === 0.3); // false
console.log(0.1 + 0.2);         // 0.30000000000000004
```
**解決方法：** 在進行金錢計算或需要精確結果時，通常會使用 `toFixed()` 處理後再轉回數字，或改用整數單位（如：以「分」代替「元」）進行運算。

#### 9. 快速轉換技巧

| 技巧       | 說明           | 範例                   |
| -------- | ------------ | -------------------- |
| 一元加號 `+` | 最快將字串轉為數字的方法 | +"123" // 123        |
| Number() | 強制轉型         | Number("456") // 456 |
