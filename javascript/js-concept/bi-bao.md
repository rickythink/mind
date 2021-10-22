# 闭包

## 定义

闭包是指那些能够访问自由变量的函数。

自由变量是指在函数中使用的，但既不是函数参数也不是函数的局部变量的变量。

```javascript
var a = 1;

function foo() {
    console.log(a);
}

foo();
```

上例函数 foo + foo 函数访问的自由变量 a也算是闭包。所以在《JavaScript权威指南》中就讲到：**从技术的角度讲，所有的JavaScript函数都是闭包**。

但是从实践上意义的闭包定义为：

1. 即使创建它的上下文已经销毁，它仍然存在（比如，内部函数从父函数中返回）
2. 在代码中引用了自由变量

## 闭包的例子

```javascript
function foo() {
    var a = 2; 
    function bar() { 
        console.log( a ); 
    } 
    
    return bar;
} 

var baz = foo(); 
baz(); // 2 —— 朋友，这就是闭包的效果。
```

在 foo() 执行后，通常会期待 foo() 的整个内部作用域都被销毁，因为我们知道引擎有垃 圾回收器用来释放不再使用的内存空间。由于看上去 foo() 的内容不会再被使用，所以很 自然地会考虑对其进行 回收 。

而闭包的“神奇”之处正是可以阻止这件事情的发生。事实上内部作用域 没有被回收。谁在使用这个内部作用域？原来是 bar() 本身在使用。

拜 bar() 所声明的位置所赐，它拥有涵盖 foo() 内部作用域的闭包，使得该作用域能够一 直存活，以供 bar() 在之后任何时间进行引用。

bar() 依然持有对该作用域的引用，而这个引用就叫作闭包。

## 陷阱1

```javascript
for ( var i=1; i<=5; i++) { 
    setTimeout( function timer() { 
        console.log( i ); 
    }, i*1000 ); 
}
```

上诉代码，最后只会连续的输出6，而不是1到5。

这背后的原因是根据作用域的工作原理，实际情况是尽管循环中的五个函数是在各个迭代中分别定义的， 但是**它们都 被封闭在一个共享的全局作用域中 ，因此实际上只有一个 i** 。

这样说的话， 当然所有函数共享一个 i 的引用。循环结构让我们误以为背后还有更复杂的机制在起作用，但实际上没有。**如果将延迟函数的回调重复定义五次，完全不使用循环， 那它同这段代码是完全等价的。**

{% hint style="info" %}
个人更多的理解是，参考event-loop机制，setTimeout是宏任务，不会被同步执行，会先注册。又由于在i\*1000所有同步执行完后，真正执行timer时，i值已经被改变了
{% endhint %}

解决问题的方式是，更改成立即执行函数

```javascript
for(var i=1;i<=5;i++){
    (function(j){
        setTimeout( function timer() { 
            console.log( j ); 
        }, j*1000 ); 
    })(i)
}
```

当然，更酷的是es6的写法：

```javascript
for ( let i=1; i<=5; i++) { 
    setTimeout( function timer() { 
        console.log( i ); 
    }, i*1000 ); 
}
```

## 陷阱2

```javascript
function Foo() {
    var i = 0;
    return function() {
        console.log(i++);
    }
}
 
var f1 = Foo(),
    f2 = Foo();
f1();
f1();
f2();
```



道题考察**闭包**和**引用类型对象**的知识点：\
1.一般来说函数执行完后它的局部变量就会随着函数调用结束被销毁，但是此题foo函数返回了一个匿名函数的引用（即一个**闭包**），它可以访问到foo()被调用产生的环境，而局部变量i一直处在这个环境中，只要一个环境有可能被访问到，它就不会被销毁，所以说闭包有延续变量作用域的功能。这就好理解为什么：

```javascript
f1();//0
f1();//1
```

2.我一开始认为f1和f2都=foo()是都指向了同一个function引用类型，所以顺理成章就会答错认为：

```javascript
f2();//2
```

但其实foo()返回的是一个匿名函数，所以f1,f2相当于指向了两个不同的函数对象，所以结果也就顺理成章的变为：

```javascript
f2();//0
```

### 注意

如果上面的代码改成下面，结果会改变

```javascript
const Foo = (function() {
    var i = 0;
    return function() {
        console.log(i++);
    }
})()
 
var f1 = Foo,
    f2 = Foo;
f1();  // 0
f1();  // 1
f2();  // 2
```

其本质区别为， Foo变成立即运行函数，已经生成了确定的匿名函数地址，再分别分配给 f1和 f2

## 闭包的应用场景

vue 源码中 全局Api中的$watch方法中使用闭包

```javascript
// 根据Vue的$watch原理实现的简易版$watch
Vue.prototype.$watch = function(exp, cb, options = {immediate: true, deep: false}) {
    let watcher = new Watcher(this, exp, cb, options);
    return () => {
        watcher.unWatch();
    };
}
```

应用

```javascript
let obj = {
	name: 'kiner',
	age: 20
};
 
//  监听obj.name的改变
let unWatchName = this.$watch("obj.name",function(newVal, oldVal){
	console.log(`新值：${newVal};旧值：${oldVal}；`);
});
 
// 取消监听，取消后，obj.name的改变不再会通知
unWatchName()
```

这样实现的巧妙之处在于，我们无须关心要调用谁去取消监听，你怎么监听的，他就给你返回一个取消监听的方法，直接调用这个方法取消就可以了。

## 闭包应用

### 节流

### 防抖

### 缓存

```javascript
const Id2NameMap = {};
const getNameById = function (id) {

  if (Id2NameMap[id]) return Id2NameMap[id];

  return new Promise(resolve => {
    mockGetNameById(id, function (name) {
      Id2NameMap[id] = name;
      resolve(name);
    })
  });
}
Promise.resolve(getNameById(id)).then(name => {
  console.log(name);
});
```

### Once

```javascript
/**
 * Ensure a function is called only once.
 */
export function once (fn: Function): Function {
  let called = false
  return function () {
    if (!called) {
      called = true
      fn.apply(this, arguments)
    }
  }
}
```
