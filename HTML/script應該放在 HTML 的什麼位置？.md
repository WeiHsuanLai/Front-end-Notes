`<script>` 可以放在 HTML 文件中的任意位置，不論是 `<head>` 或 `<body>` 甚至是 `<div>` 中，具體位置取決於要引入 JavaScript 檔案的功能以及加載順序的需要。

但要注意的是，**HTML 解析的順序是由上而下依序載入**。也就是說，放置的位置會影響到加載的順序。

此外，瀏覽器解析 HTML 時，會由上而下依序載入內容。當遇到 `<script>` 標籤時，瀏覽器會暫停 HTML 解析，先下載並執行 JavaScript 檔案，然後再繼續解析剩餘的 HTML 內容。

當 JavaScript 檔案特別大、或是檔案是從外部資源載入的，有可能會受到網路傳輸或外部伺服器的影響，如果加載過程太長，會阻塞 HTML 頁面的解析，將會導致頁面長時間停滯，影響使用者體驗。

基於上述的原因，大部分會建議 `<script>` 放置在 `</body>` 底部前，這樣做有以下好處：

1. **畫面更快呈現**：瀏覽器會先解析完整個 HTML，再下載 JavaScript 檔案、解析，因此使用者可以在第一時間看到畫面，有更好的用戶體驗
2. **避免 Script 異動到還未載入的 HTML 元素**：Script 有可能會異動到 HTML 元素，因此將 `<script>` 置於 `<body>` 的底部，可以確保 HTML 元素已經完全載入，減少異動元素造成錯誤的風險。

**程式碼範例**

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello World</title>
  </head>

  <body>
    <div>...</div>
    <div>...</div>
    <div>...</div>
    <div>...</div>
    <!-- <script> 置於 <body> 的底部 -->
    <script src="https://explainthis.com/script"></script>
  </body>
</html>
```

### 當 <script></script> 放置在 <head></head>中的問題

下方程式碼為例，
```
<html>
  <head>
    <script>
      // 嘗試異動特定的 HTML 元素
      var header = document.getElementById("header");
      header.innerHTML = "異動過的文字";
    </script>
  </head>
  <body>
    <h1 id="header">原始的文字</h1>
  </body>
</html>
```

因為 `<script>` 是放在 `<head>` 中，當頁面載入時，Script 會立即執行，此時 HTML 元素還未載入 DOM，因此

```
document.getElementById("header")
```

會回傳 `null`，導致錯誤。

### 當 <script></script> 放置在 <body></body>尾部的問題

但其實，`<script>` 放在尾部的也是有一些缺點，例如：網頁一開始會沒有功能性。因為 `<script>` 最後加載，所以功能還沒有被載入，這種情況可能會導致使用者雖然看到畫面，但卻沒有功能性的情況發生。

所以將 `<script>` 放在尾部也不是最優解，更好的方式，會是透過 defer 或 async 一邊解析頁面，一邊下載 JavaScript。

