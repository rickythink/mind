# 协程

进程和线程我们都知道，那么协程是什么呢？

**协程是一种比线程更加轻量级的存在**。可以把协程看成是跑在线程上的任务，**一个线程上可以存在多个协程，但是在线程上同时只能执行一个协程**，比如当前执行的是 A 协程，要启动 B 协程，那么 A 协程就需要将主线程的控制权交给 B 协程，这就体现在 A 协程暂停执行，B 协程恢复执行；同样，也可以从 B 协程中启动 A 协程。通常，**如果从 A 协程启动 B 协程，我们就把 A 协程称为 B 协程的父协程**。

正如一个进程可以拥有多个线程一样，一个线程也可以拥有多个协程。最重要的是，协程不是被操作系统内核所管理，而是完全由**程序所控制**（即在用户态执行）。这样带来的好处就是性能得到了很大的提升，不会像线程切换那样消耗资源。

```javascript
function* genDemo() {
  console.log("开始执行第一段")
  yield 'generator 2'

  console.log("开始执行第二段")
  yield 'generator 2'

  console.log("开始执行第三段")
  yield 'generator 2'

  console.log("执行结束")
  return 'generator 2'
}

console.log('main 0')
let gen = genDemo()
console.log(gen.next().value)
console.log('main 1')
console.log(gen.next().value)
console.log('main 2')
console.log(gen.next().value)
console.log('main 3')
console.log(gen.next().value)
console.log('main 4')
```

执行过程如下图所示，可以重点关注协程之间的切换\


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/366ae82a000d4b158397832df8016407\~tplv-k3u1fbpfcp-watermark.image)

从图中可以看出来协程的四点规则：

* 通过调用生成器函数 `genDemo` 来创建一个 **协程 `gen`**，创建之后，`gen` 协程并没有立即执行。
* 要让 `gen` 协程执行，**需要通过调用 `gen.next`**。
* 当协程正在执行的时候，可以 **通过 `yield` 关键字来暂停 `gen` 协程的执行**，并返回主要信息给父协程。
* 如果协程在执行期间，遇到了 `return` 关键字，那么 JS 引擎会结束当前协程，并将 `return` 后面的内容返回给父协程。

协程之间的切换：

* `gen` 协程和父协程是在主线程上交互执行的，并不是并发执行的，它们之前的切换是 **通过 `yield` 和 `gen.next` 来配合完成** 的。
* 当在 `gen` 协程中调用了 `yield` 方法时，JS 引擎会保存 `gen` 协程当前的调用栈信息，并恢复父协程的调用栈信息。同样，当在父协程中执行 `gen.next` 时，JS 引擎会保存父协程的调用栈信息，并恢复 `gen` 协程的调用栈信息。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3f53f90242af45e1a639e0cb19f0b4db\~tplv-k3u1fbpfcp-watermark.image)

其实在 JS 中，`Generator` 生成器就是协程的一种实现方式。\
