簡單來說，HTML 語意化就是用合乎語意的 HTML 標籤開發，例如：`<form>`、`<table>`，標籤會清楚定義其內容、讓頁面兼具良好的語意和結構。以下會提到兩個重點，HTML 語意化標籤有哪些，另一個是，優點有哪些。

## HTML 語意化標籤 (semantic tags)

目前語意化標籤大約有 100 多項，常見的如下

- `header`
- `h1~h6`
- [`hgroup`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/hgroup)
- [`nav`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav)
- [`main`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/main)
- [`article`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/article)
- [`section`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section)
- [`aside`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside)
- [`details`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details)
- [`time`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/time)
- [`footer`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/footer)
- [`figure`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/figure)
- [`figcaption`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/figcaption)
## 為什麼要使用 HTML 語意化標籤

### 提升 SEO

比起使用非語意化的標籤例如：`div`、`span`，語意化標籤可以幫助搜尋引擎重視在「標題」、「連結」裡的關鍵字，這樣可以讓有語意化標籤的網頁更容易被使用者搜尋到。

### 便於開發

語意化標籤可以提升程式碼的可讀性、幫助開發者之間更容易理解網頁架構、減少差異化，舉例來說，你可以用 `div` 來做一個按鈕，但如果用 `button` 對其他開發者而言會更一目瞭然知道那是個按鈕； 除此之外，透過語意化標籤，開發者可以輕易使用其中的原生功能。以 `button` 來說，就有許多原生功能是 `div` 沒有的。

### 無障礙網頁 (HTML accessibility)

HTML 語意化網站可以方便其他設備解析內容，例如：螢幕閱讀器、盲人閱讀器、移動設備。a11y 在前端業界是越來越被看重的領域，用語意化標籤也因此是許多公司會看重的。