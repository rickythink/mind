# Event-loop

## 执行顺序

![](../.gitbook/assets/image%20%2864%29.png)

![](../.gitbook/assets/image%20%28179%29.png)

> 一个掘金的老哥（ssssyoki）的文章摘要： 那么如此看来我给的答案还是对的。但是js异步有一个机制，就是遇到宏任务，先执行宏任务，将宏任务放入eventqueue，然后在执行微任务，将微任务放入eventqueue最骚的是，这两个queue不是一个queue。当你往外拿的时候先从微任务里拿这个回掉函数，然后再从宏任务的queue上拿宏任务的回掉函数。 我当时看到这我就服了还有这种骚操作。

* 而宏任务一般是：包括整体代码script，setTimeout，setInterval、setImmediate、requestAnimationFrame。
* 微任务：原生Promise\(有些实现的promise将then方法放到了宏任务中\)、process.nextTick、Object.observe\(已废弃\)、 MutationObserver 记住就行了。

setTimeout和setInterval的运行机制是，将指定的代码移出本次执行，等到下一轮Event Loop时，再检查是否到了指定时间。如果到了，就执行对应的代码；如果不到，就等到再下一轮Event Loop时重新判断。这意味着，setTimeout指定的代码，必须等到本次执行的所有代码都执行完，才会执行。

每一轮Event Loop时，都会将“任务队列”中需要执行的任务，一次执行完。setTimeout和setInterval都是把任务添加到“任务队列”的尾部。因此，它们实际上要等到当前脚本的所有同步任务执行完，然后再等到本次Event Loop的“任务队列”的所有任务执行完，才会开始执行。由于前面的任务到底需要多少时间执行完，是不确定的，所以没有办法保证，setTimeout和setInterval指定的任务，一定会按照预定时间执行。

## 宏任务

| \# | 浏览器 | Node |
| :--- | :--- | :--- |
| `I/O` | ✅ | ✅ |
| `setTimeout` | ✅ | ✅ |
| `setInterval` | ✅ | ✅ |
| `setImmediate` | ❌ | ✅ |
| `requestAnimationFrame` | ✅ | ❌ |

## 微任务

| \# | 浏览器 | Node |
| :--- | :--- | :--- |
| `process.nextTick` | ❌ | ✅ |
| `MutationObserver` | ✅ | ❌ |
| `Promise.then catch finally` | ✅ | ✅ |

## 题目

```javascript
async function async1() {
  console.log(1);
  const result = await async2();
  console.log(3);
}

async function async2() {
  console.log(2);
}

Promise.resolve().then(() => {
  console.log(4);
});

setTimeout(() => {
  console.log(5);
});

async1();
console.log(6);
```

答案是\[1,2,6,4,3,5\]。这道题目主要考对JS**宏任务**和**微任务**的理解程度，JS的事件循环中每个宏任务称为一个**Tick**\(标记\)，在每个标记的末尾会追加一个微任务队列，一个宏任务执行完后会执行所有的微任务，直到队列清空。上题中我觉得稍微复杂点的在于async1函数，async1函数本身会返回一个Promise，同时await后面紧跟着async2函数返回的Promise，`console.log(3)`其实是在async2函数返回的Promise的then语句中执行的，then语句本身也会返回一个Promise然后追加到微任务队列中，所以在微任务队列中`console.log(3)`在`console.log(4)`后面

## await

遇到await会同步执行await后面的函数，挂起await下面的为微任务

```javascript
setTimeout(function () {
  console.log('6')
}, 0)
console.log('1')
async function async1() {
  console.log('2')
  await async2()
  console.log('5')
}
async function async2() {
  console.log('3')
}
async1()
console.log('4')
```

1. 6是宏任务在下一轮事件循环执行
2. 先同步输出1，然后调用了async1\(\)，输出2。
3. await async2\(\) 会先运行async2\(\)，5进入等待状态。
4. 输出3，这个时候先执行async函数外的同步代码输出4。
5. 最后await拿到等待的结果继续往下执行输出5。
6. 进入第二轮事件循环输出6。

## promise

### Promise新建后会立即执行

```javascript
let promise = new Promise(function(resolve, reject) {
    consoloe.log('Promise')
    resolve()
})

promise.then(function() {
    consoloe.log('Resolved.')
})

console.log('Hi!')

// Promise
// Hi!
// Resolved
```

### 调用`resolve`或`reject`并不会终结 Promise 的参数函数的执行。

```javascript
new Promise((resolve, reject) => {
  resolve(1);
  console.log(2);
}).then(r => {
  console.log(r);
});
// 2
// 1
```

一般来说，调用`resolve`或`reject`以后，Promise 的使命就完成了，后继操作应该放到`then`方法里面，而不应该直接写在`resolve`或`reject`的后面。所以，最好在它们前面加上`return`语句，这样就不会有意外。

```javascript
new Promise((resolve, reject) => {
  return resolve(1);
  // 后面的语句不会执行
  console.log(2);
})
```

## 浏览器操作与requestAnimationFrame

假设有这样的一些`DOM`结构：

```markup
<style>
  #outer {
    padding: 20px;
    background: #616161;
  }

  #inner {
    width: 100px;
    height: 100px;
    background: #757575;
  }
</style>
<div id="outer">
  <div id="inner"></div>
</div>
```

```javascript
const $inner = document.querySelector('#inner')
const $outer = document.querySelector('#outer')

function handler () {
  console.log('click') // 直接输出

  Promise.resolve().then(_ => console.log('promise')) // 注册微任务

  setTimeout(_ => console.log('timeout')) // 注册宏任务

  requestAnimationFrame(_ => console.log('animationFrame')) // 注册宏任务

  $outer.setAttribute('data-random', Math.random()) // DOM属性修改，触发微任务
}

new MutationObserver(_ => {
  console.log('observer')
}).observe($outer, {
  attributes: true
})

$inner.addEventListener('click', handler)
$outer.addEventListener('click', handler)
```

如果点击`#inner`，其执行顺序一定是：`click` -&gt; `promise` -&gt; `observer` -&gt; `click` -&gt; `promise` -&gt; `observer` -&gt; `animationFrame` -&gt; `animationFrame` -&gt; `timeout` -&gt; `timeout`。

因为一次`I/O`创建了一个宏任务，也就是说在这次任务中会去触发`handler`。  
按照代码中的注释，在同步的代码已经执行完以后，这时就会去查看是否有微任务可以执行，然后发现了`Promise`和`MutationObserver`两个微任务，遂执行之。  
因为`click`事件会冒泡，所以对应的这次`I/O`会触发两次`handler`函数\(\_一次在`inner`、一次在`outer`\_\)，所以会优先执行冒泡的事件\(\_早于其他的宏任务\_\)，也就是说会重复上述的逻辑。  
在执行完同步代码与微任务以后，这时继续向后查找有木有宏任务。  
需要注意的一点是，因为我们触发了`setAttribute`，实际上修改了`DOM`的属性，这会导致页面的重绘，而这个`set`的操作是同步执行的，也就是说`requestAnimationFrame`的回调会早于`setTimeout`所执行。

  


