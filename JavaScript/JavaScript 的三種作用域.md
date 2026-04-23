### A. 全域作用域 (Global Scope)

在程式碼任何地方都能存取的變數。但在模組化開發（如 Vue 專案）中，除非掛載在 `window` 或 `global`，否則變數通常會被限制在模組內。

### B. 函式作用域 (Function Scope)

由 `var` 宣告的變數，其影響力僅限於該函式內部。

> **注意：** `var` 具有「提升 (Hoisting)」特性，容易造成邏輯 Bug，現在開發已極少使用。

### C. 區塊作用域 (Block Scope)

這是現在的主流，由 `let` 和 `const` 宣告。變數只在 `{ }` 括號內有效（例如 `if` 判斷式或 `for` 迴圈內）。

JavaScript

```
if (true) {
  let blockVar = "我在裡面";
  var functionVar = "我在外面也看得到 (var 的問題)";
}
console.log(functionVar); // 正常執行
console.log(blockVar);    // 報錯：ReferenceError
```

---

## 2. Vue 中的作用域陷阱

在 Vue 開發（特別是 Quasar 這種複雜框架）中，你最常遇到的作用域問題通常是以下兩種：

### A. `this` 的指向問題 (Options API)

在 Vue 2 或 Vue 3 的 Options API 中，`this` 指向組件實例。但如果你在 `setTimeout` 或非箭頭函式中使用 `this`，作用域會跑掉。

- **錯誤：** `setTimeout(function() { this.data = 1 }, 100)` — 這裡的 `this` 指向 `window`。
    
- **正確：** 使用 **箭頭函式**，它會自動捕捉定義時的外層 `this`。

#### 1. 經典錯誤：`setTimeout` 中的一般函式

這是最常遇到的情況。當你使用 `function()` 宣告時，它會建立自己的作用域，此時的 `this` 會指向全域對象（在瀏覽器中是 `window`），而不是 Vue 組件。

JavaScript

```
export default {
  data() {
    return {
      message: 'Hello Vue!'
    }
  },
  methods: {
    updateMessage() {
      console.log('外層 this:', this.message); // 正常：'Hello Vue!'
      
      setTimeout(function() {
        // ❌ 錯誤：這裡的 this 指向 window
        // window.message 是 undefined
        this.message = '已更新！'; 
        console.log('內層 this:', this.message); // undefined
      }, 1000);
    }
  }
}
```

---

#### 2. 正確做法 A：使用「箭頭函式」（推薦）

箭頭函式**沒有自己的 `this`**，它會捕獲「定義時」所處對象的 `this` 值。這是在 Vue 中最優雅、最現代的解法。

JavaScript

```
methods: {
  updateMessage() {
    setTimeout(() => {
      // ✅ 正確：箭頭函式會抓到外層的 this（即 Vue 實例）
      this.message = '已更新！';
      console.log('內層 this:', this.message); // '已更新！'
    }, 1000);
  }
}
```

---

#### 3. 正確做法 B：暫存 `this` (That / Self 變數)

這是在 ES6 普及前的傳統做法，雖然現在較少見，但在閱讀舊程式碼時一定要看懂。

JavaScript

```
methods: {
  updateMessage() {
    const self = this; // 把正確的 this 存進變數
    
    setTimeout(function() {
      // ✅ 正確：透過閉包（Closure）存取 self
      self.message = '已更新！';
    }, 1000);
  }
}
```

---

#### 4. 另一個陷阱：在 Methods 中使用箭頭函式（大忌）

這是一個反直覺的錯誤。**絕對不要直接把 Methods 定義為箭頭函式**。

JavaScript

```
export default {
  methods: {
    // ❌ 嚴重錯誤：這會導致 this 指向父級作用域（通常是 window）
    badMethod: () => {
      this.someData = 'error'; // 報錯，因為 this 不是 Vue 實例
    }
  }
}
```

> **原因：** Vue 的 Options API 需要透過一般函式來進行內部的 `this` 綁定。如果你用了箭頭函式，Vue 就沒辦法介入修改它的指向了。

### B. v-for 的作用域

在 `v-for` 中定義的變數（如 `item`），只在該循環的標籤及其子標籤內有效。

HTML

````
<div v-for="item in list" :key="item.id">
  <span>{{ item.name }}</span> </div>
<p>{{ item.name }}</p> ```

---

## 3. 插槽作用域 (Scoped Slots) - 進階必考
這是 Vue 最強大也最容易讓人混淆的作用域功能。當你想讓**父組件**拿到**子組件**內部的資料來渲染時，就會用到 Scoped Slots。

**場景：** 你用 Quasar 的 `q-table`，想根據每一列的內容自定義顯示方式。

```
<q-table :rows="rows">
  <template v-slot:body-cell-name="props">
    <q-td :props="props">
      <q-badge>{{ props.value }}</q-badge>
    </q-td>
  </template>
</q-table>
````

---

## 4. 如何避免作用域問題？

1. **永遠使用 `let` 或 `const`：** 徹底拋棄 `var`，減少變數污染。
    
2. **善用箭頭函式：** 解決 90% 的 `this` 指向錯誤。
    
3. **Composition API (`<script setup>`)：** 在 Vue 3 中，變數宣告在 `setup` 頂層就能直接在模板使用，作用域邏輯比 `data/methods` 更清晰。
    
4. **模組化：** 透過 `import` 和 `export` 嚴格控制變數的流動。