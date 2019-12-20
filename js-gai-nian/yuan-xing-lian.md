# 原型链

有用的参考：

{% embed url="https://github.com/mqyqingfeng/Blog/issues/2" %}

![](../.gitbook/assets/image%20%287%29.png)



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

