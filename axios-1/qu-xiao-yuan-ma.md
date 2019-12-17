# 取消源码

## 来源

> [https://juejin.im/post/5d501512518825159e3d7be6](https://juejin.im/post/5d501512518825159e3d7be6)

## 源码定义

```javascript
var Cancel = require('./Cancel');

function CancelToken(executor) {
  if (typeof executor !== 'function') {
    throw new TypeError('executor must be a function.');
  }

  var resolvePromise;

  // 创建一个Promise
  // 在调用cancel函数前该promise会一直处于pending状态
  this.promise = new Promise(function promiseExecutor(resolve) {
    resolvePromise = resolve;
  });

  var token = this;

  executor(function cancel(message) {
    // 判断是否已经取消请求了
    if (token.reason) {
      return;
    }

    // 创建取消请求的信息，并将信息添加到实例的reason属性上
    token.reason = new Cancel(message);
  
    // 结束this.promise的pending状态
    // 将this.promise状态设置为resolve
    resolvePromise(token.reason);
  });
}

// 判断该请求是否已经被取消的方法
CancelToken.prototype.throwIfRequested = function throwIfRequested() {
  if (this.reason) {
    throw this.reason;
  }
};

CancelToken.source = function source() {
  var cancel;
  // new CancelToken的时候，内部this.promise是pending状态
  // c = 内部的cancel 函数
  // 因此调用 cancel(msg) 时可以结束内部的this.promise
  var token = new CancelToken(function executor(c) {
    cancel = c;
  });
  return {
    token: token,
    cancel: cancel
  };
};

module.exports = CancelToken;
```

## 内部使用

```javascript
// /lib/adapters/xhr.js

request.open()

// ...省略

// 如果配置了cancelToken选项
if (config.cancelToken) {
  
  // 对CancelToken中创建的Promise添加成功的回调
  // 当调用CancelToken.source暴露的cancel函数时，回调会被触发
  config.cancelToken.promise.then(function onCanceled(cancel) {

    if (!request) {
      return;
    }

    // 取消xhr请求
    request.abort();
    
    // 将axios返回的promise，置为reject态
    reject(cancel);

    request = null;
  });
}

// ...省略

request.send()
```

## 总结机制

CancelToken，创建了一个额外的PromiseA，并将PromiseA挂载到config中，同时将该PromiseA的resolve方法暴露出去。我们在调用send方法前（发送请求前）添加对PromiseA的状态进行监听，当PromiseA的状态被修改，我们会在PromiseA的callback中取消请求，并且将axios返回的PromiseB的状态置为reject。从而达到取消请求的目的





