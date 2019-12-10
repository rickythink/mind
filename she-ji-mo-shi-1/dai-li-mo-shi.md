# 代理模式

### 存代理

缓存代理比较好理解，它应用于一些计算量较大的场景里。在这种场景下，我们需要“用空间换时间”——当我们需要用到某个已经计算过的值的时候，不想再耗时进行二次计算，而是希望能从内存里去取出现成的计算结果。这种场景下，就需要一个代理来帮我们在进行计算的同时，进行计算结果的缓存了。

一个比较典型的例子，是对传入的参数进行求和：

```javascript
// addAll方法会对你传入的所有参数做求和操作
const addAll = function() {
    console.log('进行了一次新计算')
    let result = 0
    const len = arguments.length
    for(let i = 0; i < len; i++) {
        result += arguments[i]
    }
    return result
}

// 为求和方法创建代理
const proxyAddAll = (function(){
    // 求和结果的缓存池
    const resultCache = {}
    return function() {
        // 将入参转化为一个唯一的入参字符串
        const args = Array.prototype.join.call(arguments, ',')
        
        // 检查本次入参是否有对应的计算结果
        if(args in resultCache) {
            // 如果有，则返回缓存池里现成的结果
            return resultCache[args]
        }
        return resultCache[args] = addAll(...arguments)
    }
})()
```

我们把这个方法丢进控制台，尝试同一套入参两次，结果喜人：

![](https://user-gold-cdn.xitu.io/2019/4/3/169e33b9b6895e00?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

我们发现 proxyAddAll 针对重复的入参只会计算一次，这将大大节省计算过程中的时间开销。现在我们有 6 个入参，可能还看不出来，当我们针对大量入参、做反复计算时，缓存代理的优势将得到更充分的凸显。

