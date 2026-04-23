`v-for` 是 Vue 的迴圈指令，用來重複渲染列表資料。

```
<ul>   
  <li v-for="item in list" :key="item.id">{{ item.name }}</li>
</ul>
```

除了 `in`，也可以用 `of` 語法。

```
v-for="item of list"
```
## 1. 基本語法

### 遍歷陣列 (最常用)

```
<ul>
  <li v-for="(item, index) in items" :key="item.id">
    {{ index }} - {{ item.name }}
  </li>
</ul>
```

### 遍歷物件

```
<ul>
  <li v-for="(value, key, index) in myObject">
    {{ index }}. {{ key }}: {{ value }}
  </li>
</ul>
```

## 2. 為什麼一定要寫 `:key`？ (重要！)

這是面試最常考的題目。在 Vue 的 **Diff 演算法** 中，`key` 是節點的「身分證」。

- **如果沒有 key：** Vue 會採用「就地複用」策略。如果列表順序變了，Vue 不會移動 DOM 元素，而是直接修改裡面的內容。這在處理有狀態的組件（如包含 `<input>`）時會產生嚴重的 Bug。
    
- **有了唯一的 key：** Vue 能夠精確地追蹤每個節點，知道哪些元素是新加的、哪些只是移動了位置。這能大幅提升渲染效能。
    

> **專業建議：** 永遠使用資料中的 **唯一 ID**（如 `item.id`）作為 key，避免使用 `index`（除非你的列表永遠不會變動、排序或過濾）。

---

## 3. `v-for` 與 `v-if` 的優先級 (Vue 2 vs Vue 3)

這是一個常見的陷阱，Vue 3 對此做了重大的改變：

- **Vue 2：** `v-for` 優先級較高。這意味著即使你想隱藏某些項目，Vue 還是會先跑完循環，非常浪費效能。
    
- **Vue 3：** **`v-if` 優先級較高**。這意味著在同一個標籤上同時寫這兩個指令，`v-if` 會因為拿不到 `v-for` 定義的變數而報錯。
    

**✅ 正確做法（過濾資料）：** 使用 `computed` 先過濾好資料，再交給 `v-for` 渲染。

JavaScript

```
const activeItems = computed(() => {
  return items.value.filter(item => item.isActive)
})
```

## 4. 總結筆記

1. **語法：** `v-for="(item, index) in list"`。
    
2. **核心：** 必須綁定唯一的 `:key`。
    
3. **效能：** 避免在同一個標籤上並用 `v-if` 與 `v-for`。
    
4. **變動監測：** Vue 3 的響應式系統（Proxy）可以完美監測到陣列索引的修改或長度變更，這點比 Vue 2 強大許多。