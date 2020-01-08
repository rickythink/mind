# Event-loop

## 执行顺序

![](../.gitbook/assets/image%20%2848%29.png)

![](../.gitbook/assets/image%20%28136%29.png)

> 一个掘金的老哥（ssssyoki）的文章摘要： 那么如此看来我给的答案还是对的。但是js异步有一个机制，就是遇到宏任务，先执行宏任务，将宏任务放入eventqueue，然后在执行微任务，将微任务放入eventqueue最骚的是，这两个queue不是一个queue。当你往外拿的时候先从微任务里拿这个回掉函数，然后再从宏任务的queue上拿宏任务的回掉函数。 我当时看到这我就服了还有这种骚操作。

* 而宏任务一般是：包括整体代码script，setTimeout，setInterval、setImmediate。
* 微任务：原生Promise\(有些实现的promise将then方法放到了宏任务中\)、process.nextTick、Object.observe\(已废弃\)、 MutationObserver 记住就行了。

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

  


