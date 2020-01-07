# 原型链与new优先级

```javascript
function Foo() {
  getName = function() {
    alert(1);
  };
  return this;
}
Foo.getName = function() {
  alert(2);
};
Foo.prototype.getName = function() {
  alert(3);
};
var getName = function() {
  alert(4);
};
function getName() {
  alert(5);
}

//请写出以下输出结果：
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```

{% tabs %}
{% tab title="分析" %}
考察原型链，优先级，函数声明提升等
{% endtab %}

{% tab title="Second Tab" %}
Foo.getName\(\) 输出 2

getName\(\) 输出 4，原因是5是Function声明会自动提升，4会覆盖掉

Foo\(\).getName\(\) 是1

getName\(\) 第二次输出 1, 注意，这是因为两个getName，被上一步覆盖了

new Foo.getName\(\) 输出2

new Foo\(\).getName\(\) 输出 实际为 \(new Foo\(\)\).getName\(\) 为3

new new Foo\(\).getName\(\) 输出 new \(new Foo\(\)\).getName\(\)\) 依然为 3

这其中为什么 new Foo\(\) 后的实例是没有Foo\(\)中的getName函数的，如果把Foo改写一下。

```javascript
function Foo() {
  getName = function() {
    alert(1);
  };
  this.name = 'hello'
  return this;
}

const a = new Foo()

// a =>  Foo { name: 'hello' }
```

也就是说，**Foo\(\) 作为构造函数没有绑定到this中返回也是不在实例中可访问的**
{% endtab %}
{% endtabs %}



