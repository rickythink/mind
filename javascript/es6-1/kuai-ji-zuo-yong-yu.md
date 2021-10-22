# 块级作用域

## 函数与块级作用域

ES5规定，函数只能在顶层作用域和函数作用域（闭包）之中声明，不能在块级作用域中声明。

```javascript
// 情况一
if (true) {
  function f() {}
}

// 情况二
try {
  function f() {}
} catch(e) {
  // ...
}
```

上述的声明都是非法的。但是为了兼容旧的代码，上述情况实际都可以运行，不会报错。

```javascript
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
```

上例中结果会输出`I am inside`，实际函数会被提升到函数作用域头部，被ES5转化为

```javascript
// ES5 环境
function f() { console.log('I am outside!'); }
js
(function () {
  function f() { console.log('I am inside!'); }
  if (false) {
  }
  f();
}());
```

然而会在ES6环境下报错，而不是`I am outside!`，原因是**ES6环境中，块级作用域声明的函数，行为类似var声明的变量**，实际运行代码如下：

```javascript
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }
(function () {
  var f = undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```

因此，如果一定要在块级作用域中声明函数，应该写成函数表达式，而不是函数声明语句。

```javascript
// 块级作用域内部，优先使用函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

## 方括号与块级作用域

ES6的块级作用域必须有大括号，不然JavaScript就认为不存在块级作用域

```javascript
if (true) let x = 1;

// 第二种写法，不报错
if (true) {
  let x = 1;
}
```
