# 数组filter

filter方法经常用，实现起来也比较容易。需要注意的就是filter接收的参数依次为数组当前元素、数组index、整个数组，并返回结果为ture的元素。

```javascript
Array.prototype.filter = function(fn,context = this){
    if(typeof fn != 'function'){
        throw new TypeError(`${fn} is not a function`)
    }
    let arr = this;
    let reuslt = []
    for(var i = 0;i < arr.length; i++){
        let temp= fn.call(context,arr[i],i,arr);
        if(temp){
            result.push(arr[i]);
        }
    }
    return result
}
```



