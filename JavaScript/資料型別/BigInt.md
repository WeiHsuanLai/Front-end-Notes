在 JavaScript 的整數與浮點數，都是用 `number` 這個型別，這其實只說了一半。因為 JavaScript 的 `number` 精準度有其限制，雖然多數情況很夠用 (`2^53 - 1` 會是 9007199254740991，我們很少用到比這大的數)。但有些時候會需要更往下的精準度。這時就可以用 `BigInt` 數值的型別。

`BigInt` 可以讓我們任意選擇其精準度，就可以避免一些 `number` 會遇到的問題。它跟 `number` 一樣可以用 `+`, `*`, `-`, `**`, 與  `%` 等運算子，不過要注意不可以拿 `BigInt` 跟 `number` 型別的值交互使用，這會出現 `TypeError` 。

### 1. 為什麼需要 BigInt？

JavaScript 的 `Number` 遵循 IEEE 754 標準，最大安全整數是：

$$Number.MAX\_SAFE\_INTEGER = 2^{53} - 1 = 9,007,199,254,740,991$$

超過這個數字，計算就會開始出現誤差。`BigInt` 則可以表示**任意長度**的整數。

### 2. 宣告方式

有兩種方式可以建立 `BigInt`：

1. 在整數末尾加上一個小寫的 **`n`**。
2. 使用 **`BigInt()`** 建構函式。

```
const bigIntNum = 9007199254740991n;
const bigIntFromStr = BigInt("9007199254740991");
const bigIntFromNum = BigInt(10);
```

### 3. 運算規則與限制

這是 BigInt 在使用時最容易踩坑的地方:

##### A. 不可與 Number 混用運算
你不能直接把 `BigInt` 和 `Number` 放在一起做算術運算，這會拋出 `TypeError`。

```
const big = 10n;
const num = 5;

// console.log(big + num); // ❌ 報錯
console.log(big + BigInt(num)); // ✅ 轉型後運算：15n
```

##### B. 除法會「無條件捨去」
由於 `BigInt` 只能存儲整數，除法運算不會產生小數點。

```
console.log(5n / 2n); // 2n (不是 2.5n)
```

##### C. 比較運算
雖然不能加減，但 `BigInt` 與 `Number` 可以進行大小比較和寬鬆相等比較。

```
console.log(10n > 5);   // true
console.log(10n == 10);  // true (寬鬆相等)
console.log(10n === 10); // false (型別不同：BigInt vs Number)
```

### 4. 常用方法與特性

| 特性/方法                  | 說明                                       |
| ---------------------- | ---------------------------------------- |
| typeof                 | 回傳 `"bigint"`。                           |
| toString()             | 轉為字串 (不帶 `n`)。                           |
| BigInt.asIntN(bits, n) | 將 BigInt 限制在指定的位元長度內（有符號）。               |
| JSON 限制                | `JSON.stringify()` 預設不支援 BigInt，需手動轉成字串。 |
### 5. 實務小叮嚀

- **不要用來處理小數**：BigInt 只能處理整數。
- **效能考量**：BigInt 的運算速度比一般 Number 慢，除非真的需要超大數字，否則優先使用 Number。
- **Math 物件不可用**：`Math.max()`、`Math.sqrt()` 等方法不支援 BigInt。

| 特性   | Number                 | BigInt  |
| ---- | ---------------------- | ------- |
| 型別   | number                 | bigint  |
| 宣告   | 10                     | 10n     |
| 小數   | 支援                     | 不支援     |
| 範圍限制 | $2^{53}-1$             | 僅受限於記憶體 |
| 嚴格相等 | `10 === 10n` 為 `false` |         |
