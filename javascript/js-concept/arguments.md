# arguments

`arguments`对象是所有（**非箭头**）函数中都可用的**局部变量**。

`arguments`对象不是一个 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array)&#x20;

```javascript
console.log(typeof arguments);    // 'object'
```

它类似于`Array`，但除了length属性和索引元素之外没有任何`Array`属性。例如，它没有 [pop](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global\_Objects/Array/pop) 方法。但是它可以被转换为一个真正的`Array`：

```javascript
var args = Array.prototype.slice.call(arguments);
var args = [].slice.call(arguments);

// ES6
const args = Array.from(arguments);
const args = [...arguments];
```
