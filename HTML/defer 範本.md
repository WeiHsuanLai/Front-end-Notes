這是最常用的情境。當你的 `main.js` 需要操作 DOM，或者需要用到 `library.js` 的變數時，使用 `defer` 確保順序正確，且不阻塞渲染。

HTML

```
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <title>Defer Example</title>

    <script defer src="https://cdn.example.com/jquery.min.js"></script>
    
    <script defer src="./js/plugins/slider.js"></script>
    
    <script defer src="./js/main.js"></script>
</head>
<body>
    <div id="app">
        <h1>前端開發專案</h1>
    </div>
</body>
</html>
```