# 冒泡排序

两两比较呼唤

```javascript
function bubleSort(arr) {
    var len = arr.length;
    for (let outer = len ; outer >= 2; outer--) {
        for(let inner = 0; inner <=outer - 1; inner++) {
            if(arr[inner] > arr[inner + 1]) {
                let temp = arr[inner];
                arr[inner] = arr[inner + 1];
                arr[inner + 1] = temp;
            }
        }
    }
    return arr;
}
```

这里有两点需要注意： 

1. 外层循环，从最大值开始递减，因为内层是两两比较，因此最外层当&gt;=2时即可停止； 
2. 内层是两两比较，从0开始，比较inner与inner+1，因此，临界条件是inner&lt;outer -1

在比较交换的时候，就是计算机中最经典的交换策略，用临时变量`temp`保存值，但是面试官问过我，**ES6有没有简单的方法实现？** 有的，如下：

```javascript
arr2 = [1,2,3,4];
[arr2[0],arr2[1]] = [arr2[1],arr2[0]]  //ES6解构赋值
console.log(arr2)  // [2, 1, 3, 4]
```

所以，刚才的冒牌排序可以优化如下：

```javascript
function bubleSort(arr) {
    var len = arr.length;
    for (let outer = len ; outer >= 2; outer--) {
        for(let inner = 0; inner <=outer - 1; inner++) {
            if(arr[inner] > arr[inner + 1]) {
                [arr[inner],arr[inner+1]] = [arr[inner+1],arr[inner]]
            }
        }
    }
    return arr;
}
```

