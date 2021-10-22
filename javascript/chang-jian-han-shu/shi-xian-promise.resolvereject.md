# 实现Promise.resolve/reject

```javascript
Promise.resolve = function (value) {
    return new Promise(function (resolve, reject) {
        resolve(value);
    })
};
```

```javascript
Promise.reject = function (reason) {
    return new Promise(function (resolve, reject) {
        reject(reason);
    })
};
```

\
