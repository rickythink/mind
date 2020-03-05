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
        const temp= fn.call(context,arr[i],i,arr);
        if(temp){
            result.push(arr[i]);
        }
    }
    return result
}
```

使用 reduce 来实现 filter

```javascript
Array.prototype.filter = function(fn, context = this){
    if (typeof fn !== 'function') {
      throw new TypeError(fn + 'is not a function'); 
    }
    return this.reduce((acc, value, index) => {
      // fix稀疏数组的情况
      if (index in list) {
        const ret = fn.call(context, value, index, this);
        if(ret) acc.push(ret)
      }
      return acc;
    }, []); // 注意这里的[]不能省
}
```

