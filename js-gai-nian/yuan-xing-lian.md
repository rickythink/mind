# 原型链

有用的参考：

{% embed url="https://github.com/mqyqingfeng/Blog/issues/2" %}

![](../.gitbook/assets/image%20%288%29.png)



要想理解这两张图，最好的做法是推演一下这几个表达式：

```javascript
Function.prototype instanceof Object // true
Function.prototype.__proto__ === Object.prototype // true

Object instanceof Object;//true 
Function instanceof Function; //true 

Number instanceof Number;//false 

Function instanceof Object;//true 
 
Foo instanceof Object; //true
Foo instanceof Function;//true 
Foo instanceof Foo;//false
```

## instanceof

**`instanceof`** **运算符**用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

```javascript
// 模拟实现 右边变量的原型存在于左边变量的原型链上
function instanceOf(left, right) {
  let leftValue = left.__proto__
  let rightValue = right.prototype
  while (true) {
    if (leftValue === null) {
      returnfalse
    }
    if (leftValue === right) {
      returntrue
    }
    leftValue = leftValue.__proto__
  }
}
```

