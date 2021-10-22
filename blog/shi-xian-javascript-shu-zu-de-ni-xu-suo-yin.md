---
description: 在 Python 和 Matlab 里面，我们可以通过 arr[-1] 的方式访问数组的最后一位，如何赋予 JavaScript 这种能力呢？
---

# 实现 JavaScript 数组的逆序索引

需求： 实现一种新的数组特性

```javascript
// 实现一种新的数组特性，完成数组的逆序访问
const arr = [1,2,3]
arr[-1] = 3
arr[-2] = 2
arr[-3] = 1
```

思路

首先我们不谈这道题是什么解决的，单纯了解一下 数组如何存储的。

数组( Array )在内存中用一串连续的区域来存放一些值。注意「连续」一词，它至关重要。

![](<../.gitbook/assets/image (205).png>)

上图表示存储在内存中的一个数组。存储4个4 bit 的元素,一共需要16 bit 存储区域，每个元素顺序保持一致。

假设，我声明了tinyInt arr\[4]，它占用从1201位置开始的一段存储区域。当某个时刻我尝试读取a\[2]，那么只要做简单的计算，找到a\[2]的位置就可以了。例如1201+(2X4)然后直接从1209位置读取数据。

However，JavaScript 的内存数据结构并不是连续的，而是链表结构。如果在 JavaScript 中声明var arr = new Array(4)；它会产生类似下图的结构。

![](<../.gitbook/assets/image (206).png>)

因此，如果你想在程序中某一处读取a\[2]，它必须从1201位置开始溯寻a\[2]的位置。

这就是 JavaScript 数组和真正的数组不同的地方

HowHowever,  以上只是简单科普一下 JavaScript Array 的数据结构。如果你准备构思如何改变 JavaScript 的链表结构来实现这道题。我只能祝你好运了。

真实的做法是利用 Proxy 或者 Object.defineProperty 来劫持 数组访问。直接上解决方案

```javascript
// proxy 写法
const NewArrProxy = {
  get (target, key, receiver) {
    let idx = key < 0 ? target.length + Number(key): key
    return Reflect.get(target, idx, receiver)
  }
}

const arr = new Proxy([1,2,3], NewArrProxy)
console.log(arr[-1])   // 3
console.log(arr[-2])   // 2
console.log(arr[-3])   // 1
```

```javascript
// object.definePropery 写法
const arr = [1,2,3]

function newArrObserver(arr){
  for (let idx = 1; idx <= arr.length; idx++) {
    Object.defineProperty(arr, `-${idx}`, {
      get() {
          return arr[arr.length - idx]
      }
    })
  }
}
newArrObserver(arr)

console.log(arr[-1])   // 3
console.log(arr[-2])   // 2
console.log(arr[-3])   // 1
```

惊不惊喜意不意外&#x20;

以上。
