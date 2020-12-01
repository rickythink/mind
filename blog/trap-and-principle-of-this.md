# this 陷阱与原理

## 前言

this 在很多地方都有解释。听得最多的说法就是 this 是由**调用的位置**来决定的。话虽然没有错，但是什么是调用的位置，还是非常含糊不清。

另外，this 绑定中有

* 显式绑定
* 隐式绑定
* 默认绑定

又分别是什么？看定义的名称都非常的抽象。如果我们仅仅是面试前记住对应的概念，而不知道其中的原理，这种知识会非常容易遗忘。

## 陷阱

```javascript
const obj = {
	foo: function () {
		console.log(this)
	}
}

const foo = obj.foo

// 写法一
obj.foo()

// 写法二
foo()
```

答案:

写法一的 this 指向 obj 对象，但是写法二的obj指向 window 对象 / undefined

```javascript
{foo: ƒ}
Window ... // 严格模式下指向 undefined
```

很多教科书会说，`this`指的是函数运行时所在的环境。对于`obj.foo()`来说，`foo`运行在`obj`环境，所以`this`指向`obj`；对于`foo()`来说，`foo`运行在全局环境，所以`this`指向全局环境。所以，两者的运行结果不一样。

这种解释没错，但是教科书往往不告诉你，为什么会这样？也就是说，函数的运行环境到底是怎么决定的？举例来说，为什么`obj.foo()`就是在`obj`环境执行，而一旦`var foo = obj.foo`，`foo()`就变成在全局环境执行？

## 原理

定义的对象的 JS 过程

```javascript
const obj = { foo: 5 }
```

![](../.gitbook/assets/image%20%28209%29.png)

JavaScript 引擎会先在内存里面，生成一个对象{ foo: 5 }，然后把这个对象的内存地址赋值给变量obj

变量obj是一个地址（reference）。后面如果要读取obj.foo，引擎先从obj拿到内存地址，然后再从该地址读出原始的对象，返回它的foo属性。

原始的对象以字典结构保存，每一个属性名都对应一个属性描述对象。举例来说，上面例子的foo属性，实际上是以下面的形式保存的。

![](../.gitbook/assets/image%20%28210%29.png)

{% hint style="info" %}
在 ES5 中模拟实现 const，也即通过 Object.defineProperty 来改变 writable 属性
{% endhint %}

上面的例子中，value 一个数值，但是，value也可以是一个函数。

```javascript
const obj = { foo: function () {} }
```

![](../.gitbook/assets/image%20%28207%29.png)

### 实际示例

```javascript
var f = function () {
  console.log(this.x);
}

var x = 1;
var obj = {
  f: f,
  x: 2,
};

// 单独执行
f() // 1

// obj 环境执行
obj.f() // 2
```

#### 单独运行

![](../.gitbook/assets/image%20%28212%29.png)

#### 对象运行

![](../.gitbook/assets/image%20%28206%29.png)

## 总结

回到本文开头提出的问题，`obj.foo()`是通过obj找到foo，所以就是在obj环境执行。一旦`var foo = obj.foo`，变量foo就直接指向函数的地址，所以foo\(\)就变成在全局环境执行。

## 延伸1 - this 的优先级

{% hint style="info" %}
new &gt; call / apply / bind \(显示绑定\) &gt; obj call \(隐式绑定\)&gt; function call \(默认绑定\)
{% endhint %}

下面将阐述为什么有这样的优先级。

### call / apply

apply和call所做的事情都相同，那就是改变函数内部的this指向并调用它。

```javascript
const obj = {
  foo: function () {
    console.log(this)
  }
}

obj.foo() // obj

obj.foo.call(this) // window 

```

模拟实现 call

```javascript
Function.prototype.call = function (context, ...args) {
  // 检查调用```call```的对象是否为函数
  if (typeof this !== 'function') {
    throw new TypeError('not a function')
  }

  // 将函数作为传入的```context```对象的一个属性，调用该函数
  const fn = Symbol()
  context[fn] = this
  context[fn](...args)

  // 不要忘了调用之后删除该属性
  delete context[fn]
}
```

### new

new运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。new 关键字会进行如下的操作：

1. 创建一个空的简单JavaScript对象（即{}
2. 链接该对象（即设置该对象的构造函数）到另一个对象
3. 将步骤1新创建的对象作为this的上下文
4. 如果该函数没有返回对象，则返回this。

```javascript
function myNew (fn, ...args) {
  // 第一步，创建一个空的简单JavaScript对象（即{}）；
  let obj = {}

  // 第二步，原型链绑定
  fn.prototype !== null && (obj.__proto__ = fn.prototype)

  // 第三步，改变this指向并运行该函数
  let ret = fn.call(obj, ...args)

  // 第四步，如果该函数没有返回对象，则返回this
  // 别忘了 typeof null 也返回 'object' 的bug
  if ((typeof ret === 'object' || typeof ret === 'function') && ret !== null) {
    return ret 
  }
  return obj
}
```

## 延伸2 - class this 调用陷阱

😒 问题：对象解构后丢失 this 作用域

```javascript
class Demo {
    constructor(x, y) {
        this.x = x
        this.y = y
    }
    sum() {
        const sumVal = this.x + this.y
				console.log(sumVal)
        return sumVal
    }
}

const myDemo = new Demo(2, 3)

// 用法1 
myDemo.sum() // 5

// 用法2
const sum = myDemo.sum //将sum的引用赋值给新变量sum （写法等同于 const { sum } = myDemo;）
sum() // Uncaught TypeError: Cannot read property 'x' of undefined ||  window doesnt has property 'x'
```

😁  因此有这样的改写

```javascript
class Demo {
    constructor(x, y) {
    this.x = x;
    this.y = y;
    this.sum = this.sum.bind(this); // sum中this显式绑定
    }
    sum() {
        let sumVal = this.x + this.y;
        return sumVal;
    }
}

const myDemo = new Demo(2, 3);
const sum = myDemo.sum; 
sum() // 5
```

## 延伸3 - 回调函数的 this 陷阱

```javascript
function foo() {
  console.log( this.a )
}

function doFoo(task, callback) {
	console.log(task)
  callback()
}

const obj = {
  a: 2,
  foo: foo
}

var a = "oops, global";

// 写法1
doFoo('show a', obj.foo ) // "oops, global"

// 写法2
const { foo: myFoo } = obj
doFoo('show a again', myFoo ) // "oops, global"
```

## 箭头函数

{% hint style="info" %}
箭头函数不会创建自己的this,它只会从自己的作用域链的上一层继承this，并且形成绑定关系
{% endhint %}

{% hint style="info" %}
箭头函数中的this是在定义函数的时候绑定\(词法作用域\)，而不是在执行函数的时候绑定
{% endhint %}

```javascript
var obj = {
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log( this.i, this)
  },
	d: () => () => console.log(this.i, this),
  e: () => ({
    i:100,
    f: () => console.log(this.i, this)
  }),
  g: function() {
    const h = () => console.log(this.i, this)
    return h
  }
}

obj.b(); 
// undefined, Window{...}
obj.c(); 
// 10, Object {...}
obj.d()()
// undefined, Window{...}
obj.e().f()
// undefined, Window{...}
obj.g()()
// 10, Object {...}

obj.g().call(this)
// 10, Object {...}
obj.g.call(this)()
// undefined, Window{...}

```

于是 class 中的 this 调用改写

```javascript
class Demo {
  constructor(x, y) {
    this.x = x
    this.y = y
  }
  sum = () => {
    const sumVal = this.x + this.y;
    console.log(sumVal)
    return sumVal
  }
}

let myDemo = new Demo(2, 3)

// 用法1 
myDemo.sum()  // 😎 5

// 用法2
const sum = myDemo.sum //将sum的引用赋值给新变量sum （写法等同于 const { sum } = myDemo;）
sum() // 😎 5 
```

对象方法中的 this 改写

```javascript
function foo() {
  console.log( this.a )
}

function doFoo(task, callback) {
	console.log(task)
  callback()
}

const obj = {
  a: 2,
  foo: () => foo.call(obj)
}

var a = "oops, global";

// 写法1
doFoo('show a', obj.foo ) // "oops, global"

// 写法2
const { foo: myFoo } = obj
doFoo('show a again', myFoo ) // "oops, global"
```

## 再次总结

1. this 指向当前运行环境下调用的对象
2. this 的执行可以被 call/apply/bind 改写
3. 箭头函数从句法上定义 this 指向，不能被 call/apply/bind 修改

