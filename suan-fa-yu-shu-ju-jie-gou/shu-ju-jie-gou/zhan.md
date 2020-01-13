# 栈

栈和队列本质上也是数组，是较为特殊的两种。

栈是一种后入先出的有序集合，例如 js 的执行栈。

![](../../.gitbook/assets/image%20%28112%29.png)

## 实现

```javascript
let Stack = (function(){
  let items = new WeakMap()
  class Stack {
    constructor () {
      items.set(this, [])
    }
    pop () { // 出栈
      return items.get(this).pop()
    }
    push (v) { // 入栈
      items.get(this).push(v)
    }
    peek () { // 获取当前栈顶
      return items.get(this)[items.get(this).length - 1]
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
  return Stack
})()

```

`WeakMap` 只接受对象类型作为键名，并且键名是弱引用类型的。弱引用是一种容易被垃圾回收机制回收的类型，只要垃圾回收机制发现一个对象**只被**弱引用类型所引用，那么这个对象就会被回收。上面代码中的 `this` 本身是一种强引用类型，但是 `this` 又被当作 `WeakMap` 的弱引用键名。因此当 `this` 实例被销毁的时候，`this` 原先所指向的对象的内存地址只存在 `WeakMap` 的弱引用了，所以，`WeakMap` 中对应的键值对都会在垃圾回收机制运行的时候被销毁。

## 测试

![](../../.gitbook/assets/image%20%2833%29.png)

如图所示，变量`a、b`创建了两个栈实例，并分别往各自栈里添加了一个数字元素。这时闭包`items`中是存在两个栈的。然后把变量 `b` 设为 `null` ，这时变量 `b` 所创建的栈实例对象失去了强引用，只剩下 `WeakMap` 键名的弱引用，但是打印出来的`items` 中仍然存在两个栈，因为此时垃圾回收机制没有运行。过了十秒之后，chrome 浏览器的垃圾回收机制触发，便把`items` 中的失效的栈清除了，此时打印出来 `itmes` 中只剩下一个栈了。（P.S. 经测试发现目前版本的V8引擎垃圾回收机制至多十秒就清一次啊）  


