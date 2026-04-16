## 三種基本的 CSS 樣式插入方式

### 1. 內聯樣式（Inline Styles）

內聯樣式（Inline Styles）是將 CSS **直接**寫在 HTML 標籤的 `style` 屬性中。這種方法適用於簡單的樣式或臨時調整。

- **優點**:不需要額外的 CSS 文件，適合小範圍或臨時的樣式修改
- **缺點**:不易維護，樣式分散在 HTML 檔案中
- **程式碼範例**:

```
<p style="font-size: 20px; color: red;">這是一段紅色的文字。</p>
```

### 2. 內部樣式（Internal Styles）

內部樣式（Internal Styles）是將 CSS 放在 HTML 文件的 `<head>` 內的 `<style>` 標籤中。這種方法適用於單個頁面的樣式設置。

- **優點**:將樣式集中在一處，便於管理
- **缺點**:不需要額外的 CSS 文件，適合單頁應用
- **程式碼範例**:

```
<!DOCTYPE html>
<html>
  <head>
    <style>
      h1 {
        color: blue;
        text-align: center;
      }
    </style>
  </head>
  <body>
    <h1>這是一個標題</h1>
  </body>
</html>
```

### 3. 外部樣式表（External Stylesheet）

外部樣式表是將 CSS 放在一個獨立的 `.css` 文件中，然後通過 HTML 文件的 `<link>` 標籤進行引用。這是最常見且推薦的方法，適合大型網站或需要在多個頁面間共享樣式的場景。

- **優點**:多頁面共享樣式，避免程式碼重複
- **缺點**:需要額外的 HTTP 請求來加載 CSS 文件
- **程式碼範例**:

首先，創建一個名為 `styles.css` 的文件，並添加以下內容：

```
h1 {
  color: blue;
  text-align: center;
}
```

然後，在 HTML 文件中引用該 `styles.css` 文件：

```
<!DOCTYPE html>
<html>
  <head>
    <!-- 透過 link 標籤引入樣式 -->
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <h1>這是一個標題</h1>
    <p>這是一段文字。</p>
  </body>
</html>
```

[[CSS 樣式優先順序]]