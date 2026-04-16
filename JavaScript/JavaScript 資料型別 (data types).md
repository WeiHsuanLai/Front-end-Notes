
截至目前，JavaScript 的資料型別中，有**七個原生值**(primitive values)。這七個原生值以外的，全都是屬於物件。

原生值是不可變的 (immutable)，意思是我們不能改變那個值本身。舉例來說字串 (string) 是其中一個 JavaScript 原生值，我們不能去改變 `'Hi'` 這一個字串 (但在其他程式語言，字串有可能是可變的，例如在 C 就是可變的)。我們僅可以把某個變數，賦予另一個字串，例如：

```
let greeting = "Hi";
greeting = "Hello"; // 賦予另一個值，但上面的 'Hi' 本身沒變動
```

JavaScript 的型別中的七個原生值包含

- [String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) [[String (字串)]]
- [Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/boolean)[[Boolean (布林值)]]
- [Null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null)[[Null]]
- [Undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)[[Undefined]]
- [Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/number)[[Number]]
- [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/bigint)[[BigInt]]
- [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)[[Symbol]]

## JavaScript 的物件 (objects)

除了上述的七個原生值以外的存在，在 JavaScript 當中都是物件。在 JavaScript 有一個梗是

> Objects, Arrays, Functions, Objects 當中，Objects 好像被重複提了兩次。喔不，其實是四次。

會說 Objects 被提了四次說因為在 JavaScript 當中，Array (陣列) 與 Function (函式)，都是物件。

## 如何辨別一個變數的資料型別?

要辨別一個變數的資料型別，最常見的方式是透過 `typeof` 這個方法。舉例來說 `typeof` 判斷字串。

```
let greeting = "hi";
console.log(typeof greeting); // 'string'
```

不過在 JavaScript 當中，有幾個小例外，其中一個是 `null` 。如果用 `typeof` 判斷 `null` 的資料型別，會得到 `object`。

```
console.log(typeof null); // object
```

這是一個 JavaScript 的歷史遺跡，但因為要改掉這個 bug 的成本太高，所以到目前為止還是有這個錯誤。

不論如何， `null` 的資料型別應該是 `null` 而不是 object。

另一點要注意的是，`typeof` 判斷陣列時，會回傳 `object`; 但判斷函式時，會回傳 `function`。

```
console.log(typeof []); // object
console.log(typeof function () {}); // function
```

補充 `typeof` 結果的表格，來源參考 [ECMAScript® 2015 Language Specification](https://262.ecma-international.org/5.1/#sec-11.4.3) 。

| Type of val                                         | Result                                                                                  |
| --------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Undefined                                           | "undefined"                                                                             |
| Null                                                | "object"                                                                                |
| Boolean                                             | "boolean"                                                                               |
| Number                                              | "number"                                                                                |
| String                                              | "string"                                                                                |
| Object (native and does not implement [[Call]])     | "object"                                                                                |
| Object (native or host and does implement [[Call]]) | "function"                                                                              |
| Object (host and does not implement [[Call]])       | Implementation-defined except may not be "undefined", "boolean", "number", or "string". |

### 我們該如何辨別某個變數是物件，還是陣列呢?

`Array.isArray()` 是可以協助我們的方法。如果是陣列，會回傳 `true`；但若是一般物件，則會回傳 `false`。舉例來說：

```
Array.isArray([1, 2, 3]); // true
Array.isArray({ foo: 123 }); // false
```

我們也可以透過 `Object.prototype.toString()` 的方法幫助我們辨別陣列、函式與一般物件。

```
const arr = [1, 2, 3];
const fn = () => {
  return 123;
};
const obj = { foo: 123 };

console.log(Object.prototype.toString.call(arr)); // [object Array]
console.log(Object.prototype.toString.call(fn)); // [object Function]
console.log(Object.prototype.toString.call(obj)); // [object Object]
```