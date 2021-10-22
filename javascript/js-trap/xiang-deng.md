# 相等

\==和 === 的区别在于， == 检查的是允许类型转换情况下的值的相等性，而 === 检查不允许类型转换 情况下的值的相等性；因此， === 经常被称为“严格相等”。

不等 != 与 == 对应， !== 与 === 对应。

```javascript
var a = "42";
var b = 42;

a == b;  // true
a === b; // false
```

{% hint style="info" %}
如果是比较两个非原生值的话，比如对象（包括函数和数组），那么你需要特殊注意 == 与 === 这些比较规则。因为这些值通常是通过引用访问的，所以 == 和 === 比较只是简单地检 查这些引用是否匹配，而完全不关心其引用的值是什么。

举例来说，通过简单地在元素之间插入逗号（ , ），数组在默认情况下会转换为字符串。你 可能会认为内容相同的两个数组也会 == 相等，但并非如此
{% endhint %}

```javascript
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c; // true
b == c; // true
a == b; // false
```

## \[ ] == !\[ ]

等价于&#x20;

```javascript
var a = []
var b = []
var c = ![] // => false

a == c // => a == false
[] == false 
```

这里记住不同类型的相等比较，顺序是

1. 首先尝试使用valueOf()
2. 再尝试使用toString()
3. 最后尝试toNumber()

整理就是

1. 隐式调用Boolean转型函数，对空数组转换成Boolean值，再对结果取反。此时比较\[] == false
2. 隐式调用Number转型函数，将false转换为数值0，此时比较\[] == 0
3. 调用valueOf方法和toString方法，此时\[].toString() 为空字符串，比较 '' == 0
4. 隐式调用Number转型函数，将空字符串转换为0，比较 0 == 0
5. 最后返回true

所以 \[ ] == false 为true

## {} == !{} / {} == false

分析与\[]的前两步都相同，但是{}.toString()会转换为'\[object Object]'，因此第4步会比较'\[object Object]' == 0,这必然为 false

## \[ ] == \[ ] / \[ ] == { }

值为 false

这里因为两边对比都是对象时，比较的是两个引用值在内存中是否是同一个对象

## **(a ==1 && a== 2 && a==3) 可能为 true 吗**

这里也在考察隐式变换，== 能把隐式转换 == 两边的类型相等

### 方法1

```javascript
const a = {
  i: 1,
  toString: function () {
    return a.i++;
  }
}
```

### 方法2

```javascript
// 利用数据劫持(proxy/Object.defineProperty)
let i = 1;
let a = new Proxy({}, {
    i: 1,
    get: function() {
        return () => this.i++;
    }
})
```

### 方法3

```javascript
// 数组的toString接口默认调用数组的join方法，重新join方法
let a = [1,2,3];
a.join = a.shift；
```

