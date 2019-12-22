# 柯里化2

重要参考：[https://github.com/mqyqingfeng/Blog/issues/42](https://github.com/mqyqingfeng/Blog/issues/42)

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



