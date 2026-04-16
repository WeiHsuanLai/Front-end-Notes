CSS 有許多選擇器，例如：id、class，當有一個元素重複用到選擇器定義的樣式時，**優先級**的概念就很重要，**優先級**高的選擇器樣式會優先出現，而這些選擇器的優先級也是被定義好的，以下文章會分享到大部分選擇器的優先級排列。

## 優先級分數

了解每個選擇器的順序優先級之前，要了解分數 (specificity scoring) 的概念，每個由選擇器組成的樣式自己本身會有一個加總分數，分數最高的會勝出。但是，當我們在開發 CSS 時，如果同樣能寫出想要的樣式，要盡可能選擇分數較低的，而不是分數較高的；因為如果未來有一個更重要的樣式，可以比較容易添加上去。假如某個樣式在最開始就被寫成最重要，這時要加上其他樣式就會變得困難。

## 選擇器分數

![選擇器分數](https://firebasestorage.googleapis.com/v0/b/ets-web-8484d.firebasestorage.app/o/images%2F1755263484683_c816898f1e834c149f02810d711ef3d8.png?alt=media&token=9afc94ab-eaee-4368-bd4f-e8c60ae92573)

選擇器分數

圖片來源：https://web.dev/learn/css/specificity/#specificity-scoring

## 選擇器優先級整理

- 常用優先級排序整例如下

```
!important > 行內樣式 > id > class = 偽類 > 型態選擇器 = 偽元素 > 通用
```

除了上數的分數計算規則以外，另外補充幾點樣式優先級的特性

- `!important` 分數最重，但要小心使用
- 如果優先級相同，則最後出現的樣式會生效
- 如果是藉由繼承的樣式優先級會最低

## 實際例子

透過以下例子來了解選擇器加總後的分數是如何計算的

```
<a class="my-class another-class" href="#">
  A link
</a>
```

```
// 1
a {
  color: red;
}

// 1+10 = 11
a.my-class {
  color: green;
}

// 1+10+10 = 21
a.my-class.another-class {
  color: black;
}

// 1+10+10+10 = 31
a.my-class.another-class[href] {
  color: purple;
}

// 1+10+10+10+10 = 41
a.my-class.another-class[href]:hover {
  color: lightgrey;
}

// 從上面的分數可以得知，上面的 Link 平常會是紫色，當游標移到該元素時會變成淺灰色
```

**資料來源**

- [https://web.dev/learn/css/specificity/](https://web.dev/learn/css/specificity/#specificity-scoring)