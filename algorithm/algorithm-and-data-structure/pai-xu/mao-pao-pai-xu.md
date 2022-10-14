# 冒泡排序 bubble sort

## 思路

遍历每一个子集，都相邻比较最大并交换

## 属性

* 稳定
* 时间复杂度 `O(n²)`
* 交换 `O(n²)`
* 对即将排序完成的数组进行排序 `O(n)`

## 核心概念

* 利用_交换，将最大的数冒泡到最后_
* 使用_缓存 `postion`_ 来优化
* 使用_双向遍历_来优化

## 实现

```javascript
function bubbleSort(arr){
  for(let i = 1; i < arr.length; i++){
    for(let j = 0; j <= i; i++){
      if(arr[j] > arr[j+1]){
      	const temp = arr[j]
        arr[j] = arr[j+1]
        arr[j+1] = temp
      }
    }
  }
  return arr
}
```

在比较交换的时候，就是计算机中最经典的交换策略，用临时变量`temp`保存值，但是面试官问过我，**ES6有没有简单的方法实现？** 有的，如下：

```javascript
arr2 = [1,2,3,4];
[arr2[0],arr2[1]] = [arr2[1],arr2[0]]  //ES6解构赋值
console.log(arr2)  // [2, 1, 3, 4]
```

所以，刚才的冒牌排序可以优化如下：

```javascript
function bubbleSort(arr){
  for(let i = 1; i < arr.length; i++){
    for(let j = 0; j <= i; i++){
    	if(arr[j] > arr[j+1]){
          [arr[j],arr[j+1]] = [arr[j+1],arr[j]]
      }
    }
  }
  return arr
}
```

## 优化1： 记录最后交换的位置

设置一标志性变量 `pos`,用于_记录每趟排序中最后一次进行交换的位置_。 由于 `pos` 位置之后的记录均已交换到位,故_在进行下一趟排序时只要扫描到 `pos` 位置即可_。

```javascript
function bubbleSort2(arr) {
  let i = arr.length - 1;

  while (i > 0) {
    let pos = 0;

    for (let j = 0; j < i; j++) {
      if (arr[j] > arr[j + 1]) {
        pos = j;
        [arr[j],arr[j+1]] = [arr[j+1],arr[j]]
      }
    }
    i = pos;
  }

  return arr;
}
```

## 优化2： 正反同时冒泡

传统冒泡排序中每一趟排序操作只能找到一个最大值或最小值, 我们可以 **在每趟排序中进行正向和反向两遍冒泡** ， 一次可以得到两个最终值（最大和最小） , 从而_使外排序趟数几乎减少了一半_。

```javascript
function bubbleSort3(arr) {
  let start = 0;
  let end = arr.length - 1;

  while (start < end) {
    for (let i = start; i < end; i++) {
      if (arr[i] > arr[i + 1]) {
        [arr[j],arr[j+1]] = [arr[j+1],arr[j]];
      }
    }
    end -= 1;
    for (let i = end; i > start; i--) {
      if (arr[i - 1] > arr[i]) {
        [arr[j],arr[j+1]] = [arr[j+1],arr[j]];
      }
    }
    start += 1;
  }

  return arr;
}

```
