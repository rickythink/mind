# 数组乱序

## 需求

给定一个数组，返回一个完全乱序过的数组

## 实现

Fisher–Yates Shuffle复杂度 O\(n\)。

其实它的思想非常的简单，遍历数组元素，将其与之前的任意元素交换。因为遍历有从前向后和从后往前两种方式，所以该算法大致也有两个版本的实现。

### 从后往前版本

```javascript
function shuffle(array) {
  var _array = array.concat();

  for (var i = _array.length; i--; ) {
    var j = Math.floor(Math.random() * (i + 1));
    var temp = _array[i];
    _array[i] = _array[j];
    _array[j] = temp;
  }

  return _array;
}
```

### 流传但不准确的版本 

另一个为人津津乐道的方法是 "巧妙应用" JavaScript 中的 Math.random\(\) 函数。

```javascript
function shuffle(a) {
  return a.concat().sort(function(a, b) {
    return Math.random() - 0.5;
  });
}
```

同样是 \[0, 1, 2 ... 10\] 作为初始值，同样跑了 1w 组 case，结果请猛戳 [http://hanzichi.github.io/test-case/shuffle/Math.random/](http://hanzichi.github.io/test-case/shuffle/Math.random/)。

看平均值的图表，很明显可以看到曲线浮动，而且多次刷新，折现的大致走向一致，平均值更是在 5 上下 0.4 的区间浮动。如果我们将 \[0, 1, 2 .. 9\] 作为初始数组，可以看到更加明显不符预期的结果（有兴趣的可以自己去试下）。究其原因，要追究 JavaScript 引擎对于**Array.prototype.sort**的实现原理，这里就不展开了（其实是我也不知道）。因为 ECMAScript 并没有规定 JavaScript 引擎对于 Array.prototype.sort 应该实现的方式，所以我猜想不同浏览器经过这样的乱序后，结果也不一样。

什么时候可以用这种方法乱序呢？"非正式" 场合，一些手写 DEMO 需要乱序的场合，这不失为一种 clever solution。

但是这种解法不但不正确，而且 sort 的复杂度，平均下来应该是 O\(nlogn\)，跟我们接下来要说的正解还是有不少差距的。

## 参考

1. [https://www.h5jun.com/post/array-shuffle.html](https://www.h5jun.com/post/array-shuffle.html)
2. [https://github.com/lessfish/underscore-analysis/issues/15](https://github.com/lessfish/underscore-analysis/issues/15)

