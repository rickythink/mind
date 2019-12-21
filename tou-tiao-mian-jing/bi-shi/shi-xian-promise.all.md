# 实现Promise.all

## 思路

1. 传入的一定是数组
2. Promise.resolve包裹\(元素不是 `Promise` 对象，则使用 `Promise.resolve` 转成 `Promise` 对象
3. 全部成功，返回resolve
4. 只要有一个失败，返回reject

## 代码

```javascript
function promiseAll(promises) {
  return new Promise(function(resolve, reject) {
    if (!isArray(promises)) {
      return reject(new TypeError('arguments must be an array'));
    }
    var resolvedCounter = 0;
    var promiseNum = promises.length;
    var resolvedValues = new Array(promiseNum);
    for (var i = 0; i < promiseNum; i++) {
      (function(i) {
        Promise.resolve(promises[i]).then(function(value) {
          resolvedCounter++
          resolvedValues[i] = value
          if (resolvedCounter == promiseNum) {
            return resolve(resolvedValues)
          }
        }, function(reason) {
          return reject(reason)
        })
      })(i)
    }
  })
}
```

