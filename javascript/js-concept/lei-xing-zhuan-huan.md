# 类型转换

JS中类型转换只有三种情况，分别是：

* 转换为布尔值
* 转换为数字
* 转换为字符串

### 转换为Boolean

在条件判断时，除了 `undefined`， `null`， `false`， `NaN`， `''`， `0`， `-0`，其他所有值都转为 `true`，包括所有对象。

### 转为数字\|字符串

见表

{% hint style="info" %}
注意图中有一个错误，Boolean 转字符串这行结果我指的是 true 转字符串的例子，不是说 Boolean、函数、Symblo 转字符串都是 `true`
{% endhint %}

![](../../.gitbook/assets/image%20%2881%29.png)

#### 对象转原始类型

1. 如果Symbol.toPrimitive\(\)方法，优先调用再返回
2. 调用valueOf\(\)，如果转换为原始类型，则返回
3. 调用toString\(\)，如果转换为原始类型，则返回
4. 如果都没有返回原始类型，会报错

```javascript
var obj = {
  value: 3,
  valueOf() {
    return 4;
  },
  toString() {
    return '5'
  },
  [Symbol.toPrimitive]() {
    return 6
  }
}
console.log(obj + 1); // 7
```

