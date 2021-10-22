# slice

## 定义

**`slice()`** 方法返回一个新的数组对象，这一对象是一个由 `begin` 和 `end` 决定的原数组的**浅拷贝**（包括 `begin`，不包括`end`）。原始数组不会被改变。

## 语法

```
arr.slice([begin[, end]])
```

`begin` 可选提取起始处的索引（从 `0` 开始），从该索引开始提取原数组元素。

* 如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，`slice(-2)` 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
* 如果省略 `begin`，则 `slice` 从索引 `0` 开始。
* **如果 `begin` 大于原数组的长度，则会返回空数组。**

`end` 可选提取终止处的索引（从 `0` 开始），在该索引处结束提取原数组元素。`slice` 会提取原数组中索引从 `begin` 到 `end` 的所有元素（包含 `begin`，但不包含 `end`）。

* `slice(1,4)` 会提取原数组中从第二个元素开始一直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。
* 如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 `slice(-2,-1)` 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。
* **如果 `end` 被省略，则 `slice` 会一直提取到原数组末尾。**
* **如果 `end` 大于数组的长度，`slice` 也会一直提取到原数组末尾。**

## 描述

**`slice` 不会修改原数组**，只会返回一个浅复制了原数组中的元素的一个新数组。

## 例子

```javascript
var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
var citrus = fruits.slice(1, 3);

// fruits contains ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
// citrus contains ['Orange','Lemon']
```

## 一个常见应用

很多时候会看见这样的的代码

```javascript
[xxx].slice()
```

这样可以拷贝生成一个新数组

## 另一个常见应用

```javascript
let args = [].slice.call(arguments, 1)
```

首先`slice` 返回里一个函数，`[]` 只是起到`这是Array的一个实例对象` 而已，也就是说`[].slice` 等于`Array.prototype.slice`&#x20;

{% hint style="info" %}
\[].slice的好处是，Array是可以重写的，并且这样写也比较简单
{% endhint %}

至于`call` 函数，再温习一遍。这是Javascript比较魔法的地方，不同对象之间可以**借用方法**运行。实例

```javascript
object1 = {
    name:'frank',
    greet:function(){
        alert('hello '+this.name)
    }
};

object2 = {
    name:'andy'
};

// Note that object2 has no greet method.
// But we may "borrow" from object1:

object1.greet.call(object2); // will show an alert with 'hello andy'
```

因此，这里就比较明显了。传入的`arguments` 从索引1到结尾的数据范围赋值给了`args`&#x20;
