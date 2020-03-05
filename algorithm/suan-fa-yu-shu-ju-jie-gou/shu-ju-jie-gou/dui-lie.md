# 队列

队列数据结构是遵循先进先出的有序集合，例如 js 的任务队列

## 实现

```javascript
let Queue = (function() {
  let items = new WeakMap()
  class Queue {
    constructor () {
      items.set(this, [])
    }
    enqueue (v) { // 入列
      items.get(this).push(v)
    }
    dequeue () { // 出列
      return items.get(this).shift()
    }
    front () { // 获取当前队列首位
      return items.get(this)[0]
    }
    size () { // 栈长度
      return items.get(this).length
    }
    isEmpty () { // 栈是否为空
      return items.get(this).length === 0
    }
    clear () { // 清空栈
      items.get(this).length = 0
    }
  }
  return Queue
})()
```

实现方式跟栈数据结构一样，只是实现的具体的方法不同。出栈实现的是数组的`pop`，出列实现的是数组的`shift`。

