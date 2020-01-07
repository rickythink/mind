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

