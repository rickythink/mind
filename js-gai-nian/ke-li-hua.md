# 柯里化

**柯里化**, 即 **Currying** 的音译。

**Currying** 为实现多参函数提供了一个递归降解的实现思路——**把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数。**

下面尝试对最简单的加法函数进行柯里化改造

```javascript
function add (x, y) {
  return (x + y)
}
```

## **第一层**

```javascript
function curriedAdd (x) {
  return function(y) {
    return x + y
  }
}
```

当然以上实现是有一些问题的：它并不通用，并且我们并不想通过重新编码函数本身的方式来实现 **Currying** 化。

## 第二层

```javascript
function currying (fn, ...args1) {
    return function (...args2) {
        return fn(...args1, ...args2)
    }
}

var increment = currying(add, 1)
```

在此实现中，`currying` 函数的返回值其实是一个接收剩余参数并且立即返回计算值的函数。**即它的返回值并没有自动被 Currying化 。所以我们可以通过递归来将 currying 的返回的函数也自动 Currying** 化。

## 第三层

```javascript
function trueCurrying(fn, ...args) {
    if (args.length >= fn.length) {
        return fn(...args)
    }
    return function (...args2) {
        return trueCurrying(fn, ...args, ...args2)
    }
}
```

以上的思想为

> **比较多次接受的参数总数与函数定义时的入参数量，当接受参数的数量大于或等于被 Currying 函数的传入参数数量时，就返回计算结果，否则返回一个继续接受参数的函数。**

```javascript
const curriedAdd = trueCurring(add)
curriedAdd(1,2) // 3
curriedAdd(1)(2) // 3
```

## 柯里化优势

1. 多参函数复用性
2. 函数式编程而生

## 柯里化劣势

1. 一些特性有其他解决方案

   如果我们只是想提前绑定参数，那么我们有很多好几个现成的选择，bind，箭头函数等，而且性能比Curring更好。



