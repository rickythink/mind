# 组合式继承

```javascript
function SuperClass(name) {
  this.name = name; 
  this.books = ['Js','CSS'];
}
SuperClass.prototype.getBooks = function() {
    console.log(this.books);
}
function SubClass(name,time) {
  SuperClass.call(this,name); // 拥有 name和 books 就相当于super
  this.time = time;
}
SubClass.prototype = new SuperClass();

SubClass.prototype.getTime = function() {
  console.log(this.time);
}
```

如上，我们就解决了之前说到的一些问题。

组合继承最大的问题是，无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。

{% hint style="info" %}
SuperClass.call 很重要

在 ES6 class 中，子类必须在 `constructor` 方法中调用 `super` 方法，否则新建实例时会报错。这是因为**子类没有自己的 `this` 对象，而是继承父类的 `this` 对象，然后对其进行加工。如果不调用 `super` 方法，子类就得不到 `this` 对象。**

也正是因为这个原因，在子类的构造函数中，只有调用 `super` 之后，才可以使用 `this` 关键字，否则会报错。
{% endhint %}

