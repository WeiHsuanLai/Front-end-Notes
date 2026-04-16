在 CSS 中，有許多不同的單位，例如：px、em、rem 等，在面試中，可能會被問到，CSS 這些單位的差別跟什麼情境該如何使用。

## px、em、rem 的區別

- px：最常見的單位，也是絕對單位。根據 [MDN](https://developer.mozilla.org/en-US/docs/Glossary/CSS_pixel#:~:text=The%20term%20CSS%20pixel%20is,1%2F96th%20of%201%20inch.)，1px 的實際長度通常會是 1 吋 的 1/96。
    
- em：屬於相對長度單位，用在字體和一般長度單位會是不同計算方式，以下依照兩種不同情境解釋。
    
    - 用在 font-size：會依照父層字體大小做倍數計算。例如：`font-size: 2em` 即表示，當前元素的字體大小是父元素字體大小的兩倍。
    - 用在 width：會是當前元素字體大小的倍數，例如設定 `width: 2em` 時， width 會是當前元素的字體大小再乘上 2 倍，如果元素字體大小是 `10px` 則 `width` 會是 `20px`。特別注意，如果沒有設定元素的字體大小，則會默認父元素的字體大小，換句話說，如果父元素字體大小為 `30px` 而我們沒設定該元素的字體大小，這時 `width: 2em` 會是 `60px`。
    
    以下程式碼範例，`div` 中的字體大小為 20px，因為父元素 (`body`) 是 10px，`div` 元素設定 `2em` 就會是 10px * 2。 而 `div` 寬度會是 40px，因為元素的字體大小是 20px，所以 `width: 2em`，會是 20px * 2。
    
    ```
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <title>JS Bin</title>
    </head>
    <body>
    <div>em</div>
    </body>
    <style>
    body{
      font-size:10px;
    }
    div{
      font-size:2em;
      width:2em;
      height:100px;
      border:1px solid red;
    }
      </style>
    </html>
    ```
    
- rem：類似於 em 也是相對長度單位，rem 全名是 root em，這邊的 root 是指**根元素，根元素就是** `html`**；換句話說，rem 是根據** `html` 去做倍數計算。它的計算方式類似於 em ，只是不管在哪用 rem，都是根據根元素計算，而不是根據該元素的父層元素做計算。
    

以下程式碼範例，不論是 `div` 的寬度，或是 `p` 的寬度，都會是 100px。因為根元素 (html) 的寬度是 50px，`2rem` 就會是 50px * 2。

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>JS Bin</title>
  </head>
  <body>
    <div>
      <p>rem</p>
    </div>
  </body>
  <style>
    html {
      font-size: 50px;
    }
    body {
      font-size: 10px;
    }
    div {
      font-size: 2rem;
      width: 2rem;
      height: 100px;
      border: 1px solid red;
    }
    p {
      font-size: 2rem;
      width: 2rem;
    }
  </style>
</html>
```

## px、em、rem 該如何選擇?

px 的優勢在於屬於絕對單位，開發時可以很準確的設置大小長度是多少，但缺點就是無法跟著頁面大小改變而去改變。

em 和 rem 屬於相對單位，相比於 px 更有靈活性，rem 的出現比 em 新一點，主要解決了 em 一個嚴重問題，因為使用 em 會有繼承問題，每個元素會自動繼承其父元素字體大小，如果版面設計是相當多層的，可能會造成使用上混亂，因此只建議使用於簡單排版、少層級、只需要與父層比較的版面或區塊。

使用 rem 則可以讓我們避免這個問題，不管任何階層的元素，相比的基準點會是根元素，適合使用在 RWD 或行動裝置開發上。此外，近幾年很熱門的 CSS framework - Tailwind CSS 其中某些定義好的的長度，也是使用了 rem 去做計算，如下圖。

![Tailwind CSS width 單位](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/images%2F1755263935206_Screenshot%202025-08-15%20at%209.18.42%E2%80%AFPM.png?alt=media&token=c263a819-7dc2-4249-885c-91495229a03a)

Tailwind CSS width 單位

圖片來源：https://tailwindcss.com/docs/width