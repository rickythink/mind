# 寄生组合

寄生组合式继承是寄生式继承和构造函数继承的组合。但是这里寄生式继承有些特殊，这里他处理不是对象，而是类的原型。

```javascript
// inheriteObject 其实就是Object.create的实现
function inheritObject(o) {
  //声明一个过渡对象
  function F() { }
  //过渡对象的原型继承父对象
  F.prototype = o;
  //返回过渡对象的实例，该对象的原型继承了父对象
  return new F();
}

function inheritPrototype(subClass,superClass) {
    // 复制一份父类的原型副本到变量中
  var p = inheritObject(superClass.prototype);
  // 修正因为重写子类的原型导致子类的constructor属性被修改
  p.constructor = subClass;
  // 设置子类原型
  subClass.prototype = p;
}
```

![&#x5BC4;&#x751F;&#x7EC4;&#x5408;&#x7EE7;&#x627F;&#x56FE;&#x793A;](../../.gitbook/assets/image%20%2823%29.png)

**我们需要继承的仅仅是父类的原型，不用去调用父类的构造函数**。换句话说，在构造函数继承中，我们已经调用了父类的构造函数。因此我们需要的就是父类的原型对象的一个副本，而这个副本我们可以通过原型继承拿到，但是这么直接赋值给子类会有问题，因为对父类原型对象复制得到的复制对象p中的`constructor`属性指向的不是`subClass`子类对象，因此在寄生式继承中要对复制对象p做一次增强，修复起`constructor`属性指向性不正确的问题，最后将得到的复制对象p赋值给子类原型，这样子类的原型就继承了父类的原型并且没有执行父类的构造函数。

综上，寄生组合只调用了一次超类构造函数，效率更高。避免在`SuberType.prototype`上面创建不必要的、多余的属性，与其同时，原型链还能保持不变。

有一点你需要注意，就是子类在想添加原型方法必须通过prototype来添加，否则直接赋予对象就会覆盖从父类原型继承的对象了.

```javascript
function SuperClass(name) {
  this.name = name;
  this.books=['js book','css book'];
}
SuperClass.prototype.getName = function() {
  console.log(this.name);
}
function SubClass(name,time) {
  SuperClass.call(this,name);
  this.time = time;
}
inheritPrototype(SubClass,SuperClass);
SubClass.prototype.getTime = function() {
  console.log(this.time);
}
var instance1 = new SubClass('React','2017/11/11')
var instance2 = new SubClass('Js','2018/22/33');

instance1.books.push('test book');

console.log(tance1.bostance1.books,instance2.books);
instance1.getName();
instance2.getTime();
```

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="输出" %}
instance1.books: \["js book", "css book", "test book"\]

instance2.books: \["js book", "css book"\]

instance1.getName\(\): 2017/11/11

instance2.getName\(\): 2018/22/33
{% endtab %}
{% endtabs %}







