# 原型链

有用的参考：

{% embed url="https://github.com/mqyqingfeng/Blog/issues/2" %}

## 上面参考中递进的图片实例

![&#x6784;&#x9020;&#x51FD;&#x6570;&#x548C;&#x5B9E;&#x4F8B;&#x539F;&#x578B;](../.gitbook/assets/image%20%28105%29.png)

![&#x6BCF;&#x4E00;&#x4E2A;JavaScript&#x5BF9;&#x8C61;\(&#x9664;&#x4E86; null \)&#x90FD;&#x5177;&#x6709;&#x7684;\_\_proto\_\_&#x5C5E;&#x6027; \| person.\_\_proto\_\_ === Person.prototype](../.gitbook/assets/image%20%2867%29.png)

![&#x6BCF;&#x4E2A;&#x539F;&#x578B;&#x90FD;&#x6709;&#x4E00;&#x4E2A; constructor &#x5C5E;&#x6027;&#x6307;&#x5411;&#x5173;&#x8054;&#x7684;&#x6784;&#x9020;&#x51FD;&#x6570; \| Person === Person.prototype.constructor](../.gitbook/assets/image%20%2882%29.png)

> 一个构造函数可以生成多个实例, 因此没有原型指向实例的箭头

![&#x539F;&#x578B;&#x7684;&#x539F;&#x578B; \| Person.prototype.\_\_proto\_\_ === Object.prototype](../.gitbook/assets/image%20%2825%29.png)

![&#x84DD;&#x8272;&#x7EBF;&#x5C31;&#x662F;&#x539F;&#x578B;&#x94FE; \| Object&#x7684;&#x539F;&#x578B;&#x7684;&#x539F;&#x578B;&#x4E3A;&#x7A7A; \| Object.prototype.\_\_proto\_\_ === null](../.gitbook/assets/image%20%28136%29.png)

## 真的是继承吗？

继承意味着复制操作，然而 JavaScript 默认并不会复制对象的属性，相反，JavaScript 只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些。

## 更全面的原型链图



![](../.gitbook/assets/image%20%2815%29.png)



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

## 只有函数有原型链

```javascript
let a = {}
let b = function () { }
console.log(a.prototype) // undefined
console.log(b.prototype) // { constructor: function(){...} }
```

## 不要再使用 \_\_proto\_\_ 

`__proto__`属性没有写入 ES6 的正文，而是写入了附录。

原因是它本质上是一个内部属性，而**不是一个正式的对外的 API，只是由于浏览器广泛支持，才被加入了 ES6**。

标准明确规定，只有浏览器必须部署这个属性，**其他运行环境不一定需要部署，而且新的代码最好认为这个属性是不存在的**。

### 推荐使用 Object.getPrototypeOf  Object.setPrototypeOf Object.create

所以无论从语义的角度，还是从兼容性的角度，都不要使用这个属性，应该使用：`Object.getPrototypeOf(target)`（读操作）、`Object.setPrototypeOf(target)`（写操作）、`Object.create(target)`（生成操作）代替



