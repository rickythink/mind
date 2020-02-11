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

