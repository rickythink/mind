# 归并排序

归并排序（MERGE-SORT）是利用**归并**的思想实现的排序方法，该算法采用经典的**分治**（divide-and-conquer）策略（分治法将问题**分**\(divide\)成一些小的问题然后递归求解，而**治\(conquer\)**的阶段则将分的阶段得到的各答案"修补"在一起，即分而治之\)。

![](../../.gitbook/assets/image%20%284%29.png)

### 分

不断的对数组进行二拆分

### 治

左右比较，小的先进新数组，用后一位与刚刚未进数组的比较

### 实现

```javascript
function merge(leftArr, rightArr){  
    var result = [];  
    while (leftArr.length > 0 && rightArr.length > 0){  
      if (leftArr[0] < rightArr[0])  
        result.push(leftArr.shift()); //把最小的最先取出，放到结果集中   
      else   
        result.push(rightArr.shift());  
    }   
    return result.concat(leftArr).concat(rightArr);  //剩下的就是合并，这样就排好序了  
}  

function mergeSort(array){  
    if (array.length == 1) return array;  
    var middle = Math.floor(array.length / 2);       //求出中点  
    var left = array.slice(0, middle);               //分割数组  
    var right = array.slice(middle);  
    return merge(mergeSort(left), mergeSort(right)); //递归合并与排序  
}  

var arr = mergeSort([32,12,56,78,76,45,36]);
console.log(arr);   // [12, 32, 36, 45, 56, 76, 78]
```

归并排序是稳定排序，它也是一种十分高效的排序，能利用完全二叉树特性的排序一般性能都不会太差。

从上文的图中可看出，每次合并操作的平均时间复杂度为O\(n\)，而完全二叉树的深度为\|log2n\|。总的平均时间复杂度为O\(nlogn\)。而且，归并排序的最好，最坏，平均时间复杂度均为O\(nlogn\)。

