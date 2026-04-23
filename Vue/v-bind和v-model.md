## 1. v-bind：單向綁定 (One-Way Data Binding)

`v-bind` 的作用是將資料「注入」到 HTML 標籤的屬性（Attribute）中。

- **方向：** 資料 (JS) $\rightarrow$ 視圖 (Template)。
    
- **語法糖：** 縮寫為 `:`。
    
- **特點：** 當資料改變時，視圖會更新；但當使用者在畫面操作（如輸入文字）時，JS 裡的資料**不會**自動改變。
    

HTML

```
<img v-bind:src="imageSrc">

<input :value="userName"> 
```

---

## 2. v-model：雙向綁定 (Two-Way Data Binding)

`v-model` 常見於表單元素（input, select, textarea），它讓資料和畫面同步。

- **方向：** 資料 (JS) $\leftrightarrow$ 視圖 (Template)。
    
- **特點：** 資料改了，畫面會變；使用者在畫面上輸入，JS 裡的資料也會**同步更新**。
    
- **底層原理：** 它其實是 `v-bind` 與 `事件監聽` 的語法糖。
    

HTML

```
<input v-model="userName">

<input 
  :value="userName" 
  @input="userName = $event.target.value"
>
```

---

## 3. 核心差異對比

| **特性**   | **v-bind (:)**                              | **v-model**                 |
| -------- | ------------------------------------------- | --------------------------- |
| **資料流向** | 單向：JS $\rightarrow$ DOM                     | 雙向：JS $\leftrightarrow$ DOM |
| **適用對象** | 任何 HTML 屬性 (src, class, style, disabled...) | 表單元素或自定義組件的數值               |
| **主要用途** | 傳遞資料、控制樣式、禁用按鈕                              | 收集使用者輸入的資料                  |

---

## 4. 進階觀念：在自定義組件上使用

在你開發 Quasar 專案時，會頻繁遇到在「組件」上傳遞資料。

### **情境 A：我只想傳資料進去**

這時用 `v-bind`。例如傳遞一個按鈕的標籤文字：

HTML

```
<MyButton :label="btnText" />
```

### **情境 B：我希望子組件修改後，父組件也跟著變**

這時用 `v-model`。在 Vue 3 中，這相當於傳入一個名為 `modelValue` 的 prop，並監聽一個 `update:modelValue` 的事件。

---

## 5. 總結與建議

- 如果你只是要**顯示**資料，或是控制一個 `class` 是否存在，請用 **`v-bind`**。
    
- 如果你是要**接收**使用者輸入（例如登入表單、搜尋框），請用 **`v-model`**。