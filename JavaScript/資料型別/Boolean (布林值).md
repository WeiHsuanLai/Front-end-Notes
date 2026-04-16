有 `true` 與 `false` 兩個值的布林值，也是 JavaScript 的原生值。它是程式邏輯控制（如 `if` 判斷、迴圈）的核心。
## 1. 真值與假值 (Truthy & Falsy)

在 JS 中，當你在需要布林值的地方（例如 `if` 判斷式）放入非布林型別的值，JS 會自動進行轉型。這引申出了「真值」與「假值」的概念。

#### 假值 (Falsy Values)

以下 **6 個值** 在轉型後永遠為 `false`：

- `false`
- `0` (包含 `-0` 或 `0n`)
- `""` (空字串)
- `null`
- `undefined`
- `NaN`

#### 真值 (Truthy Values)

**除了上述假值以外的所有值** 都是 `true`。
**特別注意：** 空陣列 `[]`、空物件 `{}` 以及字串 `"0"` 或 `"false"` 在 JS 中都是 **真值**。

## 2. 布林轉換方法

如果你想明確地將其他型別轉換為布林值，有兩種主要方式：

| 方法             | 範例               | 結果   |
| -------------- | ---------------- | ---- |
| `Boolean()` 函式 | Boolean("Hello") | true |
| 雙驚嘆號 `!!`      | !!"abc"          | true |
`!!` 是開發中最常用的語法糖，第一個 `!` 會將值轉為反向布林，第二個 `!` 再轉回來，達成快速轉型。

## 3. 邏輯運算子 (Logical Operators)

| 運算子  | 名稱      | 說明                     |
| ---- | ------- | ---------------------- |
| &&   | AND (且) | 全部為真才回傳真。常用於「短路邏輯」。    |
| \|\| | OR (或)  | 其中一個為真即回傳真。常用於設定「預設值」。 |
| !    | NOT (非) | 反轉布林值。                 |
## 4. 實務應用場景
#### A. 短路邏輯 (Short-circuiting)

這在 React 或一般邏輯判斷中非常常見：
```
// 如果 user 存在，才執行後面的內容
const name = user && user.name; 

// 如果沒有設定顏色，則預設為 'black'
const color = customColor || 'black';
```

#### B. 條件判斷

這是布林值最根本的用途：
```
const isAdult = age >= 18; // 這會產生一個布林值

if (isAdult) {
  console.log("可以進入");
} else {
  console.log("謝絕進入");
}
```

## 5. Boolean 物件 vs 原始布林值

**注意：** 雖然可以用 `new Boolean(true)` 建立物件，但在實務上**極度不建議**這樣做。

```
const boolObj = new Boolean(false);
if (boolObj) {
  // 這裡會執行！因為 boolObj 是一個「物件」，而物件永遠是真值 (Truthy)。
}
```