這類腳本通常與你的頁面邏輯無關，早一點或晚一點執行都不會影響 User 使用功能。最經典的例子就是 **Google Analytics (GA4)**。

HTML

```
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <title>Async Example</title>

    <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXX"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', 'G-XXXXX');
    </script>

    <script async src="https://example.com/ads-provider.js"></script>
</head>
<body>
    <h1>我的個人網站</h1>
</body>
</html>
```