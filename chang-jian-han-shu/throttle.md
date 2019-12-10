---
description: 节流函数总结
---

# Throttle

## 定义

throttle和debounce非常像。唯一的区别就是，throttle会设置固定的时间间隔，即使事件还没有间断大于某个阈值，只要时间间隔到了，就会执行一次。

简单来说，throttle是在debounce最后会执行一次的基础上，穿插在中间固定时间间隔执行。

## ES6实现

```javascript
function throttle(fn, wait) {
    let timeout, lastTime = 0
    return (...args) =>{
        const currentTime = Date.now()
        if(current >= lastTime + wait){
            lastTime = current
            fn(...args)
        }else {
            clearTimeout(timeout)
            timeout = setTimout(()=> fn(...args), wait)
        }
    }
}
```

