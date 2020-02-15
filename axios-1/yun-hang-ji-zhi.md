# 运行机制

![](../.gitbook/assets/image%20%2864%29.png)

## promise链

```javascript
// 组成`Promise`链
// Hook up interceptors middleware
// 把 xhr 请求 的 dispatchRequest 和 undefined 放在一个数组里
var chain = [dispatchRequest, undefined];
// 创建 Promise 实例
var promise = Promise.resolve(config);

// 遍历用户设置的请求拦截器 放到数组的 chain 前面
this.interceptors.request.forEach(function unshiftRequestInterceptors(interceptor) {
  chain.unshift(interceptor.fulfilled, interceptor.rejected);
});

// 遍历用户设置的响应拦截器 放到数组的 chain 后面
this.interceptors.response.forEach(function pushResponseInterceptors(interceptor) {
  chain.push(interceptor.fulfilled, interceptor.rejected);
});

// 遍历chain 数组，直到遍历 chain.length 为 0
while (chain.length) {
  // 两两对应移出来 放到 then 的两个参数里。
  promise = promise.then(chain.shift(), chain.shift());
}

return promise;
```

## 适配器模式

```javascript
function getDefaultAdapter() {
  var adapter;
  if (typeof XMLHttpRequest !== 'undefined') {
    // For browsers use XHR adapter
    adapter = require('./adapters/xhr');
  } else if (typeof process !== 'undefined' && Object.prototype.toString.call(process) === '[object process]') {
    // For node use HTTP adapter
    adapter = require('./adapters/http');
  }
  return adapter;
}
var defaults = {
  adapter: getDefaultAdapter(),
  // ...
};
```

## 取消

在各个阶段都检查有没有取消操作`throwIfCancellationRequested`

```javascript
function dispatchRequest(config){
  // 有可能是执行到这里就取消了，所以抛出错误会被err2 捕获到
  throwIfCancellationRequested(config);
  //  adapter xhr适配器
  return new Promise((resovle, reject) => {
    var request = new XMLHttpRequest();
    console.log('request', request);
    if (config.cancelToken) {
        // Handle cancellation
        config.cancelToken.promise.then(function onCanceled(cancel) {
            if (!request) {
                return;
            }

            request.abort();
            reject(cancel);
            // Clean up request
            request = null;
        });
    }
  })
  .then(function(res){
    // 有可能是执行到这里就才取消 取消的情况下执行这句
    throwIfCancellationRequested(config);
    console.log('res', res);
    return res;
  })
  .catch(function(reason){
    // 有可能是执行到这里就才取消 取消的情况下执行这句
    throwIfCancellationRequested(config);
    console.log('reason', reason);
    return Promise.reject(reason);
  });
}
```

