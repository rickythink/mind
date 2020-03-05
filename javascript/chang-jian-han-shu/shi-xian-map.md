# 实现 map

```javascript
 Array.prototype.map = function (fn, context = this) {
      if (typeof fn !== 'function') {
        throw new TypeError(fn + 'is not a function')  
      }
      const arr = this
      const result = new Array(ary.length);
      
      for (let i = 0; i < arr.length; i++) {
        // fix稀疏数组的情况
        if (i in arr) {
            result[i] = fn.call(context, arr[i], i, arr);
        }
      }
      return result
  }
```

使用 reduce 来实现 map

```javascript
Array.prototype.map = function(fn, context = this){
    if (typeof fn !== 'function') {
      throw new TypeError(fn + 'is not a function'); 
    }
    return this.reduce((acc, value, index) => {
      // fix稀疏数组的情况
      if (index in list) {
        acc[index] = fn.call(context, value, index, this);
      }
      return acc;
    }, []); // 注意这里的[]不能省
}
```

