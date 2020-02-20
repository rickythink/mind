# 上报

![](../../.gitbook/assets/image.png)

性能指标收集注意项：

1. 保证数据的准确性 
2. 尽量不影响应用的性能

## 上报方式

```javascript
window.addEventListener('unload', function () {
    // Collect Run data like before
    let rumData = new FormData()
    rumData.append("entries", JSON.stringify(performance.getEntries())
}, false)
```

## 错误监听

1）js error  
监听 window.onerror 事件  
2）promise reject 的异常  
监听 unhandledrejection 事件

```javascript
window.addEventListener("unhandledrejection", function (event) {
    console.warn("WARNING: Unhandled promise rejection. Shame on you! Reason: "
        + event.reason);
});
```

3）资源加载失败  
window.addEventListener\('error'\)  
4）网络请求失败  
重写 window.XMLHttpRequest 和 window.fetch 捕获请求错误  
5）iframe 异常  
window.frames\[0\].onerror  
6）window.console.error

