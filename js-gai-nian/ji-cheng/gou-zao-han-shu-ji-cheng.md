# 构造函数继承

```javascript
function SuperClass(id) {
  this.books = ['js','css'];
  this.id = id;
}
SuperClass.prototype.showBooks = function() {
  console.log(this.books);
}
function SubClass(id) {
  //继承父类
  SuperClass.call(this,id);
}
//创建第一个子类实例
var instance1 = new SubClass(10);
//创建第二个子类实例
var instance2 = new SubClass(11);

instance1.books.push('html');
console.log(instance1)
console.log(instance2)
instance1.showBooks();//TypeError
```

`SuperClass.call(this,id)`当然就是构造函数继承的核心语句了.由于父类中给this绑定属性，因此子类自然也就继承父类的共有属性。由于这种类型的继承没有涉及到原型`prototype`，所以父类的原型方法自然不会被子类继承。

