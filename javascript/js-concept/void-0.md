# void 0

## undefiend并不是保留词

undefined 并不是保留词（reserved word），它只是全局对象的一个属性，在低版本 IE 中能被重写。

```javascript
var undefined = 10;

// undefined -- chrome
// 10 -- IE 8
alert(undefined);
```

事实上，undefined 在 ES5 中已经是全局对象的一个只读（read-only）属性了，它不能被重写。但是在局部作用域中，还是可以被重写的。

```javascript
(function() {
  var undefined = 10;

  // 10 -- chrome
  alert(undefined);
})();

(function() {
  undefined = 10;

  // undefined -- chrome
  alert(undefined);
})();
```

## 为什么是void 0

void 运算符能对给定的表达式进行求值，然后返回 undefined。也就是说，void 后面你随便跟上一个表达式，返回的都是 undefined，都能完美代替 undefined！那么，这其中最短的是什么呢？毫无疑问就是 void 0 了。

