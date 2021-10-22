# Symbol

`Symbol` 是 ES6 引入的新的基本数据类型。`Symbol`类型是为了解决属性名冲突的问题，顺带还具备模拟私有属性的功能。

## 简介

创建`symbol`变量最简单的方法是用`Symbol()`函数。`sysmbol`变量有两点比较特别：

1. 它可以作为对象属性名。只有字符串和 `symbol` 类型才能用作对象属性名。
2. 没有两个`symbol` 的值是相等的。

```javascript
const symbol1 = Symbol();
const symbol2 = Symbol();

symbol1 === symbol2; // false

const obj = {};
obj[symbol1] = 'Hello';
obj[symbol2] = 'World';

obj[symbol1]; // 'Hello'
obj[symbol2]; // 'World'
```

{% hint style="danger" %}
尽管调用`Symbol()` 让它看起来像是对象，实际上`symbol`是 JavaScript 原始数据类型。把`Symbol`当作构造函数来用 `new`会报错。
{% endhint %}

## 参考

1. [https://juejin.im/post/5e89ba2ee51d4547052cf2fa](https://juejin.im/post/5e89ba2ee51d4547052cf2fa)
