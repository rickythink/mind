# 实现斐波那契数列

**所谓的斐波纳契数列是指**：

前 2 个数是 0 和 1 。

第 i 个数是第 i-1 个数和第 i-2 个数的和。

**斐波纳契数列的前 10 个数字是**：

```
    0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ...
```

#### 怎样算解成功： <a href="zen-yang-suan-jie-cheng-gong" id="zen-yang-suan-jie-cheng-gong"></a>

给定 1，返回 0

给定 2，返回 1

给定 10，返回 34

#### 题目分析： <a href="ti-mu-fen-xi" id="ti-mu-fen-xi"></a>

## 递归实现

```javascript
const f = (num) =>{
  if(num === 1) return 0
  if(num === 2) return 1
  return f(num-1) + f(num-2)
}
```

## 非递归实现

```javascript
const f = n => {
  let num = new Array(n).fill(0); // 初始化数组，并设置初始值
  num[1] = 1; // 设置第二个元素的值 推导第3个元素
  for (let i = 2; i <= n - 1; i++) {
    num[i] = num[i - 2] + num[i - 1]; // 遍历逐步推导元素值 数组完全符合数列不用进行判断等 运行效率最高。
  }
  return num[n - 1]; // 数组是从0开始计算 所以要减1
};
```
