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
  SuperClass.call(this,name);
  this.time = time;
}
SubClass.prototype = new SuperClass();

SubClass.prototype.getTime = function() {
  console.log(this.time);
}
```

如上，我们就解决了之前说到的一些问题，唯一遗憾的式`SuperClass`的构造函数执行了两遍就感觉非常的不妥.

