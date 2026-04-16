瀏覽器引擎在佈局文檔 (document) 時，會根據 CSS 的 Box Model 把每個元素視為長方形狀的 Box，這個 Box 是由內容 (content)、內距 (padding)、邊框 (border) 和外距 (margin) 所組成。

![[Pasted image 20260416102836.png]]
### 1. 標準盒模型 (The W3C Box Model)

這是現代瀏覽器的預設模式（對應 CSS 屬性 `box-sizing: content-box;`）。

- **定義：** 設定的 `width` **僅包含內容（Content）本身**。
    
- **計算公式：**
    
    - 總寬度 = `width` + `padding` + `border` + `margin`
        
- **特點：** 如果你設定一個盒子 `width: 100px` 並加上 `padding: 20px`，盒子最後在畫面上看起來會變寬（變成 140px）。這在排版時比較不直觀，因為加了邊框或內距後，盒子會「往外撐大」。
    

### 2. 怪異盒模型 (The Internet Explorer Box Model)

這是舊時代 IE 瀏覽器的行為（對應 CSS 屬性 `box-sizing: border-box;`）。

- **定義：** 設定的 `width` **包含了內容、內距（Padding）與邊框（Border）**。
    
- **計算公式：**
    
    - 總寬度 = `width` + `margin`（其中 `width` 已經吞掉了 `padding` 和 `border`）
        
- **特點：** 內容區域會被自動壓縮。如果你設定 `width: 100px`，無論你怎麼加 `padding`，盒子外觀寬度永遠固定是 100px。


### 總結與現代開發應用

|**項目**|**標準 (W3C)**|**怪異 (IE / Border-box)**|
|---|---|---|
|**CSS 屬性**|`box-sizing: content-box`|`box-sizing: border-box`|
|**Width 包含**|僅 Content|Content + Padding + Border|
|**排版直覺度**|較低（容易爆版）|**較高（目前主流）**|

**為什麼現在大家都愛用右邊（IE 模式）？**

雖然它被稱為「怪異模式」，但因為它固定了外層總寬度，對於響應式設計（Responsive Design）非常友善。

這也是為什麼現在大多數的前端框架（如你常用的 **Quasar** 或 **Tailwind CSS**）以及常用的 CSS Reset，都會預設加上這段程式碼：

CSS

```
*, *::before, *::after {
  box-sizing: border-box;
}
```

這能確保當你設定 `width: 100%` 時，不會因為加了一個 `border` 就讓頁面出現水平滾動條。