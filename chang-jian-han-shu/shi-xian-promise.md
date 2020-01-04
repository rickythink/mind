# 实现Promise

> 参阅这个，层层递进，写得非常好：[https://zhuanlan.zhihu.com/p/58428287](https://zhuanlan.zhihu.com/p/58428287)

## 完整的实现

```javascript
//完整的实现
class Promise {
    callbacks = [];
    state = 'pending';//增加状态
    value = null;//保存结果
    constructor(fn) {
        fn(this._resolve.bind(this));
    }
    then(onFulfilled) {
        return new Promise(resolve => {
            this._handle({
                onFulfilled: onFulfilled || null,
                resolve: resolve
            });
        });
    }
    _handle(callback) {
        if (this.state === 'pending') {
            this.callbacks.push(callback);
            return;
        }
        //如果then中没有传递任何东西
        if (!callback.onFulfilled) {
            callback.resolve(this.value);
            return;
        }
        var ret = callback.onFulfilled(this.value);
        callback.resolve(ret);
    }
    _resolve(value) {

        if (value && (typeof value === 'object' || typeof value === 'function')) {
            var then = value.then;
            if (typeof then === 'function') {
                then.call(value, this._resolve.bind(this));
                return;
            }
        }

        this.state = 'fulfilled';//改变状态
        this.value = value;//保存结果
        this.callbacks.forEach(callback => this._handle(callback));
    }
} 
```

解释

1. then方法中，创建并返回了新的Promise实例，这是串行Promise的基础，是实现真正链式调用的根本。
2. handle方法是promise内部的方法。then方法传入的形参onFulfilled以及创建新Promise实例时传入的resolve均被push到当前promise的callbacks队列中，这是衔接当前promise和后邻promise的关键所在（这里一定要好好的分析下handle的作用）
3. resolve方法中会先检查value是不是Promise对象，如果是一个新的Promise，那么将当前promise的状态改变操作先不执行，添加当前的resolve到那个新Promise的then中，等到新的Promise状态改变时，改变当前的Promise状态。这里会比较绕，我们后面逐一通过例子去加深理解。

