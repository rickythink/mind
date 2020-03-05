# 适配器模式

在 [axios 的核心逻辑](https://github.com/axios/axios/blob/master/lib/core/Axios.js)中，我们可以注意到实际上派发请求的是 [dispatchRequest 方法](https://github.com/axios/axios/blob/master/lib/core/dispatchRequest.js)。该方法内部其实主要做了两件事：

1. 数据转换，转换请求体/响应体，可以理解为数据层面的适配；
2. 调用适配器。

调用适配器的逻辑如下：

```javascript
// 若用户未手动配置适配器，则使用默认的适配器
var adapter = config.adapter || defaults.adapter;
  
  // dispatchRequest方法的末尾调用的是适配器方法
  return adapter(config).then(function onAdapterResolution(response) {
    // 请求成功的回调
    throwIfCancellationRequested(config);

    // 转换响应体
    response.data = transformData(
      response.data,
      response.headers,
      config.transformResponse
    );

    return response;
  }, function onAdapterRejection(reason) {
    // 请求失败的回调
    if (!isCancel(reason)) {
      throwIfCancellationRequested(config);

      // 转换响应体
      if (reason && reason.response) {
        reason.response.data = transformData(
          reason.response.data,
          reason.response.headers,
          config.transformResponse
        );
      }
    }

    return Promise.reject(reason);
  });
```

大家注意注释的第一行，“若用户未手动配置适配器，则使用默认的适配器”。手动配置适配器允许我们自定义处理请求，主要目的是为了使测试更轻松。

实际开发中，我们使用默认适配器的频率更高。默认适配器在[`axios/lib/default.js`](https://github.com/axios/axios/blob/master/lib/defaults.js)里是通过`getDefaultAdapter`方法来获取的：

```javascript
function getDefaultAdapter() {
  var adapter;
  // 判断当前是否是node环境
  if (typeof process !== 'undefined' && Object.prototype.toString.call(process) === '[object process]') {
    // 如果是node环境，调用node专属的http适配器
    adapter = require('./adapters/http');
  } else if (typeof XMLHttpRequest !== 'undefined') {
    // 如果是浏览器环境，调用基于xhr的适配器
    adapter = require('./adapters/xhr');
  }
  return adapter;
}
```

我们再来看看 Node 的 http 适配器和 xhr 适配器大概长啥样：

> http 适配器：

```javascript
module.exports = function httpAdapter(config) {
  return new Promise(function dispatchHttpRequest(resolvePromise, rejectPromise) {
    // 具体逻辑
  }
}
```

> xhr 适配器：

```javascript
module.exports = function xhrAdapter(config) {
  return new Promise(function dispatchXhrRequest(resolve, reject) {
    // 具体逻辑
  }
}
```

具体逻辑啥样，咱们目前先不关心，有兴趣的同学，可以狠狠地点[这里](https://github.com/axios/axios/tree/master/lib/adapters)阅读源码。咱们现在就注意两个事儿：

* 两个适配器的入参都是 config；
* 两个适配器的出参都是一个 Promise。

