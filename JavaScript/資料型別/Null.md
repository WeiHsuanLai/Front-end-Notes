`null` 是很容易跟 `undefined` 搞混的原生值。`undefined` 是因為某變數還沒有賦值，所以對 JavaScript 來說，它不知道該變數的值是什麼，所以要讀取該變數時，會是 `undefined`。不過 `null` 則是我們賦予某個變數 `null` 這一個值。

#### 1. 什麼時候會用到 Null？
**初始化變數**：當你預期一個變數之後會儲存一個「物件」，但現在還沒有時。

```
let currentUser = null; // 現在還沒人登入
```

- **清空物件引用**：當一個物件不再需要時，可以手動將其設為 `null`，這有助於垃圾回收（Garbage Collection）機制理解該物件不再被使用。
    
- **API 的回傳值**：當查詢結果為空時（例如在資料庫找不到該 ID 的資料），後端或方法通常會回傳 `null`。

```
const element = document.getElementById('non-existent-id'); 
console.log(element); // null
```

#### 2. Null 的特殊型別：一個古老的 Bug
這是 JS 筆記中必考的冷知識：
```
console.log(typeof null); // "object"
```

這其實是 JavaScript 最初設計時的一個 **Bug**。在 JS 的底層標籤中，物件的類型標籤是 `0`，而 `null` 代表空指標，剛好也被表示為全零，因此 `typeof` 錯誤地將其判斷為 `"object"`。雖然之後有人提議修正，但為了不破壞現有的網站邏輯，這個 Bug 就一直沿用至今。

#### 3. Null vs Undefined 比較表

| 特性            | Undefined       | Null                           |
| ------------- | --------------- | ------------------------------ |
| 意義            | 「缺少值」（變數還沒準備好）  | 「空值」（刻意人為設定為空）                 |
| 型別 (`typeof`) | `"undefined"`   | **`"object"`** (這是 JS 的經典 Bug) |
| 轉為數字          | NaN             | 0                              |
| 出現方式          | 通常是系統自動產生       | 通常是開發者手動賦值                     |
| 轉為布林          | `false` (Falsy) | `false` (Falsy)                |

#### 4. 判斷技巧與運算子

##### **A. 嚴格相等 (`===`)**

要精確判斷一個值是否為 `null`，必須使用 `===`，因為 `==` 會把 `null` 跟 `undefined` 混為一談。

```
const value = null;

console.log(value == undefined);  // true (寬鬆相等會判定兩者相似)
console.log(value === undefined); // false (嚴格相等才安全)
console.log(value === null);      // true
```

##### **B. 空值合併運算子 (`??`)**

當你想在值是 `null` 或 `undefined` 時提供預設值，可以使用 `??`。

```
let userPreference = null;
const theme = userPreference ?? "dark"; // "dark"
```

#### 5. 數學運算中的 Null
在數學運算中，`null` 會被強制轉型為 **0**。

```
console.log(10 + null); // 10
console.log(5 * null);  // 0
```

**筆記提醒**：這跟 `undefined` 完全不同，`10 + undefined` 會得到 `NaN`。

### 💡 筆記小叮嚀：

- **語意化建議**：如果你想表達「這個欄位目前是空的」，請用 `null`。
    
- **DOM 操作**：在網頁開發中，如果抓不到 HTML 元素，回傳的一定是 `null`，這是一個很好的判斷基準點。