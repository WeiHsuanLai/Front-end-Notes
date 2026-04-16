它是一個獨特 (unique) 值，多半會搭配物件一起使用，作為物件的鍵 (key)。
它是 ES6 引入的第七種型別，主要特徵是 **「獨一無二」** 且 **「不可變」**。
它不像字串或數字，Symbol 的存在不是為了顯示內容，而是為了當作 **「絕對唯一的標記（識別碼）」**。

```
const sym = Symbol("ExplainThis");
const obj = { [sym]: "Interview Preps for Software Engineers" };
obj[sym]; // Interview Preps for Software Engineers
```

#### 1. 基本宣告
每個被創建出來的 Symbol 都是完全獨立的，即使描述文字（description）相同也不相等。

```
const s1 = Symbol("id");
const s2 = Symbol("id");

console.log(s1 === s2); // false (獨一無二的特性)
```

#### 2. 核心用途：隱藏的物件屬性

Symbol 最常見的用法是作為物件的 **Key (屬性名)**。這樣做有兩個大好處：

1. **防止屬性名稱衝突**：如果你在開發插件或大型框架，使用 Symbol 可以確保你的屬性不會被其他開發者不經意地覆蓋。
    
2. **非枚舉性 (Semi-private)**：Symbol 作為 Key 時，一般的迴圈（如 `for...in`）或 `Object.keys()` 是抓不到它的。

```
const MY_KEY = Symbol("secret");
let obj = {
  name: "Gemini",
  [MY_KEY]: "這是隱藏資訊"
};

console.log(obj[MY_KEY]); // "這是隱藏資訊"
console.log(Object.keys(obj)); // ["name"] (Symbol 被跳過了)
```

#### 3. 全域共享：Symbol.for()

有時候我們需要在不同地方使用同一個 Symbol，這時可以使用「全域註冊表」。

| 方法                 | 功能                  | 範例                                          |
| ------------------ | ------------------- | ------------------------------------------- |
| Symbol.for(key)    | 檢查註冊表，有則回傳，無則建立     | Symbol.for("a") === Symbol.for("a") // true |
| Symbol.keyFor(sym) | 反查該 Symbol 在註冊表中的名稱 | Symbol.keyFor(s1) // "a"                    |

#### 4. 內建的知名 Symbol (Well-known Symbols)

JavaScript 內部使用了一些特殊的 Symbol 來定義語言的底層行為。你可以透過修改這些 Symbol 來改變物件的運作方式。

- **`Symbol.iterator`**：定義一個物件如何被 `for...of` 遍歷。
    
- **`Symbol.toStringTag`**：自定義 `Object.prototype.toString.call()` 回傳的標籤文字。
    
- **`Symbol.hasInstance`**：定義 `instanceof` 的判斷邏輯。

#### 5. 如何獲取物件內的 Symbol 屬性？

雖然 `Object.keys()` 拿不到，但並非真的「私有」。你可以透過以下方式獲取：

```
// 獲取物件中所有的 Symbol Key
const symbols = Object.getOwnPropertySymbols(obj);

// 獲取物件所有屬性（含一般 Key 與 Symbol Key）
const allKeys = Reflect.ownKeys(obj);
```

### 💡 筆記小叮嚀：

- **不能使用 `new`**：`Symbol` 不是建構函式，直接呼叫 `Symbol()` 即可。
    
- **不支援轉型**：Symbol 不能自動轉成字串或數字（例如 `Symbol("id") + "123"` 會報錯），必須顯式呼叫 `.toString()`。
    
- **JSON 限制**：`JSON.stringify()` 會直接忽略 Symbol 屬性。