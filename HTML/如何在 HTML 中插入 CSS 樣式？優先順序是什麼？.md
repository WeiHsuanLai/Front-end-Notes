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

## CSS 樣式優先順序

既然有多種方式可以加入 CSS ，那我們就必須了解這幾種方式若都有使用的情況下，他們分別的優先級是什麼。大部分情況下，這三種 CSS 插入方式的優先順序為：

內聯樣式（Inline Styles） > 內部樣式（Internal Styles）= 外部樣式表（External Stylesheet）。

內聯樣式基本上會蓋過其他任何樣式，只有用 `!important` 標注的樣式才不會蓋過。在這三種樣式中，內聯樣式優先度也是最高的。

而內部樣式與外部樣式兩者的優先度會是一樣的。不過你可能會問，假如優先度一樣，那麼兩邊都寫樣式 (例如一個寫 `color:blue` 另一個寫 `color:orange`) 有衝突的話，會是以哪個為主呢?

在這種情況下，會去看其他的因素，例如 CSS 被引入的順序。假如某個先出現，而後面又引入了另一個，兩者優先度相同的狀況下，會以新引入的為主。

下面兩張圖在 HTML 加入 CSS 不同方式的優先順序效果的範例。

![可以看到右邊的字體呈現的是內聯樣式（Inline Styles）的藍色](https://explainthis-image.octave.vip/dcf07dba3bd447e18b0d3c2a3c209fab.png)

可以看到右邊的字體呈現的是內聯樣式（Inline Styles）的藍色

![把內連樣式移除之後，右邊的字顯示的是內部樣式（Internal Styles）的橘色，讀者可以試著對調 `<link>` 與 `<style>` 出現的順序，會有不同效果](https://explainthis-image.octave.vip/78e708a3c2654e229d23388b74cfbfd0.png)

把內連樣式移除之後，右邊的字顯示的是內部樣式（Internal Styles）的橘色，讀者可以試著對調 `<link>` 與 `<style>` 出現的順序，會有不同效果

## 結論

對於大型網站或需要重複使用的樣式，建議使用外部樣式表，以提高可維護性和效率; 對於只需要在特定頁面或元素上使用的樣式，可以考慮使用內嵌樣式或行內樣式。