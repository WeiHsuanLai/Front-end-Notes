`undefined` 是一個型別，它本身也是一個值。
假如某個變數還沒被宣告，我們就先使用，在 JavaScript 會出現索引錯誤 `ReferenceError` (如果是用 `let` 與 `const` 來宣告該變數)。
但如果是宣告了，但沒有賦予某個值，這時因為對 JavaScript 來說，它不知道該變數的值是什麼，所以會印出 `undefined`。
簡單來說，`undefined` 代表的是「**變數雖然存在，但目前還沒有值**」。

## 1. 什麼時候會出現 Undefined？

**變數剛宣告但未賦值**：

```
let x;
console.log(x); // undefined
```

**函式沒有回傳值**（預設回傳 `undefined`）：

```
function sayHi() {}
console.log(sayHi()); // undefined
```

```
// 還沒宣告就使用，會有 `ReferenceError`
console.log(greeting); // Uncaught ReferenceError: greeting is not defined
let greeting;
```

**存取物件不存在的屬性**：

```
const user = { name: "Alice" };
console.log(user.age); // undefined
```

**函式參數未傳入**：

```
function greet(name) {
  console.log(name);
}
greet(); // undefined
```

## 2. Undefined vs Null

這是面試與開發中最容易搞混的部分。

| 特性            | Undefined      | Null                           |
| ------------- | -------------- | ------------------------------ |
| 意義            | 「缺少值」（變數還沒準備好） | 「空值」（刻意人為設定為空）                 |
| 型別 (`typeof`) | `"undefined"`  | **`"object"`** (這是 JS 的經典 Bug) |
| 轉為數字          | NaN            | 0                              |
| 出現方式          | 通常是系統自動產生      | 通常是開發者手動賦值                     |
## 3. 檢查 Undefined 的正確姿勢

在判斷一個變數是否為 `undefined` 時，建議使用以下方式：

#### A. 嚴格相等 (推薦)

```
if (x === undefined) { ... }
```

#### B. 使用 `typeof` (更安全)

如果變數可能「連宣告都沒宣告」，直接存取 `x` 會報錯，此時用 `typeof` 不會報錯：

```
if (typeof x === "undefined") { ... }
```

## 4. 預設值處理 (Default Values)

在處理 `undefined` 時，我們會利用它來設定預設值：

```
// 1. ES6 參數預設值
function setup(timeout = 3000) { ... }

// 2. 空值合併運算子 (Nullish Coalescing)
const config = data ?? "default"; 
// 只有 data 為 undefined 或 null 時才使用 "default"
```

## 5. 注意事項：別把 undefined 當成變數名稱

雖然在現代 JS 中你無法在全域範圍覆寫 `undefined` 的值，但在某些舊版環境或局部作用域中，`undefined` 其實是可以被宣告為變數名稱的（這非常混亂）：

```
// 千萬不要這樣做
let undefined = "I am a string now";
```

- **不要手動賦值 `undefined`**：如果你想清空一個變數，請使用 `null`，這能明確告訴其他開發者：「這個變數現在是有意識地被清空了」，而不是「忘記給值」。
- **虛無五兄弟**：在邏輯判斷中，`undefined` 是 Falsy（假值）的一員。