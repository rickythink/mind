# 寄生继承

```javascript
function inheritObject(o) {
    //声明一个过渡对象
  function F() { }
  //过渡对象的原型继承父对象
  F.prototype = o;
  //返回过渡对象的实例，该对象的原型继承了父对象
  return new F();
}

function createBook(obj) {
    //通过原型方式创建新的对象
  var o = new inheritObject(obj);
  // 拓展新对象
  o.getName = function(name) {
    console.log(name)
  }
  // 返回拓展后的新对象
  return o;
}
```

其实寄生式继承就是对原型继承的拓展，一个二次封装的过程，这样新创建的对象不仅仅有父类的属性和方法，还新增了别的属性和方法。
