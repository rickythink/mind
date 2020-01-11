# type

## 定义

判断类型

## 背景知识

js中有几种数据类型：

* null\(空\)
* undefined\(未声明\)
* number\(数字\)
* string\(字符串\)
* object\(对象\)
* boolean\(布尔值\)
* symbol\(符号，es6新增\)

{% hint style="info" %}
js中只有这7种数据类型，其他的都是对象的子类型（数组不是js的数据类型）
{% endhint %}

typeof运算符后可跟着一个操作数，用于判断该操作数的数据类型；**typeof适用于上面7种数据类型中除null、object的其他5种类型的判断**：

```javascript
typeof undefined;  // "undefined";
typeof 1;          // "number";
typeof '1';        // "string";
typeof true;       // "boolean";
typeof Symbol();   // "symbol";
typeof {a : 1};    // "object";
```

以上都是可靠正确而且易于理解的，接下来我们一起看几个正确的，但似乎不是我们期望的结果的例子：

```text
typeof null === "object"; // true
typeof function () {} === "function"; // true
```

![](../.gitbook/assets/image%20%2844%29.png)

### 为什么 typeof null === 'object'

在 JavaScript 最初的实现中，JavaScript 中的值是由一个表示类型的标签和实际数据值表示的。对象的类型标签是 0。由于 null 代表的是空指针（大多数平台下值为 0x00），因此，null的类型标签也成为了 0，typeof null就错误的返回了"object"。

### 为什么 typeof function\(\){} === 'function'

函数是对象的子类型，按道理来说"typeof function \(\) {} === 'object'"才对，但是结果却是"function"。这是因为，**函数是一个“特殊的对象”，它特殊就特殊在它内部有一个属性\[\[call\]\]，这个属性使它变成一个“可调用的对象”。**而根据规范，实现了\[\[call\]\]属性的对象，typeof的结果是“function”。也因为这个，new出来的function对象也是返回function

```javascript
typeof new Function("console.log(123);") === 'function'; // true
```

## Object.prototype.toString 识别类型

```javascript
// 以下是11种：
var number = 1;          // [object Number]
var string = '123';      // [object String]
var boolean = true;      // [object Boolean]
var und = undefined;     // [object Undefined]
var nul = null;          // [object Null]
var obj = {a: 1}         // [object Object]
var array = [1, 2, 3];   // [object Array]
var date = new Date();   // [object Date]
var error = new Error(); // [object Error]
var reg = /a/g;          // [object RegExp]
var func = function a(){}; // [object Function]

function checkType() {
    for (var i = 0; i < arguments.length; i++) {
        console.log(Object.prototype.toString.call(arguments[i]))
    }
}

checkType(number, string, boolean, und, nul, obj, array, date, error, reg, func)
```

![](../.gitbook/assets/image%20%28123%29.png)

## 实现

{% hint style="info" %}
在 IE6 中，null 和 undefined 会被 Object.prototype.toString 识别成 \[object Object\]！
{% endhint %}

```javascript
// 第二版
var class2type = {};

// 生成class2type映射
"Boolean Number String Function Array Date RegExp Object Error".split(" ").map(function(item, index) {
    class2type["[object " + item + "]"] = item.toLowerCase();
})

function type(obj) {
    // 一箭双雕
    if (obj == null) {
        return obj + "";
    }
    return typeof obj === "object" || typeof obj === "function" ?
        class2type[Object.prototype.toString.call(obj)] || "object" :
        typeof obj;
}
```

## 参考

* [https://github.com/mqyqingfeng/Blog/issues/28](https://github.com/mqyqingfeng/Blog/issues/28)
* [https://zhuanlan.zhihu.com/p/38249035](https://zhuanlan.zhihu.com/p/38249035)

## 补充

### 判断是否为数组？

 `Array.isArray` 原生方法

