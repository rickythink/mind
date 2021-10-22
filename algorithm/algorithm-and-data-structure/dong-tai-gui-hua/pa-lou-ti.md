# 爬楼梯

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？（给定 n 是一个正整数。）

#### 思路分析： <a href="si-lu-fen-xi" id="si-lu-fen-xi"></a>

这类题我们首先要来找其中的规律，找到了里面的规律，剩下的就好办了。

我再列举出几个结果：

```javascript
0 =0 0种方法
1 = 1 种方法
2 = 1+1 =2 2种方法
3=1+1+1=1+2=2+1 3种方法
4 = 1*4 = 1+2+1 = 1+1+2 = 2+1+1 = 2+2 5种方法
5 = 1*5 = 2+1+2 =2+2+1 = 1+2+2 =1+2+1+1 = 1+1+2+1 = 2+1+1+1 = 1+1+1+2 8种方法
```

#### 规律： <a href="gui-lv" id="gui-lv"></a>

这道题的规律实际上跟之前做的[查找斐波纳契数列中第 N 个数](http://obkoro1.com/web\_accumulate/algorithm/induction/%E6%9F%A5%E6%89%BE%E6%96%90%E6%B3%A2%E7%BA%B3%E5%A5%91%E6%95%B0%E5%88%97%E4%B8%AD%E7%AC%ACN%E4%B8%AA%E6%95%B0.html)中的规律有点类似。

斐波纳契数列中第 N 个数的规律

前 2 个数是 0 和 1,第 i 个数是第 i-1 个数和第 i-2 个数的和。

**本题中的规律是**：

除了 1 阶楼梯和 2 阶楼梯是一种和两种方法之外，第 n 阶的楼梯的方法是第 i-1 阶楼梯和第 i-2 阶楼梯所用方法的和。

```javascript
const climb = function(n) {
  // 前两个值的返回结果
  if (n === 1) return 1;
  if (n === 2) return 2;
  let a = 1, // 1阶楼梯
    b = 2, // 2阶楼梯
    c;
  for (let i = 3; i <= n; i++) {
    c = a + b; // n的结果
    // 为了后续推导，不断保存前两个值
    a = b;
    b = c;
  }
  return c;
};
```

es6 解构赋值交换变量

```javascript
const climb = function(n) {
  // 前两个值的返回结果
  if (n === 1) return 1;
  if (n === 2) return 2;
  let a = 1,
    b = 2;
  for (var i = 3; i <= n; i++) {
    [a, b] = [b, b + a];
  }
  return b;
}
```
