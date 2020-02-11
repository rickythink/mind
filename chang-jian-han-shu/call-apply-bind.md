# Call, Apply, Bind

Call, Apply 和 Bind 都能用来改变this的指向。它们之间的区别是：

* call 用来明确参数个数确定的情况
* apply 用在参数个数不明确的情况
* bind 是传递对象
* call和apply都会立即执行
* bind是创建一个新的函数副本

## Call 和 Apply

举例 获取数组中的最大值和最小值

```javascript
var  numbers = [5, 458 , 120 , -215 ]; 
var maxInNumbers = Math.max.apply(Math, numbers),   //458
    maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
```

 举例 面试题 定义一个 log 方法，让它可以代理 console.log 方法

```javascript
function log(){
  console.log.apply(console, arguments);
};
log(1);    //1
log(1,2);    //1 2
```

接下来的要求是给每一个 log 消息添加一个"\(app\)"的前辍

```javascript
function log(){
  var args = Array.prototype.slice.call(arguments);
  args.unshift('(app)');
 
  console.log.apply(console, args);
};
```

## Bind

bind的定义是，bind\(\)方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind\(\)方法的第一个参数作为 this，传入 bind\(\) 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

```javascript
var bar = function(){
console.log(this.x);
}
var foo = {
x:3
}
bar(); // undefined
var func = bar.bind(foo);
func(); // 3
```

## 对比

使用举例

```javascript
var obj = {
    x: 81,
};
 
var foo = {
    getX: function() {
        return this.x;
    }
}
 
console.log(foo.getX.bind(obj)());  //81
console.log(foo.getX.call(obj));    //81
console.log(foo.getX.apply(obj));   //81
```

三个输出的都是81，但是注意看使用 bind\(\) 方法的，他后面多了对括号。

也就是说，区别是，当你希望改变上下文环境之后并非立即执行，而是回调执行的时候，使用 bind\(\) 方法。而 apply/call 则会立即执行函数。

## 实现

### Bind

参考链接 

* [https://github.com/mqyqingfeng/Blog/issues/12](https://github.com/mqyqingfeng/Blog/issues/12)

#### ES5 实现

```javascript
Function.prototype.bind2 = function (context) {

    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);

    var fNOP = function () {};

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
}
```

#### ES6 实现

```javascript
// formerArgs 为传递给 bind 函数的第二个到之后的参数
Function.prototype.bind = function (ctx, ...formerArgs) {
    let _this = this
    
    // laterArgs 为传递给原函数的参数
    return (...laterArgs) => {
        // bind 函数的不定参数在原函数参数之前
        // formerArgs 本身就是数组，可以直接调用数组的 concat 方法
        // 无需借助 call 或 apply
        return _this.apply(ctx, formerArgs.concat(laterArgs))
    }
}
```

### Call

```javascript
// 第三版
Function.prototype.call2 = function (context) {
    var context = context || window; // 处理null就指向window
    context.fn = this;

    // 处理参数
    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

    var result = eval('context.fn(' + args +')');

    delete context.fn
    return result;
}
```

其中

```javascript
var args = [];
for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
}
```

最终的数组为：

```javascript
var args = [arguments[1], arguments[2], ...]
```

然后

```javascript
 var result = eval('context.fn(' + args +')');
```

在eval中，args 自动调用 args.toString\(\)方法，eval的效果如 jawil所说，最终的效果相当于：

```javascript
 var result = context.fn(arguments[1], arguments[2], ...);
```

### Apply

```javascript
Function.prototype.apply = function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;

    var result;
    if (!arr) {
        result = context.fn();
    }
    else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')
    }

    delete context.fn
    return result;
}
```



