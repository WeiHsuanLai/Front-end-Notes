在 JavaScript 當中有許多可以比較相等與否的方法。其中最常見的三個分別是 **`===` (嚴格比較)**、 **`==` (鬆散比較)**，以及 **`Object.is` (同值比較)**。
## `==` 鬆散比較 (loose equality)

`==` 在比較兩個值之前，**會先強制轉換型別與值**，舉例來說，下面的這三個例子，之所以會印出 `true` ，正是因為在比較前，型別被轉換了；否則數字 `1` 在 JavaScript 中跟字串 `'1'` 是代表不同的值，嚴格來說不應該相等。同理 `undefined` 與 `null` 在 JavaScript 是不同型別與不同的值，但如果使用 `==` 會回傳 `true`。

```
console.log(1 == "1"); // true
console.log(0 == false); // true
console.log(undefined == null); // true (這是 JS 的特殊規則)
console.log(true == 1); // true (true 被轉成數字 1)
```

因為會強制轉換型別，`==` 會帶給開發者一些困擾。因此多數的情況，**不建議使用 `==`**。

## `===` 嚴格比較 (strict equality)

`===` 不會強制轉換型別與值，所以如果是不同型別，比較兩者會回傳 `false`。不同值的話一樣會回傳 `false`。不過有兩個情況例外，當我們比較 `+0` 和 `-0`時，嚴格比較會回傳 `true`；以及比較 `NaN` 和 `NaN` 會是 `false`。而這兩個狀況則是同值比較 `Object.is` 派上用場的時候。

```
+0 === -0; // true
NaN === NaN; // false
console.log(true === 1); // false
```

## `Object.is` 同值比較 (same-value equality)

同值比較顧名思義是在比較兩個值是不是相等。雖然它是 Object 開頭，但比較的可以是任意的兩個值。例如：

```
console.log(Object.is(1, 1)); // true
console.log(Object.is(1, "1")); // false
```

上面提到的兩種在 `===` 時遇到的問題，可以透過 `Object.is` 有效分辨

```
console.log(Object.is(+0, -0)); // false
console.log(Object.is(NaN, NaN)); // true
```

``` 
關於 NaN 的比較 
const myNaN = NaN; 
console.log(myNaN === NaN); // false (在 JS 中，NaN 不等於自己) console.log(Object.is(myNaN, NaN)); // true (Object.is 可以正確辨識 NaN)

// 關於正負零 的比較 
console.log(0 === -0); // true 
console.log(Object.is(0, -0)); // false (Object.is 認為正負號不同即不同)
```


不過如果要有效分辨 `NaN`，在 JavaScript 有一個方法叫 `isNaN` 是可以使用的。端看開發團隊的習慣，假如對於 `Object.is` 感到陌生，可以選擇用 `Number.isNaN`。

## 物件與陣列的比較（參照比較）

無論使用哪種方法，對於**非原始型別**（Object, Array），比較的是「記憶體位址」而非內容。

```
const arr1 = [1, 2];
const arr2 = [1, 2];
const arr3 = arr1;

console.log(arr1 === arr2);        // false (內容相同但記憶體位址不同)
console.log(Object.is(arr1, arr2)); // false
console.log(arr1 === arr3);        // true  (指向同一個記憶體位址)
```
