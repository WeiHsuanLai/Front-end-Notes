字串是最常見的原生值之一。如前面提到，在 JavaScript 當中字串本身是不可變的。當我們用 `substring()` 來擷取字串，或用 `concat()` 來把兩個字串合為一，這些都是會回傳另一個字串，而非改變原本的字串。

### 1. 字串的宣告方式

在 JS 中有三種常見的宣告方式：

- **單引號/雙引號**：`'Hello'` 或 `"Hello"`，兩者功能相同。
    
- **樣板字面值 (Template Literals)**：使用反引號 **`**。支援**多行字串**與**嵌入變數** `${expression}`。

```
const name = "Gemini";
// 樣板字面值範例
const greeting = `Hello, 
my name is ${name}.`;
```

### 2. 常用字串方法分類
#### A. 搜尋與檢查

用於確認字串中是否包含特定內容。

| 方法              | 功能                 | 範例                              |
| --------------- | ------------------ | ------------------------------- |
| includes(str)   | 是否包含子字串 (回傳布林)     | 'Apple'.includes('A') // true   |
| indexOf(str)    | 找子字串的位置 (找不到回傳 -1) | 'Apple'.indexOf('p') // 1       |
| startsWith(str) | 是否以特定字串開頭          | 'Hello'.startsWith('H') // true |
#### B. 擷取與修剪

用於取得字串的一部分或處理空格。

| 方法                | 功能                | 範例                                    |
| ----------------- | ----------------- | ------------------------------------- |
| slice(start, end) | 擷取範圍內的字串 (不含 end) | 'JavaScript'.slice(0, 4) // "Java"    |
| split(sep)        | 將字串切分成**陣列**      | 'A,B,C'.split(',') // ["A", "B", "C"] |
| trim()            | 移除頭尾的所有空格         | ' hi '.trim() // "hi"                 |
#### C. 轉換與替換

用於改變字串的外觀或內容。

| 方法                | 功能          | 範例                                      |
| ----------------- | ----------- | --------------------------------------- |
| toUpperCase()     | 全部轉大寫       | 'apple'.toUpperCase() // "APPLE"        |
| toLowerCase()     | 全部轉小寫       | 'APPLE'.toLowerCase() // "apple"        |
| replace(old, new) | 替換第一個匹配到的字串 | 'blue pen'.replace('blue', 'red')       |
| replaceAll()      | 替換所有匹配到的字串  | '1-2-3'.replaceAll('-', '/') // "1/2/3" |


### 3. 重要特性：不可變性 (Immutability)

這是一個新手常犯的錯誤：認為呼叫方法會改變原變數。
```
let text = "hello";
text.toUpperCase(); // 這行執行完，text 依然是 "hello"

// 正確做法：必須重新賦值
text = text.toUpperCase(); 
console.log(text); // "HELLO"
```

### 4. 實務小技巧：字串長度與索引

- **長度**：使用 `.length` 屬性（注意：它是屬性不是方法，不用加括號）。
    
- **存取字元**：可以使用陣列下標 `str[0]` 或 `str.charAt(0)`。

```
const str = "Code";
console.log(str.length); // 4
console.log(str[0]);      // "C"
```

