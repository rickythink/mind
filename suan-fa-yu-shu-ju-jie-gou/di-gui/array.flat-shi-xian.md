# Array.flat 实现

```javascript
var arr1 = [1, 2, [3, 4]];
arr1.flat(); 
// [1, 2, 3, 4]
```

## 解法

1. 注意第一次写上来就用了map, 是不可以的，原因是输出只有一层\[ \]，中间的迭代不能产生数组

```javascript
Array.prototype.flat = function() {
    var arr = [];
    this.forEach((item,idx) => {
        if(Array.isArray(item)) {
            arr = arr.concat(item.flat()); //递归去处理数组元素
        } else {
            arr.push(item)   //非数组直接push进去
        }
    })
    return arr;   //递归出口
}
```

