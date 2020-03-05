# 柯里化2

重要参考：[https://github.com/mqyqingfeng/Blog/issues/42](https://github.com/mqyqingfeng/Blog/issues/42)

> 用闭包把参数保存起来，当参数的数量足够执行函数了，就开始执行函数，有没有毛病

## 第一版

```javascript
var curry = function (fn) {
    // arguments[0]是fn, 这里是收集更多的参数
    // 注意，arguments是array-like object，所以要用array的方法
    var args = [].slice.call(arguments, 1);
    return function() {
        // 合并所有参数
        var newArgs = args.concat([].slice.call(arguments));
        return fn.apply(this, newArgs);
    };
};


//应用
function add(a, b) {
    return a + b;
}

var addCurry = curry(add, 1, 2);
addCurry() // 3
//或者
var addCurry = curry(add, 1);
addCurry(2) // 3
//或者
var addCurry = curry(add);
addCurry(1, 2) // 3
```

## 第二版

```javascript
function sub_curry(fn) {
    var args = [].slice.call(arguments, 1);
    return function() {
        return fn.apply(this, args.concat([].slice.call(arguments)));
    };
}

function curry(fn, length) {

    length = length || fn.length;

    var slice = Array.prototype.slice;

    return function() {
        if (arguments.length < length) {
            var combined = [fn].concat(slice.call(arguments));
            return curry(sub_curry.apply(this, combined), length - arguments.length);
        } else {
            return fn.apply(this, arguments);
        }
    };
}

// 应用
var fn = curry(function(a, b, c) {
    return [a, b, c];
});

fn("a", "b", "c") // ["a", "b", "c"]
fn("a", "b")("c") // ["a", "b", "c"]
fn("a")("b")("c") // ["a", "b", "c"]
fn("a")("b", "c") // ["a", "b", "c"]
```

更易懂的实现

```javascript
function curry(fn, args) {
    var length = fn.length;
    args = args || [];
    return function() {
        var _args = args.slice(0), arg, i;
        for (i = 0; i < arguments.length; i++) {
            arg = arguments[i];
            _args.push(arg);
        }
        if (_args.length < length) {
            return curry.call(this, fn, _args);
        }
        else {
            return fn.apply(this, _args);
        }
    }
}
java

var fn = curry(function(a, b, c) {
    console.log([a, b, c]);
});

fn("a", "b", "c") // ["a", "b", "c"]
fn("a", "b")("c") // ["a", "b", "c"]
fn("a")("b")("c") // ["a", "b", "c"]
fn("a")("b", "c") // ["a", "b", "c"]
```

## 超简洁写法

```javascript
var curry = fn =>
    judge = (...args) =>
        args.length === fn.length
            ? fn(...args)
            : (arg) => judge(...args, arg)
```

{% hint style="info" %}
参数不满足长度要求时，返回的函数仍需使用rest parameter：`(...arg) => judge(...args, ...arg)`，否则除了第一次可以传递多个参数，后面只能一个一个传到足够为止了，不能直接处理fn\(1,2\)\(3,4,5\)的情况。
{% endhint %}

## 超超简洁写法

```javascript
var curry = (fn, ...args) =>
        fn.length <= args.length
            ? fn(...args)
            : curry.bind(null, fn, ...args)
var fn = curry(function(a, b, c){
  console.log([a,b,c])
})

fn(1,2,3)
fn(1,2)(3)
fn(1)(2,3)
fn(1)(2)(3)

// [ 1, 2, 3 ]
// [ 1, 2, 3 ]
// [ 1, 2, 3 ]
// [ 1, 2, 3 ]
```



