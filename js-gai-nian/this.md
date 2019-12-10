# This

`this`是在运行中绑定的，并不是在编写时绑定。它的上下文取决于函数调用时的各种条件。`this`的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。

当一个函数被调用时，会创建一个活动记录（执行上下文）。这个记录会包含函数在哪里被调用（调用栈）、函数的调用方式、传入的参数等信息。`this`就是这样一个属性，**它指向什么完全取决于在哪里被调用。**

在javascript中，函数的调用一共有4种方式**：**

1. Function Invocation Pattern
2. Method Invocation Pattern
3. Constructor Pattern
4. Apply Pattern

下面来分别解释一下四种调用模式。

## Function Invocation Pattern

诸如`foo()`的调用形式被称为Function Invocation Pattern，是函数最直接的使用形式，注意这里的foo是作为单独的变量出现，而不是属性。

```javascript
function foo() {
	console.log(this.a);
}

var a = 2;
foo(); // 2
```

{% hint style="info" %}
参数传递/间接引用也会有Function Invocation Pattern的场景出现
{% endhint %}

```javascript
function foo(){
	console.log(this.a);
}

var obj = {
	a: 2,
	foo: foo
}

var a = 'I am global'; //全局对象的属性

function doFoo(fn){
	// fn实际上引用的是foo 尽管代码显示传进来是obj.foo，但实际上，执行的是foo函数的本身
	// 这是一种参数传递的 Function 调用
	fn(); // <---调用位置
}

doFoo(obj.foo); // I am global

// 内置的函数也有参数传递特性
setTimeout( obj.foo, 100 ); // I am global
```

```javascript
function foo() { console.log(this.a) }

var a = 2; 
var o = { a: 3, foo: foo }; 
var p = { a: 4 };

o.foo(); // 3 
(p.foo = o.foo)(); // 2
```

赋值表达式 p.foo = o.foo 的 返回值 是目标函数的引用， 因此调用位置是 foo\(\) 而不是 p.foo\(\) 或者 o.foo\(\) 。

## Method Invocation Pattern

诸如`foo.bar()`的调用形式被称为Method Invocation Pattern，注意其特点是被调用的函数作为一个对象的属性出现，必然会有“.”或者“\[\]”这样的关键符号。在这种模式下，bar函数体中的this永远为“.”或“\[”前的那个对象，如上例中就一定是foo对象。

```javascript
function foo(){
	console.log(this.a);
}

var obj = {
	a: 2,
	foo: foo
};

obj.foo(); // 2
```

{% hint style="info" %}
对象属性引用链中只有上一层或者说最后一层在调用位置中起作用
{% endhint %}

```javascript
function foo() { 
    console.log( this.a ); 
}

var obj2 = { 
    a: 42, 
    foo: foo
};

var obj1 = { 
    a: 2, 
    obj2: obj2
};
​​
obj1.obj2.foo(); // 42
```

## Constructor Pattern

`new foo()`这种形式的调用被称为Constructor Pattern，其关键字`new`就很能说明问题，非常容易识别。 在这种模式下，foo函数内部的this永远是new foo\(\)返回的对象。

```javascript
function foo(a) {
	this.a = a;
}

var bar = new foo(2);

console.log(bar.a); //2
```

## Apply Pattern

`foo.call(thisObject)`和`foo.apply(thisObject)`的形式被称为Apply Pattern，使用了内置的`call`和`apply`函数。在这种模式下，`call`和`apply`的第一个参数就是foo函数体内的this，如果thisObject是`null`或`undefined`，那么会变成Global对象。

```javascript
function foo(){
	console.log(this.a);
}

var obj = {
	a : 2
};

foo.call(obj); // 2
// 通过 foo.call(..)，可以在调用 foo 时强制把它的 this 绑定到 obj 上

function foo(something){
	console.log(this.a, something);
	return this.a + something;
}

var obj = {
	a : 2
};

var bar = foo.bind(obj);
var b = bar(3); // 2 3

console.log(b); // 5
```

## 严格模式

如果使用严格模式（ strict mode ），那么全局对象将无法使用默认绑定，因此 全局this 会绑定 到 undefined 

## 优先级

1. **Constructor Pattern**
2. **Apply Pattern**
3. **Method Invocation Pattern**
4. **Function Invocation Pattern**

### Apply Pattern &gt; Method Invocation Pattern

```javascript
function foo() { console.log( this .a ); }

var obj1 = { a: 2, foo: foo };

var obj2 = { a: 3, foo: foo };

obj1.foo(); // 2 
obj2.foo(); // 3

obj1.foo.call( obj2 ); // 3 
obj2.foo.call( obj1 ); // 2
```

### Constructor Pattern &gt; Apply Pattern

```javascript
function foo(something) { this .a = something; }

var obj1 = { foo: foo };

var obj2 = {};

obj1.foo( 2 ); 
console.log( obj1.a ); // 2

obj1.foo.call( obj2, 3 ); 
console.log( obj2.a ); // 3

var bar = new obj1.foo( 4 ); 
console.log( obj1.a ); // 2 
console.log( bar.a ); // 4
```

### 结论

现在我们可以根据优先级来判断函数在某个调用位置应用的是哪条规则。可以按照下面的 顺序来进行判断：

1.函数是否在 new 中调用（ new 绑定）？如果是的话 this 绑定的是新创建的对象。

var bar = new foo\(\)

2.函数是否通过call, apply指定的对象?如果是的话，this 绑定的是指定的对象

var bar = foo.call\(obj2\)

3.函数是否在某个上下文对象中调用？如果是的话， this 绑定的是那个上 下文对象。

var bar = obj1.foo\(\)

4.如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined ，否则绑定到 全局对象。

var bar = foo\(\)

## 箭头函数的this

箭头函数是根据外层（函数或者全局）作用域来决定`this`

```javascript
function foo(){
//返回一个箭头函数
	return (a) => {
		//this继承自foo()
		console.log(this.a);
	}
}

var obj1 = {
	a: 2
}

var obj2 = {
	a: 3
}

var bar = foo.call(obj1);

bar.call(obj2); // 2，不是3！
```

`foo()`内部创建的箭头函数会捕获调用时`foo()`的`this`。由于`foo()`的`this`绑定到`obj1`，`bar`的`this`也会绑定到`obj1`，箭头函数的绑定无法被修改（`new`也不行）

## 题目

```javascript
class Foo {
    print = () => {
        console.log(this.x);
    }

    constructor() {
        this.x = 1;
    }
}

let foo = new Foo();
foo.print.call({x: 2}); // 1
```

```javascript
let Foo = {
    print: function(){
        console.log(this.x)
    },
    
    x: 1
}

Foo.print() // 1
Foo.print.call({x:2}) // 2
Foo.print.call({x:3}) // 3
```

